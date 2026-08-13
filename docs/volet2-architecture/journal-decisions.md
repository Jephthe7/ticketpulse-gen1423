# Journal des décisions — TicketPulse

## Journal

| Date | Décision | Pourquoi | Alternative rejetée | Impact |
|---|---|---|---|---|
| 05 août | Architecture retenue : monolithe modulaire, instances multiples sans état derrière un répartiteur de charge, module réservation isolé avec transactions strictes | Rapidité/simplicité adaptées au délai de 8 jours ; trafic majoritairement en lecture (90%) donc scalabilité horizontale suffisante sans complexité distribuée | Architecture orientée événements — meilleure absorption théorique des pics mais complexité et risque trop élevés pour le délai | Dimensionnement séparé lecture/écriture ; diagramme d'architecture avec répartiteur + cache ; tests de charge + tests de concurrence ciblés sur la réservation. Détail dans `ADR-001-choix-architecture.md` |
| 05 août | Base relationnelle transactionnelle comme source de vérité | Transactions ACID, contraintes et verrouillage nécessaires pour l'inventaire | Cache ou stockage non transactionnel comme autorité de vente | Les réservations et ventes sont confirmées uniquement dans la base. |
| 05 août | Cache limité aux lectures | Réduire la pression de 45 000 lectures/s estimées | Cache utilisé pour confirmer une disponibilité | Une donnée de cache peut être obsolète ; la vente exige une validation transactionnelle. |
| 06 août | Paiement délégué à un prestataire externe | Réduire l'exposition aux données de carte et bénéficier d'un service spécialisé | Traitement direct des cartes | Références fournisseur et webhooks traités de manière idempotente. |
| 06 août | État canonique d'une réservation temporaire : `HELD` | Unifier le vocabulaire entre données, séquences et tests | Alternance entre `RESERVED` et `HELD` | Transitions `AVAILABLE → HELD → SOLD`, ou retour à `AVAILABLE`. |
| 10 août | Objectif de disponibilité de 99,9 % | Cible de conception réaliste et mesurable pour le projet | Promesse supérieure non étayée | Environ 43 minutes d'indisponibilité mensuelle théorique ; surveillance et reprise requises. |
