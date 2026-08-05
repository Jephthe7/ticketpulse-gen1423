# TicketPulse — GEN1423 Génie Logiciel — Groupe 3

## Présentation du projet

TicketPulse est une plateforme de billetterie d'événements en temps réel (Sujet A) conçue pour répondre aux défis liés aux ventes à forte demande.

Le système doit notamment permettre de gérer :
- un nombre élevé d'utilisateurs simultanés ;
- des réservations concurrentes ;
- des transactions sécurisées ;
- une disponibilité élevée du service.

Ce dépôt contient l'ensemble des documents produits par l'équipe dans le cadre du projet : analyses, décisions architecturales, documents de qualité et rapport final.

---

## Équipe

### Fedjo Tejioleng, Wilfried Jephthé
Responsabilités principales :
- Coordination générale du projet ;
- Suivi Agile (Git, Jira, organisation des tâches) ;
- Intégration et cohérence des livrables ;
- Contribution aux décisions architecturales.

### Kadanga, Eyadèma Ezékiel
Responsabilités principales :
- Analyse architecturale ;
- Comparaison des solutions techniques ;
- Dimensionnement et conception du système.

### Sanon, Djelia Hanifah
Responsabilités principales :
- Qualité du système ;
- Tests ;
- Sécurité ;
- Maintenance et évolution.

Les décisions structurantes (architecture, hypothèses importantes, compromis techniques) sont prises collectivement par l'équipe.

---

## Contexte académique

Projet réalisé dans le cadre du cours GEN1423 — Génie logiciel à l'Université du Québec en Outaouais (UQO).

Enseignant : Abderrahmane Ben Mimoune

Dates importantes :
- Remise finale : 12 août 2026 à 23h59
- Présentation : 13 août 2026

---

## Organisation du dépôt

```
ticketpulse-gen1423/
├── docs/
│   ├── volet1-agile/
│   │   → Organisation et suivi du projet :
│   │      comptes rendus de réunions, suivi Agile,
│   │      gestion des risques, preuves Git/Jira
│   │
│   ├── volet2-architecture/
│   │   → Conception technique du système :
│   │      hypothèses de conception, décisions architecturales,
│   │      dimensionnement, diagrammes, compromis techniques
│   │
│   ├── volet3-qualite/
│   │   → Assurance qualité :
│   │      stratégie de tests, sécurité, observabilité, maintenance
│   │
│   ├── templates/
│   │   → Modèles utilisés pour uniformiser la documentation du projet
│   │
│   ├── etat-du-projet.md
│   │   → Tableau de suivi de l'avancement global
│   │
│   └── decisions-en-attente.md
│       → Décisions importantes restant à trancher
│
├── rapport-final/
│   → Construction progressive du rapport final remis sur Moodle
│
├── references/
│   → Sources utilisées : énoncé, notes de cours, références techniques
│
├── CONTRIBUTING.md
│   → Règles de collaboration Git et organisation du travail
│
└── README.md
    → Point d'entrée du projet
```

---

## Outils utilisés

### Git / GitHub
Utilisés pour :
- gérer les versions des documents ;
- conserver l'historique des contributions ;
- faciliter la collaboration entre les membres.

### Jira
Utilisé pour :
- organiser le backlog ;
- suivre les tâches ;
- gérer les deux sprints du projet.

### Figma
Utilisé pour :
- concevoir les maquettes haute fidélité ;
- représenter le parcours utilisateur.

---

## Organisation Git

### Branches
- `main` : branche principale contenant uniquement les versions validées.
- Une branche individuelle par membre pour le travail en cours.

Branches actuelles :
- `fedjo-dev`
- `ezekiel-dev`
- `djelia-dev`

Les modifications importantes sont intégrées après validation.

---

## Convention de commits

Format :
```
<volet>: <action réalisée>
```

Exemples :
```
volet2: ajout comparaison architectures
volet3: ajout scénarios de tests
rapport: correction structure finale
```

Les commits doivent représenter des avancées réelles et documenter l'évolution du projet.
