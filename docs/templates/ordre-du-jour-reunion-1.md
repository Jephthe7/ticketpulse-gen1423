# Ordre du jour — Réunion 1 (aujourd'hui, 4 août)

**Durée cible :** 1h · **Objectif :** sortir avec les 8 hypothèses tranchées et une répartition provisoire

## 1. Contexte rapide (5 min)
Rappel du sujet, du barème, du calendrier jusqu'au 12 août.

## 2. Les 8 questions de compréhension du problème (35-40 min)
Voir `plan_directeur_interne.md`, Phase 1. Passer chaque question une à une, noter la réponse retenue et pourquoi directement dans `docs/volet2-architecture/journal-decisions.md` pendant la réunion (pas après).

1. Taux de conversion des connectés en pic → acheteurs réels
2. Taille moyenne d'un événement (nombre de sièges)
3. Nombre max de sièges par transaction
4. Comportement si le paiement échoue après réservation
5. Priorité cohérence vs disponibilité (CAP) en cas de conflit
6. Ratio lecture/écriture pendant le pic de 50k req/s
7. Taille moyenne d'une requête/réponse API
8. Budget de disponibilité visé (ex. 99.9%)

## 3. Répartition provisoire des responsabilités (10 min)
- Fedjo : coordination + Volet 2 (co-lead)
- Kadanga (Eyadèma Ezékiel) : Volet 3 (lead)
- Sanon (Djelia) : Volet 2 (lead)
*(à ajuster selon les forces/préférences réelles de chacun — proposition de départ, pas figée)*

## 4. Décisions à obtenir avant la fin de la réunion
- [ ] Les 8 hypothèses de la Phase 1 sont tranchées et notées
- [ ] Chacun sait ce qu'il fait demain matin
- [ ] Date/heure du point quotidien de 1h confirmée
- [ ] Accès Jira et Git confirmés pour les 3 membres

## 5. Prochaine réunion
Date, objectif (normalement : Jalon 2 — comparaison architecturale).
