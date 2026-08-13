# Réunion 1 — 04 août 2026

**Présents :** Fedjo, Ezekiel, Djelia

## Pourquoi cette réunion
Le sujet ne fournit volontairement pas toutes les données nécessaires au dimensionnement du système (taux de conversion, taille des événements, niveau de disponibilité visé, etc.). Cette réunion avait pour but de fixer et justifier ces hypothèses en équipe, condition nécessaire avant de pouvoir comparer des architectures de façon rigoureuse.

## Décisions
- 8 hypothèses de conception validées → `docs/volet2-architecture/hypotheses.md`
- Répartition des responsabilités confirmée :
  - Fedjo : Volet 1 (responsable) + Volet 2 (co-responsable)
  - Ezekiel : Volet 2 (responsable)
  - Djelia : Volet 3 (responsable) + Volet 2 (co-responsable)
- Rythme de travail : point d'équipe quotidien, ~1h

## Impact sur la suite
Ces hypothèses servent directement de base à la comparaison architecturale prévue le 5 août : le choix entre les deux orientations envisagées (monolithe modulaire vs architecture orientée événements) sera justifié à partir de ces valeurs, notamment le ratio lecture/écriture et la priorité cohérence/disponibilité.

## Prochaine réunion
05 août — Jalon 2 : comparaison architecturale et choix retenu.
