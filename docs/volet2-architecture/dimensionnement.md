# Dimensionnement du système — TicketPulse

## 1. Objectif

Le dimensionnement vérifie que l'architecture retenue pour TicketPulse peut absorber le profil de charge imposé par le sujet, tout en respectant les contraintes de réservation concurrente, de temps de réponse et de disponibilité.

Les hypothèses chiffrées utilisées proviennent de `hypotheses.md`, qui constitue la source unique de vérité pour les données du sujet.

Lorsque le sujet ne fournit pas une donnée nécessaire au calcul, celle-ci est explicitement présentée comme une **hypothèse de dimensionnement**. Ces hypothèses devront être validées par des tests de charge sur un prototype.

---

## 2. Données de référence

| Paramètre                         |        Valeur |
| --------------------------------- | ------------: |
| Utilisateurs simultanés au pic    |       250 000 |
| Requêtes/seconde au pic           |  50 000 req/s |
| Ratio lecture/écriture            |   90 % / 10 % |
| Requêtes de lecture au pic        |  45 000 req/s |
| Requêtes d'écriture au pic        |   5 000 req/s |
| Taille moyenne d'une requête      |         ~2 Ko |
| Taille moyenne d'une réponse      |         ~5 Ko |
| Conversion connectés → acheteurs  |          20 % |
| Taille moyenne d'un événement     | 50 000 sièges |
| Maximum de sièges par transaction |             8 |
| Disponibilité visée               |        99,9 % |
| Majorité des réponses             |         < 2 s |

Ces valeurs servent de base au dimensionnement analytique. Elles ne constituent pas une mesure réelle d'une infrastructure déployée.

---

## 3. Débit applicatif

Le débit maximal prévu est :

`QPS_total = 50 000 req/s`

### 3.1 Requêtes de lecture

`QPS_lecture = 50 000 × 0,90`

`QPS_lecture = 45 000 req/s`

### 3.2 Requêtes d'écriture

`QPS_écriture = 50 000 × 0,10`

`QPS_écriture = 5 000 req/s`

Les 5 000 écritures/s représentent les requêtes d'écriture au niveau de l'API. Elles ne doivent pas être interprétées comme 5 000 achats/s : une réservation ou un paiement peut produire plusieurs opérations internes.

---

## 4. Volume de données réseau

### 4.1 Requêtes entrantes

Avec une taille moyenne de requête d'environ 2 Ko :

`50 000 × 2 Ko = 100 000 Ko/s`

Soit environ :

`100 Mo/s`

ou :

`0,8 Gbit/s`

### 4.2 Réponses sortantes

Avec une taille moyenne de réponse d'environ 5 Ko :

`50 000 × 5 Ko = 250 000 Ko/s`

Soit environ :

`250 Mo/s`

ou :

`2,0 Gbit/s`

### 4.3 Trafic applicatif total

`100 + 250 = 350 Mo/s`

Soit environ :

`2,8 Gbit/s`

Cette estimation correspond au trafic applicatif utile. Elle exclut notamment les en-têtes HTTP, TLS, retransmissions, logs, métriques et communications internes entre composants.

Une marge opérationnelle doit donc être prévue dans un déploiement réel.

---

## 5. Dimensionnement des instances applicatives

Le sujet fournit le débit global de 50 000 req/s, mais ne fournit pas la capacité réelle d'une instance applicative.

Pour obtenir une première estimation, on utilise donc l'hypothèse de dimensionnement suivante :

> **Une instance applicative peut traiter 1 000 req/s soutenues dans les conditions de charge considérées.**

Cette valeur est une hypothèse de travail et devra être validée par un benchmark.

### 5.1 Nombre minimal d'instances

`Nombre_instances = 50 000 / 1 000`

`Nombre_instances = 50`

Il faudrait donc au minimum **50 instances applicatives** pour absorber théoriquement 50 000 req/s.

### 5.2 Marge de capacité

Une marge de 30 % est ajoutée afin de tenir compte des variations de charge, des opérations internes et des périodes de pointe :

`50 × 1,30 = 65`

Le dimensionnement cible est donc d'environ :

**65 instances applicatives.**

Cette valeur n'est pas une capacité garantie. Elle constitue une cible initiale à confirmer par des tests de charge.

### 5.3 Répartition de la charge

Les instances applicatives peuvent être placées derrière un répartiteur de charge afin de distribuer les requêtes.

L'architecture doit permettre une réplication horizontale de l'application. Une instance ne doit donc pas être considérée comme une source de vérité pour l'état des réservations.

Les données critiques doivent rester dans les composants persistants et transactionnels prévus à cet effet.

---

## 6. Dimensionnement de l'inventaire

Un événement moyen contient :

`50 000 sièges`

Chaque siège possède au minimum un état permettant de représenter sa disponibilité.

États proposés :

* `AVAILABLE` : disponible ;
* `HELD` : réservation temporaire ;
* `SOLD` : vendu.

Transitions principales :

`AVAILABLE → HELD → SOLD`

En cas d'échec, d'annulation ou d'expiration :

`HELD → AVAILABLE`

La transition critique doit être atomique et protégée par les mécanismes d'intégrité de la base de données afin d'empêcher deux transactions concurrentes de vendre le même siège.

Le maximum de sièges par transaction est de 8. Une transaction de réservation doit donc pouvoir verrouiller ou mettre à jour au maximum 8 sièges dans le cadre d'une même opération métier.

---

## 7. Concurrence des réservations

La concurrence constitue une contrainte majeure de TicketPulse.

Deux utilisateurs peuvent tenter de réserver le même siège simultanément. Le système doit garantir qu'un seul d'entre eux puisse obtenir ce siège.

La réservation doit donc être réalisée dans une transaction atomique.

Le principe retenu est :

1. vérifier la disponibilité du siège ;
2. verrouiller ou réserver atomiquement le siège ;
3. modifier son état ;
4. valider la transaction ;
5. confirmer la réservation au client.

La base transactionnelle constitue la **source de vérité** pour l'état d'un siège.

Le cache ne doit pas être utilisé comme source de vérité pour déterminer si un siège est réellement disponible.

Une contrainte d'intégrité sur l'identification du siège et de l'événement, combinée au mécanisme transactionnel approprié de la base de données, permet d'empêcher deux ventes concurrentes du même siège.

En cas de concurrence élevée, les requêtes d'écriture peuvent également être limitées ou mises en file afin de protéger la capacité de la base transactionnelle.

---

## 8. Conversion et volume d'acheteurs

L'hypothèse prévoit une conversion de 20 % des utilisateurs connectés en acheteurs.

Avec 250 000 utilisateurs simultanés :

`250 000 × 20 % = 50 000 acheteurs`

Le résultat est donc :

**50 000 acheteurs potentiels.**

Ce nombre ne permet cependant pas de calculer directement un nombre de transactions par seconde.

En effet, la durée exacte de la période de vente et la répartition temporelle des achats ne sont pas fournies.

Il serait donc incorrect d'affirmer que 50 000 acheteurs impliquent 50 000 transactions/s.

---

## 9. Latence

L'objectif de qualité est que la majorité des réponses soit fournie en moins de :

**2 secondes.**

Les opérations de lecture non critiques doivent être optimisées par :

* l'indexation appropriée ;
* la mise en cache lorsque cela est possible ;
* la réduction des accès inutiles à la base de données ;
* la réplication horizontale des instances applicatives.

Pour les opérations critiques de réservation, la cohérence des données est prioritaire sur une optimisation agressive de la latence.

Une réponse légèrement plus lente est préférable à une réservation incohérente ou à une double-vente.

La validation de l'objectif de 2 secondes devra être effectuée au moyen de tests de charge et de mesures de percentiles de latence.

---

## 10. Disponibilité

L'objectif de disponibilité est :

**99,9 %.**

Sur une période de 30 jours :

`30 × 24 × 60 = 43 200 minutes`

Le budget d'indisponibilité est donc :

`43 200 × 0,001 = 43,2 minutes`

Ainsi, une disponibilité de 99,9 % correspond à environ :

**43 minutes d'indisponibilité maximale par mois de 30 jours.**

Pour contribuer à atteindre cet objectif, l'architecture doit éviter de dépendre d'une seule instance applicative.

La réplication horizontale, le répartiteur de charge et la capacité de remplacement d'une instance défaillante sont donc des éléments importants du déploiement.

---

## 11. Stockage et données persistantes

Le dimensionnement exact du stockage dépend du nombre d'événements, de la durée de conservation des données et de la taille réelle des enregistrements.

Le sujet fournit principalement une taille moyenne de 50 000 sièges par événement, mais ne fournit pas suffisamment d'informations pour calculer un volume annuel exact sans introduire de nouvelles hypothèses.

Le dimensionnement est donc exprimé de manière paramétrable.

Soit :

* `N` = nombre d'événements ;
* `50 000` = nombre moyen de sièges par événement ;
* `S` = taille moyenne persistée d'un enregistrement de siège.

Le nombre total d'enregistrements de sièges est :

`N × 50 000`

Le volume brut correspondant est :

`N × 50 000 × S`

Ce volume doit ensuite être augmenté pour prendre en compte :

* les index ;
* les réservations ;
* les utilisateurs ;
* les paiements ;
* les journaux applicatifs persistants lorsque nécessaire ;
* les sauvegardes ;
* la réplication.

La capacité réelle de stockage devra donc être déterminée après mesure de la taille des enregistrements et définition de la politique de rétention.

---

## 12. Conséquences architecturales

Les résultats du dimensionnement entraînent les conséquences suivantes :

1. **Optimiser fortement les lectures**, car elles représentent environ 90 % du trafic.
2. **Conserver une frontière transactionnelle forte autour de la réservation.**
3. **Isoler logiquement le module de réservation** afin de protéger les opérations critiques.
4. **Permettre la réplication horizontale de l'application** pour absorber les 50 000 req/s.
5. **Utiliser la base transactionnelle comme source de vérité** pour les données critiques.
6. **Utiliser le cache principalement pour accélérer les lectures**, et non pour garantir la cohérence des réservations.
7. **Prévoir une marge de capacité** afin d'absorber les variations de charge.
8. **Ajouter un mécanisme de limitation ou de file d'attente** devant les opérations critiques si les tests montrent un dépassement de capacité.
9. **Surveiller la base de données**, notamment les temps de réponse, les connexions et les IOPS.
10. **Valider les hypothèses par des tests de charge** avant de considérer les valeurs de dimensionnement comme définitives.

---

## 13. Limites du dimensionnement

Le présent dimensionnement est analytique. Il permet de vérifier la cohérence générale de l'architecture, mais ne remplace pas une validation expérimentale.

Les éléments suivants restent à mesurer sur un prototype :

* CPU par instance ;
* mémoire par instance ;
* capacité réelle en requêtes/seconde par instance ;
* IOPS de la base de données ;
* taille physique réelle des données ;
* capacité réelle du cache ;
* nombre maximal de connexions simultanées ;
* latence réelle de la base ;
* comportement sous 50 000 req/s ;
* comportement sous forte concurrence sur un même événement ;
* taux d'erreurs sous charge ;
* temps de récupération après une défaillance.

L'hypothèse de **1 000 req/s par instance** utilisée pour obtenir environ **65 instances avec une marge de 30 %** doit notamment être validée par un benchmark.

Ces validations relèvent des tests de charge et de performance du volet 3.

---

## 14. Synthèse

Le dimensionnement montre que le profil cible de TicketPulse est principalement caractérisé par :

* **250 000 utilisateurs simultanés** ;
* **50 000 req/s au pic** ;
* **45 000 req/s de lecture** ;
* **5 000 req/s d'écriture** ;
* environ **2,8 Gbit/s de trafic applicatif** ;
* **50 000 acheteurs potentiels** selon l'hypothèse de conversion ;
* des événements pouvant contenir **50 000 sièges** ;
* un maximum de **8 sièges par transaction** ;
* une disponibilité cible de **99,9 %** ;
* une majorité des réponses attendues sous **2 secondes**.

Sous l'hypothèse de 1 000 req/s par instance et avec une marge de 30 %, une cible initiale d'environ **65 instances applicatives** est obtenue.

Cette estimation doit être confirmée expérimentalement. L'architecture retenue doit surtout garantir que la montée en charge des lectures ne compromette pas la cohérence des réservations et des paiements.
