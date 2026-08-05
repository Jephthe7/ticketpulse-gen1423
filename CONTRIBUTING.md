# Guide de contribution — équipe TicketPulse

## Branches
- `fedjo-dev`
- `ezekiel-dev`
- `djelia-dev`

Chacun travaille sur sa branche, ouvre une Pull Request vers `main` quand une tâche Jira passe en "Done".
Au moins un autre membre relit avant le merge (ça crée aussi une trace de révision croisée, utile pour l'évaluation individuelle).

## Rythme
- Commit dès qu'une sous-tâche avance, même incomplète.
- Lier chaque commit à son ticket Jira dans le message si possible (ex. `[TICK-12] volet2: ...`).
- Éviter tout commit unique regroupant plusieurs jours de travail — l'énoncé pénalise explicitement ce pattern.

## Revue croisée
Avant de fermer un ticket Jira en "Done", un autre membre que l'auteur doit :
1. Relire le livrable
2. Vérifier que les hypothèses sont justifiées (pas juste affirmées)
3. Laisser un commentaire Jira, même court

## Plan de continuité — qui reprend quoi

| Situation | Réponse concrète |
|---|---|
| Ezekiel indisponible 1-3 jours | Djelia et Fedjo se répartissent le Volet 2 (déjà co-responsables tous les deux). |
| Djelia indisponible 1-3 jours | Fedjo reprend le Volet 3 en priorité ; Ezekiel absorbe temporairement le reste du Volet 2. |
| Fedjo indisponible 1-3 jours | Le Volet 1 est repris par les deux autres à tour de rôle ; les décisions urgentes se documentent pour validation asynchrone à son retour. |
| Une tâche prend plus de temps que prévu | Elle reste ouverte, on ne bloque pas les tâches qui n'en dépendent pas ; le retard est documenté en commentaire Jira, pas caché. |
| Retard global constaté au point du 7 août | On coupe d'abord dans le Volet 3 (15% du barème, le plus compressible) avant de toucher au Volet 2 (35%) ou à la qualité du rapport. Voir section Volet 3 du plan directeur pour le scope minimal. |
| GitHub inaccessible (panne, blocage réseau) | Travail continue en local, commits en attente, push dès le retour d'accès — ne jamais perdre de travail en attendant que GitHub revienne. |
| On réalise après plusieurs jours que l'architecture choisie a un défaut majeur | Décision de retour en arrière prise uniquement en réunion d'équipe (pas seule), documentée comme ADR "Remplacée par ADR-YYY" — jamais un silencieux changement de cap. Seuil : si le défaut remet en cause une exigence clé (ex. non-surréservation), on revient en arrière immédiatement peu importe le jour ; si c'est un simple sous-optimum, on documente et on continue. |
| Jira reste vide plusieurs jours (aucun mouvement de tickets) | Signal d'alerte à soulever explicitement au point quotidien suivant — c'est justement ce que le professeur pénalise, donc on le traite en priorité absolue dès qu'on le remarque. |
| Désaccord non résolu sur un choix technique | Documenter les deux options dans le tableau des compromis plutôt que trancher à l'arrache ; ça devient un contenu valorisable pour le rapport plutôt qu'une perte de temps. |
