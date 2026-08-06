# ADR-001 : Choix de l'architecture — monolithe modulaire

**Date :** 05 août 2026
**Statut :** Acceptée
**Auteurs :** Ezekiel (proposition technique) · **Validée par :** équipe entière (Fedjo, Ezekiel, Djelia)

## Contexte
TicketPulse doit absorber un pic de 250 000 utilisateurs connectés simultanément et jusqu'à 50 000 requêtes/seconde pendant 10 minutes (profil estimé à 90% lecture / 10% écriture), tout en garantissant qu'aucun siège n'est vendu deux fois. Le projet doit être conçu par une équipe de 3 personnes en 8 jours.

## Options considérées
1. **Monolithe modulaire** — application unique, modules internes séparés (recherche, réservation, paiement, utilisateurs)
2. **Architecture orientée événements** — services séparés communiquant par file d'attente/événements

## Décision
Monolithe modulaire, déployé en plusieurs instances identiques sans état derrière un répartiteur de charge, avec un module de réservation isolé utilisant des transactions strictes pour garantir la cohérence.

## Justification
- Développement et intégration plus rapides, adaptés au délai de 8 jours
- Le trafic étant majoritairement en lecture (90%), la scalabilité horizontale des instances applicatives (sans état) et une couche de cache suffisent à absorber l'essentiel du pic sans la complexité d'une architecture distribuée
- Le chemin d'écriture (réservation), plus sensible, reste isolé et protégé par des transactions/verrous garantissant qu'un siège n'est jamais vendu deux fois

## Alternative rejetée et pourquoi
Architecture orientée événements rejetée à ce stade : meilleure absorption théorique des pics et isolation des pannes, mais complexité de conception, d'implémentation et de test nettement supérieure — risque jugé trop élevé compte tenu du délai du projet, sans bénéfice proportionnel vu le profil de charge majoritairement en lecture.

## Conséquences
- Limite connue et assumée : au-delà du volume dimensionné pour les instances applicatives et la base de données, le système peut se dégrader ; cette limite doit être chiffrée dans le dimensionnement et testée en charge, pas seulement mentionnée
- Le module de réservation doit être conçu avec une attention particulière à la gestion de la concurrence (verrouillage/transactions)
- Diagrammes et tests doivent refléter explicitement la séparation lecture/écriture décrite ci-dessus
