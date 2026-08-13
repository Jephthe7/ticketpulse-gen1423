# Guide de contribution — TicketPulse

## Branches

- `main` contient les versions relues et validées ;
- `fedjo-dev`, `ezekiel-dev` et `djelia-dev` servent aux travaux individuels ;
- une Pull Request vers `main` permet la revue avant intégration.

## Flux de contribution

1. Travailler sur sa branche.
2. Effectuer des commits correspondant à des avancées identifiables.
3. Mentionner le ticket Jira réel dans le message lorsque possible, par exemple `[SCRUM-12] volet3: ajout des scénarios d'acceptation`.
4. Ouvrir une Pull Request.
5. Faire relire le livrable par au moins un autre membre.
6. Mettre à jour le ticket Jira avec le lien ou le résultat associé.
7. Fusionner dans `main` après validation.

L'équipe a appris progressivement Git et GitHub pendant le projet. Les premières manipulations n'ont pas toujours suivi parfaitement ce flux ; les Pull Requests et revues ultérieures montrent la correction progressive de la pratique.

## Règles documentaires

- Un livrable est classé par volet, jamais dans un dossier portant le nom d'un membre.
- Les hypothèses chiffrées sont centralisées dans `docs/volet2-architecture/hypotheses.md`.
- Une décision structurante est ajoutée au journal des décisions.
- Les exports de travail sont séparés des versions canoniques.
- Une donnée non mesurée est présentée comme une hypothèse, jamais comme un résultat observé.

