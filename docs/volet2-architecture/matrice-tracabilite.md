# Matrice de traçabilité — TicketPulse

| Exigence | Jira | Décision/composant | Diagrammes | Validation |
|---|---|---|---|---|
| Recherche d'événements | Backlog initial / SCRUM-18 | Module Recherche + cache | Cas d'utilisation, architecture | TF01, SA01, charge lecture |
| Plan de salle en temps réel | Backlog initial / SCRUM-18 | Recherche + cache, base source de vérité | Cas d'utilisation, architecture | TF03, latence < 2 s pour la majorité |
| Réservation temporaire | Backlog initial / SCRUM-18 | Module Réservation, état `HELD`, expiration | Séquence réservation | TU01, TU03, SA02, SA08 |
| Aucune double-vente | Backlog initial / SCRUM-12 | Transaction, verrou de ligne, contrainte d'intégrité | Séquence réservation | TU02, TF08, SA03, test concurrent |
| Paiement sécurisé | Backlog initial / SCRUM-13 | Prestataire externe, TLS, idempotence | Séquence paiement | TI01, M03, tests succès/refus/rejeu |
| Paiement < 5 min | Backlog initial / SCRUM-12 | Expiration de réservation | Séquence paiement | SA09 |
| Billet électronique et QR Code | Backlog initial / SCRUM-12 | Module Billetterie | Séquence paiement | TU04, TU05, TF06, SA06 |
| Confirmation par courriel | Backlog initial / SCRUM-12 | Module Notification + service externe | Architecture, séquence paiement | TI04, TF07, SA07 |
| 250 000 utilisateurs / 50 000 req/s | SCRUM-6 | Instances sans état, répartiteur, cache | Architecture globale | Scénarios PERF-01 à PERF-04 |
| Majorité des réponses < 2 s | SCRUM-6 / SCRUM-14 | Index, cache, autoscaling | Architecture globale | SA10, p95 < 2 s proposé |
| Disponibilité et reprise | SCRUM-14 / SCRUM-15 | health checks, réplication, sauvegardes | Architecture globale | P01 à P08, tests de restauration |

Les identifiants issus du CSV sont décrits comme « backlog initial » lorsque l'identifiant Jira final n'est pas visible dans les preuves disponibles. Aucun identifiant n'est inventé.

