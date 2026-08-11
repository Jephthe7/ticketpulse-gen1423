# Compromis architecturaux — TicketPulse

| Décision | Bénéfice | Coût / risque accepté | Contrôle |
|---|---|---|---|
| Monolithe modulaire | Simplicité et cohérence | Scaling moins indépendant | Tests de charge + frontières strictes |
| Base relationnelle | ACID et intégrité | Scaling d'écriture plus complexe | Indexation + transactions |
| Cache des lectures | Réduit la charge | Donnée potentiellement obsolète | Jamais utilisé pour confirmer une vente |
| Cohérence forte réservation | Zéro double-vente | Coût transactionnel | Opération atomique + contrainte d'unicité |
| Paiement externe | Réduit l'exposition aux données bancaires | Dépendance fournisseur | Idempotence + gestion des reprises |
| Réservation temporaire | Libère l'inventaire après échec | Gestion des expirations | `expires_at` + job idempotent |
| Réplication applicative | Absorbe davantage de trafic | Besoin d'une donnée partagée cohérente | Base transactionnelle |
| 99,9% de disponibilité | Objectif réaliste | ≈43,2 min/mois d'indisponibilité théorique | Monitoring + sauvegardes |

## Solution rejetée

L'architecture orientée événements reste une option d'évolution si les mesures réelles justifient l'extraction de services.

Elle n'est pas retenue pour la version actuelle en raison de la complexité distribuée supplémentaire et du besoin de cohérence forte sur la réservation.
