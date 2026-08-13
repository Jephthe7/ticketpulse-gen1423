# Analyse de sécurité

**Ticket source :** SCRUM-13  
**Contribution principale :** Djelia Hanifah Sanon

## Objectifs

La sécurité protège la confidentialité des données, l'intégrité des réservations et billets, la disponibilité pendant les ventes flash et la traçabilité des opérations sensibles.

## Menaces et contrôles

| ID | Menace | Contrôles proposés |
|---|---|---|
| M01 | Compromission d'un compte | Authentification robuste, sessions sécurisées, limitation des tentatives. |
| M02 | Interception des communications | HTTPS/TLS. |
| M03 | Fraude ou rejeu du paiement | Prestataire externe, validation serveur, clé d'idempotence. |
| M04 | Double réservation | Transaction atomique, verrouillage et contrainte d'intégrité. |
| M05 | Déni de service | Rate limiting, filtrage et répartition de charge. |
| M06 | Injection | Validation des entrées et requêtes paramétrées. |
| M07 | Falsification d'un billet | Identifiant unique et validation serveur du QR Code. |
| M08 | Blocage abusif de sièges | Quotas, expiration et détection des comportements automatisés. |
| M09 | Privilèges excessifs | Contrôle par rôles et moindre privilège. |
| M10 | Données sensibles dans les journaux | Masquage, contrôle d'accès et politique de rétention. |

TicketPulse ne stocke pas les données de carte. Il conserve une référence fournisseur et l'état nécessaire au rapprochement. Les contrôles doivent être testés, notamment par rejeu de notifications, tentatives concurrentes, entrées malveillantes et saturation contrôlée.

## Références

- OWASP, *Application Security Verification Standard*.
- OWASP, *Authentication*, *Input Validation* et *Logging Cheat Sheets*.
- PCI Security Standards Council, documentation PCI DSS applicable au prestataire de paiement.

