# Registre des risques — TicketPulse

| # | Catégorie | Risque | Prob. | Impact | Mitigation / suivi | Responsable | État au 12 août |
|---|---|---|---|---|---|---|---|
| 1 | Technique | Capacité insuffisante au pic de 50 000 req/s | Moy. | Élevé | Instances sans état, cache, marge de 30 %, plan de charge PERF-02 | Ezékiel | À valider par test |
| 2 | Technique | Double-vente sous concurrence | Moy. | Élevé | Transaction, verrou de ligne, contrainte, test PERF-03 | Djelia + Ezékiel | Contrôle conçu |
| 3 | Organisationnel | Indisponibilité d'un membre | Élevée | Moyen | Redistribution et revue asynchrone | Wilfried | Surveillé |
| 4 | Organisationnel | Dépendance à une seule personne | Moy. | Élevé | Branches individuelles, PR et revues croisées | Équipe | Réduit |
| 5 | Sujet/exigence | Hypothèses de charge erronées | Moy. | Élevé | Hypothèses explicites et validation expérimentale | Ezékiel | À valider par test |
| 6 | Sujet/exigence | Paiement ou données sensibles mal protégés | Faible | Élevé | Prestataire externe, TLS, idempotence, aucun stockage de carte | Djelia | Contrôle conçu |
| 7 | Sujet/exigence | Bots et blocage abusif des sièges | Moy. | Moyen | Limite de huit sièges, rate limiting, expiration | Djelia | Contrôle conçu |
| 8 | Sujet/exigence | Paiement dépassant cinq minutes | Moy. | Moyen | Réservation expirante et suivi de latence | Ezékiel | À valider par test |

Un risque traité reste dans le registre afin de préserver la trace du suivi.
