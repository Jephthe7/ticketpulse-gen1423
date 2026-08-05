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

## 3. Répartition des responsabilités — validée en réunion
- Fedjo : Volet 1 (responsable) + Volet 2 (co-responsable)
- Ezekiel : Volet 2 (responsable)
- Djelia : Volet 3 (responsable) + Volet 2 (co-responsable)

## 4. Décisions obtenues en fin de réunion
- [x] Les 8 hypothèses de conception sont tranchées et notées (voir `docs/volet2-architecture/hypotheses.md`)
- [x] Répartition des responsabilités confirmée
- [x] Point quotidien fixé à 1h/jour
- [x] Accès Jira et Git confirmés pour les 3 membres

## 5. Prochaine réunion
5 août — Jalon 2 : comparaison architecturale et choix retenu.

## Statut

Réunion réalisée le 4 août 2026.

Les hypothèses validées sont documentées dans :
docs/volet2-architecture/hypotheses.md

Le compte rendu est disponible dans :
docs/volet1-agile/compte-rendu-reunion-1.md
