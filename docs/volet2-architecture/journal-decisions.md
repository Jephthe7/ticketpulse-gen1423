# Journal des décisions — TicketPulse

> **Règle :** écrire ici immédiatement après la réunion où la décision a été prise, jamais plusieurs jours plus tard — la justification se perd sinon.

À compléter à chaque décision structurante (architecture, données, hypothèse de charge, choix technique). Objectif : à la fin du projet, le rapport (tableau des décisions + tableau des compromis) est déjà quasiment rédigé.

Format court, pas besoin d'une ADR complète pour chaque ligne — réserver le format ADR (`docs/templates/ADR-template.md`) aux décisions vraiment structurantes (choix d'architecture, choix de base de données, stratégie de cohérence).

## Journal

| Date | Décision | Pourquoi | Alternative rejetée | Impact |
|---|---|---|---|---|
| _(à compléter au fur et à mesure)_ | | | | |

---
*Exemple de ligne (à retirer une fois le journal réellement démarré) :*

| 04 août | Taux de conversion pic fixé à 15% des connectés simultanés | Ordre de grandeur réaliste pour une vente flash à forte demande (billetterie type Ticketmaster lors de ventes très demandées) | Fixer à 50% jugé irréaliste, aurait surdimensionné le système inutilement | Sert de base au calcul du QPS d'achat réel |
