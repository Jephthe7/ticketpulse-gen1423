# Stratégie d'observabilité

**Ticket source :** SCRUM-14  
**Contribution principale :** Djelia Hanifah Sanon

## Signaux

| Signal | Contenu |
|---|---|
| Métriques | débit, latences, erreurs, CPU, mémoire, connexions, verrous, réservations et paiements |
| Journaux | événements structurés, horodatage, niveau, composant et identifiant de corrélation |
| Traces | parcours recherche–réservation–paiement–billet–notification |

Les journaux ne doivent contenir ni données de carte, ni secrets, ni renseignements personnels non nécessaires.

## Alertes proposées

| Alerte | Condition indicative | Priorité |
|---|---|---|
| ALT01 | p95 dépasse 2 s pendant 5 min | Haute |
| ALT02 | Taux d'erreur dépasse 1 % pendant 5 min | Haute |
| ALT03 | Base ou paiement indisponible | Critique |
| ALT04 | Paiements échoués anormalement nombreux | Haute |
| ALT05 | CPU/mémoire au-dessus de 80 % durablement | Haute |
| ALT06 | Billet absent après paiement confirmé | Critique |
| ALT07 | Hausse anormale du trafic ou des réservations | Haute |

Ces seuils sont des valeurs de départ à ajuster à partir des mesures de charge.

