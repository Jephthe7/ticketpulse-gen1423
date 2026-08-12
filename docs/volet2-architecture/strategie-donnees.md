# Stratégie de données — TicketPulse

## 1. Objectifs

La stratégie de données de TicketPulse vise à garantir la cohérence, l'intégrité, la disponibilité et la sécurité des données, particulièrement lors des périodes de forte demande.

Elle doit notamment :

* empêcher toute double-vente ;
* conserver un état fiable de l'inventaire ;
* supporter un trafic de lecture élevé ;
* garantir la cohérence des réservations concurrentes ;
* assurer l'intégrité des paiements ;
* permettre une récupération après incident ;
* permettre l'évolution progressive du système.

La réservation constitue la zone où la cohérence des données est prioritaire.

---

## 2. Source de vérité

Une base de données relationnelle transactionnelle constitue la source de vérité pour les données critiques de TicketPulse.

Elle contient notamment :

* les événements ;
* les salles et sièges ;
* les réservations ;
* les commandes ;
* les paiements ;
* les billets ;
* les données nécessaires aux utilisateurs.

Le choix d'une base relationnelle est motivé par les garanties transactionnelles ACID, les contraintes d'intégrité référentielle et les contraintes d'unicité nécessaires à la gestion de l'inventaire.

Si PostgreSQL est retenu dans l'architecture technique, il constitue le système de gestion de base de données utilisé pour cette couche.

PostgreSQL fournit notamment des mécanismes de transactions, d'isolation et de verrouillage permettant de gérer les accès concurrents aux données critiques [1][2].

---

## 3. Modèle logique

### Event

Attributs principaux :

* `id`
* `name`
* `venue_id`
* `starts_at`
* `status`

### Seat

Attributs principaux :

* `id`
* `event_id`
* `section`
* `row`
* `number`
* `status`

Une contrainte d'unicité est appliquée sur :

```text
UNIQUE(event_id, section, row, number)
```

Elle permet d'éviter qu'un même siège soit défini plusieurs fois pour un même événement.

### Reservation

Attributs principaux :

* `id`
* `event_id`
* `user_id`
* `status`
* `expires_at`
* `created_at`

Une réservation temporaire possède une date d'expiration permettant de libérer les sièges non confirmés.

### ReservationItem

Attributs principaux :

* `reservation_id`
* `seat_id`

Cette relation permet d'associer les sièges à une réservation.

Un siège ne peut pas être associé simultanément à deux réservations actives incompatibles.

### Payment

Attributs principaux :

* `id`
* `reservation_id`
* `provider_reference`
* `status`
* `created_at`

Aucune donnée bancaire sensible n'est stockée directement par TicketPulse.

Le système conserve principalement la référence fournie par le prestataire de paiement, l'état de la transaction et les informations nécessaires à l'audit.

### Ticket

Après confirmation du paiement :

1. la commande est finalisée ;
2. le siège passe à l'état `SOLD` ;
3. le billet est généré ;
4. un QR code est associé au billet.

---

## 4. Intégrité des données

L'intégrité des données repose sur plusieurs mécanismes :

* clés primaires ;
* clés étrangères ;
* contraintes `UNIQUE` ;
* contraintes `NOT NULL` ;
* contraintes `CHECK` ;
* validation au niveau applicatif ;
* transactions atomiques.

Les règles principales sont :

* un siège appartient à un seul événement ;
* un siège ne peut être vendu qu'une seule fois ;
* une réservation doit référencer un utilisateur et un événement valides ;
* une réservation expirée ne peut pas être finalisée comme une nouvelle réservation ;
* un paiement déjà traité ne doit pas être appliqué deux fois.

---

## 5. Réservation concurrente

La concurrence constitue l'un des principaux risques pour TicketPulse.

Supposons qu'un seul siège soit disponible et que deux utilisateurs tentent de le réserver simultanément.

Une implémentation non protégée pourrait produire :

```text
Utilisateur A → lit AVAILABLE
Utilisateur B → lit AVAILABLE
Utilisateur A → réserve
Utilisateur B → réserve
```

Le système aurait alors créé une double-vente.

Cette situation doit être empêchée par un contrôle transactionnel.

Le flux recommandé est :

```text
1. Démarrer une transaction
2. Vérifier l'état du siège
3. Verrouiller ou modifier atomiquement le siège
4. Créer la réservation temporaire
5. Valider la transaction
6. Initier le paiement
7. En cas de succès → passer à SOLD
8. En cas d'échec ou d'expiration → revenir à AVAILABLE
```

Avec PostgreSQL, les verrous au niveau des lignes et le mécanisme `SELECT ... FOR UPDATE` permettent notamment de protéger les lignes concernées contre des modifications concurrentes pendant une transaction [2].

Une lecture suivie d'une écriture non protégée est donc interdite pour les opérations critiques d'inventaire.

---

## 6. Transactions et isolation

Les opérations critiques de réservation doivent être réalisées dans une transaction.

Le principe est :

```text
BEGIN

    vérifier le siège
    verrouiller le siège
    créer la réservation
    modifier l'état du siège

COMMIT
```

En cas d'erreur :

```text
ROLLBACK
```

Le niveau d'isolation doit être choisi en fonction du besoin de cohérence et du niveau de concurrence.

PostgreSQL propose notamment les niveaux :

* `Read Committed` ;
* `Repeatable Read` ;
* `Serializable`.

`Read Committed` constitue le niveau par défaut dans PostgreSQL. Les niveaux plus stricts peuvent être utilisés lorsqu'une opération exige des garanties supplémentaires contre certaines anomalies de concurrence [1].

Le choix définitif du niveau d'isolation doit tenir compte du compromis entre cohérence et performance.

---

## 7. Expiration des réservations

Chaque réservation temporaire possède :

```text
expires_at
```

Lorsqu'une réservation expire, un traitement périodique libère les sièges associés.

Le traitement doit être idempotent.

Cela signifie qu'une même expiration exécutée plusieurs fois ne doit pas produire un état incorrect.

Exemple :

```text
AVAILABLE
   ↓
RESERVED
   ↓
Expiration
   ↓
AVAILABLE
```

Une réservation déjà expirée et libérée ne doit pas être libérée une seconde fois de manière incorrecte.

---

## 8. Paiement

Le paiement est confié à un prestataire externe spécialisé.

TicketPulse conserve :

* la référence de transaction ;
* le statut ;
* le montant nécessaire au suivi ;
* les informations d'audit pertinentes.

Les données bancaires sensibles ne sont pas conservées directement dans la base TicketPulse.

Les notifications provenant du prestataire de paiement doivent être traitées de manière idempotente.

Ainsi, si une même notification est reçue plusieurs fois, elle ne doit pas finaliser plusieurs fois la même réservation.

---

## 9. Cache

Un cache peut être utilisé afin de réduire la charge de lecture.

Il peut notamment contenir :

* les recherches d'événements ;
* les informations statiques ;
* la configuration des salles ;
* les informations publiques fréquemment consultées.

Cependant, le cache ne doit jamais être la source d'autorité pour confirmer une vente.

La base transactionnelle reste la source de vérité.

```text
Cache
  ↓
Accélération des lectures

Base de données
  ↓
Source de vérité
```

Une information temporairement obsolète dans le cache ne doit donc jamais permettre de vendre un siège déjà vendu.

---

## 10. Indexation et performance

Des index doivent être prévus sur les colonnes utilisées fréquemment dans les recherches, jointures et opérations critiques.

Exemples :

```text
Event.starts_at
Seat.event_id
Reservation.user_id
Reservation.event_id
Reservation.status
Payment.reservation_id
```

Les index doivent être ajoutés selon les requêtes réellement exécutées et les résultats des tests de performance.

Une multiplication excessive des index peut augmenter le coût des écritures.

Il faut donc rechercher un compromis entre performance de lecture et coût de maintenance des données.

---

## 11. Sécurité des données

La stratégie de sécurité repose notamment sur le principe du moindre privilège.

Le compte utilisé par l'application doit disposer uniquement des permissions nécessaires à son fonctionnement.

Les comptes administrateurs ne doivent pas être utilisés par l'application pour les opérations courantes [3].

Les informations sensibles telles que les identifiants de connexion à la base de données doivent être conservées dans un mécanisme sécurisé de gestion des secrets et non directement dans le code source.

Les communications entre les composants doivent être protégées par des mécanismes de chiffrement appropriés.

Les sauvegardes contenant des données sensibles doivent également être protégées contre les accès non autorisés.

---

## 12. Journalisation

Les événements importants doivent être journalisés afin de permettre la détection des erreurs, le diagnostic des incidents et l'audit.

Exemples :

```text
AUTHENTICATION_FAILED
RESERVATION_CREATED
RESERVATION_CANCELLED
PAYMENT_FAILED
DATABASE_ERROR
SYSTEM_ERROR
ADMIN_ACTION
```

Les journaux peuvent notamment contenir :

* la date et l'heure ;
* le type d'événement ;
* le niveau de gravité ;
* le composant concerné ;
* un identifiant permettant de suivre la requête.

Les données sensibles ou informations secrètes ne doivent pas être enregistrées inutilement dans les journaux.

OWASP recommande de journaliser les événements importants de sécurité et de protéger les journaux contre les accès ou modifications non autorisés [4].

---

## 13. Reprise après incident

Les données transactionnelles doivent bénéficier d'une stratégie de sauvegarde et de restauration.

Cette stratégie doit comprendre :

* des sauvegardes régulières ;
* plusieurs points de restauration ;
* une protection des sauvegardes ;
* un stockage séparé de la production lorsque nécessaire ;
* des tests périodiques de restauration.

La restauration doit être testée afin de vérifier que les sauvegardes sont réellement exploitables.

Une stratégie de réplication peut également être utilisée afin d'améliorer la disponibilité.

Le système doit pouvoir reconstruire son état transactionnel à partir de la base plutôt qu'à partir du cache.

---

## 14. Haute disponibilité

Pour répondre aux besoins de disponibilité élevée, une architecture comprenant une base primaire et des répliques peut être envisagée.

```text
              APPLICATION
                   |
                   v
          +----------------+
          | Base primaire  |
          +-------+--------+
                  |
             Réplication
                  |
          +-------v--------+
          | Réplique(s)    |
          +----------------+
```

La base primaire reste responsable des écritures critiques.

Les répliques peuvent être utilisées pour certaines lectures et pour améliorer les capacités de récupération après incident.

La configuration exacte dépend du dimensionnement et de l'infrastructure retenus.

---

## 15. Règles d'intégrité

Les règles suivantes doivent toujours être respectées :

1. Un siège appartient à un seul événement.
2. Un siège ne peut être vendu qu'une seule fois.
3. Une réservation expirée ne peut pas être finalisée comme une nouvelle réservation.
4. Un paiement déjà traité ne doit pas être appliqué deux fois.
5. Les opérations critiques doivent être idempotentes lorsque nécessaire.
6. Une réservation doit toujours être associée à un utilisateur et à un événement valides.
7. Les quantités et montants doivent respecter les contraintes définies par le système.

---

## 16. Sauvegarde

Les sauvegardes doivent être effectuées régulièrement.

Elles doivent être :

* protégées ;
* contrôlées ;
* conservées selon une politique définie ;
* testées périodiquement.

Une sauvegarde ne doit pas être considérée comme fiable uniquement parce que le fichier a été créé.

Un test de restauration permet de vérifier que les données peuvent réellement être récupérées.

---

## 17. Évolution de la stratégie

La stratégie de données pourra évoluer selon la croissance de TicketPulse.

### Court terme

* optimiser les requêtes ;
* ajouter les index nécessaires ;
* améliorer le cache ;
* renforcer les tests de concurrence.

### Moyen terme

* mettre en place la réplication ;
* automatiser les sauvegardes ;
* améliorer l'observabilité ;
* optimiser les traitements d'expiration.

### Long terme

Si la volumétrie et la charge l'exigent :

* partitionnement ;
* augmentation de capacité ;
* séparation de certaines charges de lecture ;
* optimisation avancée du stockage.

Ces mécanismes ne doivent être introduits qu'en fonction de besoins mesurés afin d'éviter une complexité prématurée.

---

## 18. Compromis

La cohérence est privilégiée sur le chemin critique de réservation, au prix d'un coût transactionnel plus élevé.

Les lectures non critiques peuvent accepter une légère obsolescence grâce au cache.

En revanche, les informations déterminant si un siège peut être vendu doivent toujours provenir de la source de vérité transactionnelle.

Le système privilégie donc :

```text
Cohérence des réservations
        >
Performance des lectures critiques
```

tout en utilisant le cache et les index pour maintenir de bonnes performances globales.

---

## 19. Synthèse

| Besoin                   | Solution                                |
| ------------------------ | --------------------------------------- |
| Source de vérité         | Base relationnelle transactionnelle     |
| Intégrité                | Contraintes + transactions              |
| Double-vente             | Verrous + opérations atomiques          |
| Réservations temporaires | `expires_at` + traitement idempotent    |
| Paiement                 | Prestataire externe + idempotence       |
| Performance              | Index + cache                           |
| Haute disponibilité      | Réplication / mécanismes de secours     |
| Sécurité                 | Moindre privilège + gestion des secrets |
| Audit                    | Journalisation                          |
| Reprise                  | Sauvegardes + restauration testée       |
| Évolution                | Optimisations progressives              |

---

## 20. Conclusion

La stratégie de données de TicketPulse repose sur une base transactionnelle comme source de vérité et accorde une priorité particulière à la cohérence des réservations.

La concurrence est contrôlée par des transactions, des contraintes d'intégrité et des mécanismes de verrouillage adaptés.

Le cache permet d'améliorer les performances de lecture sans devenir une source d'autorité pour les ventes.

La stratégie est complétée par des mécanismes de sécurité, de journalisation, de sauvegarde, de restauration et de réplication.

L'ensemble permet de répondre aux besoins actuels de TicketPulse tout en conservant une possibilité d'évolution progressive selon la croissance de la plateforme.

---

# Références

**[1] PostgreSQL Global Development Group.** *Transaction Isolation — PostgreSQL Documentation*.
https://www.postgresql.org/docs/current/transaction-iso.html

**[2] PostgreSQL Global Development Group.** *Explicit Locking — PostgreSQL Documentation*.
https://www.postgresql.org/docs/current/explicit-locking.html

**[3] OWASP Foundation.** *Database Security Cheat Sheet*.
https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html

**[4] OWASP Foundation.** *Logging Cheat Sheet*.
https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html
