# TicketPulse — GEN1423 Génie logiciel — Groupe 3

TicketPulse est une proposition d'architecture pour une plateforme de billetterie devant demeurer disponible pendant des ventes flash, traiter jusqu'à 250 000 utilisateurs simultanés et 50 000 requêtes par seconde, et empêcher toute double-vente de siège.

## Équipe

- **Wilfried Jephthé Fedjo Tejioleng** — coordination, pilotage Agile, intégration et cohérence globale.
- **Eyadèma Ezékiel Kadanga** — architecture, dimensionnement et stratégie de données.
- **Djelia Hanifah Sanon** — analyse des besoins, qualité, tests, sécurité, observabilité et reprise.

Les responsabilités principales facilitent la coordination ; les décisions structurantes et les revues sont collectives.

## Organisation

```text
docs/
├── volet1-agile/        # Jira, Git, risques, réunions et preuves
├── volet2-architecture/ # besoins, hypothèses, décisions, calculs et diagrammes
└── volet3-qualite/      # tests, performance, sécurité, observabilité et reprise
rapport-final/           # rapport consolidé
presentation/            # support de présentation orale
references/              # énoncé et bibliographie
```

Le classement est effectué par livrable et non par membre. Les sources DOCX/PDF conservées dans `docs/volet3-qualite/sources/` assurent la traçabilité des contributions déjà fusionnées.

## Architecture retenue

L'équipe retient un monolithe modulaire répliqué en instances sans état derrière un répartiteur de charge. Une base relationnelle transactionnelle demeure la source de vérité pour les réservations. Le cache accélère uniquement les lectures et ne peut jamais confirmer une vente.

## État de la remise

Les volets documentaires sont consolidés. Les maquettes Figma sont encore en production et devront être déposées dans `docs/volet2-architecture/maquettes/` avant le gel final du contenu.

