# Scénarios de panne et plan de reprise

**Ticket source :** SCRUM-15  
**Contribution principale :** Djelia Hanifah Sanon

| Panne | Détection | Réponse immédiate | Reprise |
|---|---|---|---|
| Prestataire de paiement indisponible | erreurs et health check | ne pas confirmer l'achat | vérifier les transactions incertaines avant reprise |
| Base indisponible | connexions en échec | suspendre les écritures critiques | basculer/restaurer puis contrôler la cohérence |
| Instance applicative en panne | health check | retirer l'instance | répartir sur les instances saines et remplacer |
| Surcharge | latence, files, saturation | limiter le trafic non prioritaire | ajouter de la capacité et rétablir progressivement |
| Courriel indisponible | erreurs d'envoi | conserver l'achat valide | mettre en file et réessayer |
| Génération du billet en panne | billet absent après paiement | conserver l'achat confirmé | relancer de manière idempotente |
| Communication interrompue | timeouts et traces incomplètes | empêcher les transitions ambiguës | reprendre après vérification de l'état |
| Données corrompues | contrôles d'intégrité | isoler la source | restaurer puis valider les invariants |

## Cas critique : paiement incertain

Si le prestataire a peut-être débité le client mais que TicketPulse n'a pas reçu la confirmation, la transaction passe à `PAYMENT_PENDING_REVIEW`. Le système interroge ensuite le prestataire avec la référence idempotente avant de confirmer ou d'annuler.

## Objectifs proposés

- **RTO réservation/paiement :** 15 minutes ;
- **RTO consultation :** 30 minutes ;
- **RPO transactions confirmées :** proche de zéro grâce à la réplication et au journal transactionnel ;
- **RPO données non critiques :** 15 minutes.

Ces objectifs sont des hypothèses de conception, l'énoncé ne fournissant aucun RTO/RPO.

