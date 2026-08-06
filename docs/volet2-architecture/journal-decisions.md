# Journal des décisions — TicketPulse

> **Règle :** écrire ici immédiatement après la réunion où la décision a été prise, jamais plusieurs jours plus tard — la justification se perd sinon.

À compléter à chaque décision structurante (architecture, données, hypothèse de charge, choix technique). Objectif : à la fin du projet, le rapport (tableau des décisions + tableau des compromis) est déjà quasiment rédigé.

Format court, pas besoin d'une ADR complète pour chaque ligne — réserver le format ADR (`docs/templates/ADR-template.md`) aux décisions vraiment structurantes (choix d'architecture, choix de base de données, stratégie de cohérence).

## Journal

| Date | Décision | Pourquoi | Alternative rejetée | Impact |
|---|---|---|---|---|
| 05 août | Architecture retenue : monolithe modulaire, instances multiples sans état derrière un répartiteur de charge, module réservation isolé avec transactions strictes | Rapidité/simplicité adaptées au délai de 8 jours ; trafic majoritairement en lecture (90%) donc scalabilité horizontale suffisante sans complexité distribuée | Architecture orientée événements — meilleure absorption théorique des pics mais complexité et risque trop élevés pour le délai | Dimensionnement séparé lecture/écriture ; diagramme d'architecture avec répartiteur + cache ; tests de charge + tests de concurrence ciblés sur la réservation. Détail dans `ADR-001-choix-architecture.md` |

