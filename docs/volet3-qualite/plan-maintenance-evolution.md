# Plan de maintenance et d'évolution — TicketPulse

## 1. Introduction

Le plan de maintenance et d'évolution de TicketPulse définit les pratiques permettant de maintenir le système fiable, sécurisé, performant et évolutif après sa mise en service.

La maintenance ne se limite pas à corriger les erreurs. Elle comprend également la prévention des incidents, les mises à jour de sécurité, la surveillance du système, les sauvegardes, les tests et l'adaptation du logiciel aux nouveaux besoins.

Le plan couvre quatre catégories principales :

* maintenance corrective ;
* maintenance préventive ;
* maintenance adaptative ;
* maintenance évolutive.

---

## 2. Objectifs

Les objectifs sont :

* maintenir la disponibilité du service ;
* corriger rapidement les anomalies ;
* prévenir les incidents ;
* maintenir la sécurité ;
* conserver de bonnes performances ;
* limiter les régressions ;
* faciliter l'ajout de nouvelles fonctionnalités ;
* contrôler la dette technique ;
* assurer la traçabilité des changements.

---

## 3. Maintenance corrective

La maintenance corrective consiste à résoudre les anomalies détectées après l'intégration ou la mise en service du système.

Exemples :

* impossibilité de réserver un siège ;
* double-vente détectée ;
* erreur de paiement ;
* problème d'authentification ;
* erreur lors de la génération d'un billet ;
* problème d'affichage ;
* ralentissement important ;
* panne d'un service.

Le processus est :

```text
Détection
   ↓
Création d'un ticket
   ↓
Analyse
   ↓
Correction
   ↓
Tests
   ↓
Revue
   ↓
Déploiement
   ↓
Surveillance
```

Chaque incident important doit être documenté afin de conserver une trace de sa cause, de son impact et de sa correction.

---

## 4. Classification des incidents

| Niveau   | Description                               | Exemple                |
| -------- | ----------------------------------------- | ---------------------- |
| Critique | Fonction critique indisponible            | Impossible de réserver |
| Élevé    | Fonction importante fortement dégradée    | Paiement indisponible  |
| Moyen    | Fonction dégradée mais système utilisable | Recherche lente        |
| Faible   | Impact limité                             | Problème visuel        |

Les incidents critiques sont traités en priorité.

---

## 5. Maintenance préventive

La maintenance préventive vise à réduire la probabilité d'apparition d'incidents.

Elle comprend notamment :

* mise à jour des dépendances ;
* application des correctifs de sécurité ;
* vérification des sauvegardes ;
* tests de restauration ;
* surveillance des performances ;
* analyse des journaux ;
* vérification des ressources ;
* tests de charge ;
* vérification de la configuration ;
* contrôle des permissions.

Les dépendances doivent être régulièrement vérifiées afin d'identifier les versions présentant des vulnérabilités connues.

---

## 6. Maintenance adaptative

La maintenance adaptative permet d'adapter TicketPulse aux changements de son environnement.

Exemples :

* changement d'infrastructure ;
* mise à jour de PostgreSQL ;
* changement d'un fournisseur de paiement ;
* changement d'une API externe ;
* évolution des exigences de sécurité ;
* changement de version d'une technologie utilisée.

Chaque changement doit être précédé d'une analyse d'impact.

---

## 7. Maintenance évolutive

La maintenance évolutive concerne l'ajout ou l'amélioration des fonctionnalités.

Exemples :

* nouveaux types de billets ;
* notifications ;
* nouvelles fonctionnalités administratives ;
* amélioration de la recherche ;
* nouveaux modes de paiement ;
* statistiques ;
* gestion avancée des événements.

Une nouvelle fonctionnalité doit suivre le même processus de développement, de test et de validation qu'une correction.

---

## 8. Sécurité

La sécurité doit être intégrée à toutes les activités de maintenance.

Les actions comprennent :

* analyse régulière des dépendances ;
* mise à jour des bibliothèques vulnérables ;
* revue des permissions ;
* rotation des secrets ;
* vérification des comptes ;
* analyse des journaux ;
* correction des vulnérabilités.

Les comptes techniques doivent respecter le principe du moindre privilège.

Les secrets ne doivent jamais être stockés dans le dépôt Git.

---

## 9. Observabilité

L'observabilité permet de détecter les problèmes et d'analyser le comportement du système.

Elle repose sur trois éléments principaux.

### 9.1 Métriques

Les métriques importantes comprennent :

* temps de réponse ;
* taux d'erreur ;
* nombre de requêtes ;
* utilisation CPU ;
* utilisation mémoire ;
* connexions à la base ;
* temps des requêtes ;
* nombre de réservations ;
* taux d'échec des paiements.

### 9.2 Journaux

Les événements importants doivent être journalisés.

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

Les journaux ne doivent pas contenir inutilement de données sensibles.

### 9.3 Alertes

Des alertes doivent être déclenchées lorsque des seuils critiques sont dépassés.

Exemples :

```text
Taux d'erreur élevé
        ↓
      ALERTE

Temps de réponse élevé
        ↓
      ALERTE

Base de données indisponible
        ↓
  ALERTE CRITIQUE
```

---

## 10. Sauvegarde et restauration

Les sauvegardes doivent faire partie intégrante du plan de maintenance.

Elles doivent être :

* régulières ;
* protégées ;
* conservées selon une politique définie ;
* accessibles uniquement aux personnes autorisées ;
* testées périodiquement.

Des tests de restauration doivent être réalisés afin de vérifier que les sauvegardes sont réellement exploitables.

---

## 11. Gestion des versions

Les modifications du système sont gérées avec Git.

L'organisation actuelle du projet comprend notamment :

```text
main
 ├── fedjo-dev
 ├── ezekiel-dev
 └── djelia-dev
```

La branche `main` doit contenir uniquement les versions validées.

Les modifications importantes sont réalisées dans une branche de développement puis intégrées après validation.

---

## 12. Gestion des tâches

Jira est utilisé pour suivre les tâches et les incidents.

Chaque problème important doit être associé à un ticket permettant de suivre :

* la description ;
* la priorité ;
* le responsable ;
* l'état ;
* les actions réalisées ;
* la résolution.

Les tâches de maintenance importantes peuvent être intégrées au backlog.

---

## 13. Gestion des changements

Chaque changement important doit suivre le processus :

```text
Besoin
  ↓
Analyse d'impact
  ↓
Création de la tâche
  ↓
Développement
  ↓
Tests
  ↓
Revue
  ↓
Pull Request
  ↓
Fusion
  ↓
Déploiement
  ↓
Surveillance
```

L'analyse d'impact doit notamment considérer :

* la base de données ;
* les API ;
* l'interface ;
* la sécurité ;
* les performances ;
* les tests ;
* la documentation.

---

## 14. Migrations de la base de données

Les modifications du schéma doivent être versionnées.

Exemple :

```text
V001_initial_schema
V002_add_reservation_status
V003_add_payment_reference
V004_add_event_index
```

Avant une migration importante :

1. effectuer une sauvegarde ;
2. tester la migration ;
3. vérifier la compatibilité ;
4. exécuter la migration ;
5. vérifier les données ;
6. surveiller le système.

---

## 15. Tests de non-régression

Les tests de non-régression permettent de vérifier qu'une modification n'a pas cassé une fonctionnalité existante.

Les tests doivent notamment couvrir :

### Authentification

* connexion valide ;
* connexion invalide ;
* permissions.

### Événements

* création ;
* modification ;
* consultation.

### Billetterie

* affichage des billets ;
* réservation ;
* annulation ;
* épuisement du stock.

### Concurrence

Plusieurs utilisateurs doivent pouvoir tenter simultanément de réserver un même siège sans provoquer de double-vente.

### Paiement

* succès ;
* refus ;
* interruption ;
* notification répétée ;
* reprise après erreur.

---

## 16. Tests de charge

Les tests de charge doivent reproduire les périodes de forte demande.

```text
Trafic normal
      ↓
Augmentation progressive
      ↓
Pic de trafic
      ↓
Maintien
      ↓
Retour à la normale
```

Les éléments suivants doivent être mesurés :

* temps de réponse ;
* taux d'erreur ;
* nombre de requêtes ;
* utilisation CPU ;
* utilisation mémoire ;
* comportement de la base ;
* comportement des réservations concurrentes.

Les résultats permettent d'identifier les limites du système et d'anticiper les besoins de capacité.

---

## 17. Retour arrière

Pour les changements critiques, une procédure de rollback doit être prévue.

```text
Problème détecté
      ↓
Évaluation
      ↓
Décision
      ↓
Retour à la version précédente
      ↓
Vérification
      ↓
Analyse de la cause
```

Lorsque le changement concerne la base de données, la procédure de retour arrière doit être définie avant le déploiement.

---

## 18. Gestion de la dette technique

La dette technique doit être identifiée et suivie dans Jira.

Exemples :

* code complexe ;
* tests manquants ;
* documentation incomplète ;
* dépendance obsolète ;
* requête SQL inefficace ;
* duplication de code ;
* architecture difficile à faire évoluer.

Les problèmes importants doivent être transformés en tâches et planifiés dans les cycles de développement.

---

## 19. Indicateurs de maintenance

| Indicateur            | Objectif                             |
| --------------------- | ------------------------------------ |
| Nombre d'incidents    | Mesurer la stabilité                 |
| MTTR                  | Mesurer le temps moyen de résolution |
| Taux d'erreur         | Mesurer la fiabilité                 |
| Disponibilité         | Mesurer la continuité                |
| Vulnérabilités        | Mesurer la sécurité                  |
| Couverture de tests   | Mesurer la prévention                |
| Régressions           | Mesurer la qualité                   |
| Échecs de déploiement | Mesurer la fiabilité du processus    |

Ces indicateurs peuvent être analysés périodiquement.

---

## 20. Plan d'évolution

### Court terme

* améliorer les tests ;
* améliorer la journalisation ;
* corriger les anomalies identifiées ;
* renforcer la gestion des erreurs.

### Moyen terme

* automatiser davantage les déploiements ;
* améliorer le cache ;
* renforcer l'observabilité ;
* améliorer la haute disponibilité ;
* automatiser les sauvegardes.

### Long terme

Si la charge augmente :

* augmenter la capacité ;
* améliorer la répartition des lectures ;
* optimiser le stockage ;
* envisager le partitionnement ;
* faire évoluer l'architecture si nécessaire.

Les évolutions doivent être basées sur les besoins réels et les mesures obtenues.

---

## 21. Responsabilités

| Domaine           | Responsable principal        |
| ----------------- | ---------------------------- |
| Architecture      | Équipe                       |
| Données           | Responsable architecture     |
| Tests             | Responsable qualité          |
| Sécurité          | Responsable qualité + équipe |
| Git / intégration | Responsable coordination     |
| Documentation     | Toute l'équipe               |
| Maintenance       | Équipe technique             |

Les responsabilités peuvent être adaptées selon l'organisation du projet.

---

## 22. Cycle général de maintenance

```text
             SURVEILLANCE
                  ↓
          Détection d'un besoin
                  ↓
            Analyse d'impact
                  ↓
             Développement
                  ↓
                 Tests
                  ↓
              Validation
                  ↓
              Déploiement
                  ↓
             Surveillance
                  ↓
              Nouveau cycle
```

La maintenance est ainsi intégrée au cycle de vie complet du logiciel.

---

## 23. Conclusion

Le plan de maintenance et d'évolution de TicketPulse vise à garantir la fiabilité, la sécurité, la disponibilité et l'évolutivité du système tout au long de son cycle de vie.

La maintenance corrective permet de résoudre les incidents, tandis que la maintenance préventive permet de réduire les risques. La maintenance adaptative permet de répondre aux changements technologiques et la maintenance évolutive permet d'ajouter progressivement de nouvelles fonctionnalités.

L'utilisation de Git et Jira assure la traçabilité des changements. Les tests, la surveillance, la journalisation, les sauvegardes et les procédures de rollback permettent de réduire les risques associés aux modifications du système.

Enfin, l'évolution de TicketPulse doit rester progressive et être basée sur les besoins réels, les performances observées et les retours obtenus.

---

# Références

**[1] OWASP Foundation.** *Database Security Cheat Sheet*.
https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html

**[2] OWASP Foundation.** *Logging Cheat Sheet*.
https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html

**[3] PostgreSQL Global Development Group.** *PostgreSQL Documentation*.
https://www.postgresql.org/docs/current/
