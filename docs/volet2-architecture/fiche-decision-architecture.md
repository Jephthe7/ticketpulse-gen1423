# Fiche de décision — Architecture

## Statut du document

Document préparatoire utilisé lors de la réunion d'architecture du 5 août 2026.
Il présente les options étudiées et les critères de comparaison. La décision finale sera documentée séparément après validation de l'équipe.

## Option A — Monolithe modulaire
Un seul système, séparé en modules internes (recherche, réservation, paiement, utilisateurs), déployé comme une seule application.

**Avantages**
- Plus rapide à concevoir et développer en 8 jours
- Moins de pièces à orchestrer → moins de risques d'erreurs d'intégration
- Plus simple à déboguer et déployer pour une équipe de 3

**Limites**
- Scalabilité horizontale plus limitée face au pic de charge attendu
- Risque de couplage fort entre modules si la séparation n'est pas rigoureuse

## Option B — Architecture orientée événements
Plusieurs services séparés (recherche, réservation, paiement) communiquant via une file d'attente/événements.

**Avantages**
- Meilleure absorption des pics grâce au découplage (chaque service scale indépendamment)
- Isolation des pannes : un service en panne n'entraîne pas tout le système

**Limites**
- Complexité de conception et d'implémentation plus élevée
- Risque de complexité supplémentaire dans le contexte du délai limité du projet
- Cohérence distribuée plus difficile à garantir — justement là où se joue l'exigence "zéro double-vente"

## Critères de choix (issus de nos hypothèses validées le 4 août)
1. Capacité à répondre aux contraintes de charge indiquées dans l'énoncé (jusqu'à 250 000 utilisateurs simultanés et 50 000 requêtes/seconde pendant les périodes de pointe), avec un profil estimé à 90% lecture / 10% écriture
2. Garantie de zéro double-vente sur la réservation (cohérence forte exigée)
3. Faisabilité réaliste dans le délai imparti du projet
4. Capacité de chacun à expliquer le choix à l'oral
5. Maintenance/évolution future

## Piste de discussion (à challenger en réunion, pas une décision)
Une piste à analyser est qu'une architecture monolithique modulaire bien conçue pourrait répondre aux besoins, notamment grâce à une séparation stricte du module de réservation et à une gestion transactionnelle forte. Cette hypothèse devra être comparée avec les avantages d'une architecture orientée événements.

## Décision attendue lors de la réunion
À la fin de la réunion, l'équipe devra avoir validé :
- l'orientation architecturale retenue ;
- les raisons principales du choix ;
- les compromis acceptés ;
- les impacts sur le dimensionnement, les diagrammes et les tests.
