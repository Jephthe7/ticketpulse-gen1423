# Analyse des besoins — TicketPulse

## Parties prenantes

| Partie prenante | Attentes principales |
|---|---|
| Acheteurs | Rechercher, consulter, réserver, payer et recevoir un billet. |
| Organisateurs | Publier des événements et suivre l'inventaire. |
| Gestionnaires TicketPulse | Assurer la viabilité, la qualité et la conformité du service. |
| Équipe technique et exploitation | Concevoir, surveiller, maintenir et rétablir le système. |
| Prestataire de paiement | Autoriser ou refuser les transactions de manière sécurisée. |
| Service de courriel | Acheminer les confirmations. |
| Autorités réglementaires | Encadrer les transactions et la protection des renseignements. |

## Besoins fonctionnels

| ID | Besoin |
|---|---|
| BF01 | Rechercher des événements. |
| BF02 | Consulter les billets disponibles. |
| BF03 | Consulter le plan de salle et l'état des sièges. |
| BF04 | Réserver temporairement jusqu'à huit sièges. |
| BF05 | Effectuer un paiement sécurisé. |
| BF06 | Générer un billet électronique et un QR Code. |
| BF07 | Envoyer une confirmation par courriel. |
| BF08 | Libérer une réservation échouée ou expirée. |

## Besoins non fonctionnels

| ID | Besoin |
|---|---|
| BNF01 | Disponibilité élevée pendant les ventes flash. |
| BNF02 | Majorité des réponses en moins de deux secondes. |
| BNF03 | Jusqu'à 250 000 utilisateurs simultanés. |
| BNF04 | Pointe de 50 000 requêtes/s pendant dix minutes. |
| BNF05 | Aucun siège vendu deux fois. |
| BNF06 | Paiement finalisé en moins de cinq minutes. |
| BNF07 | Paiement et données protégés. |
| BNF08 | Maintenabilité, observabilité et reprise après incident. |

## Contraintes et priorités

La cohérence de la réservation est prioritaire sur la disponibilité du chemin d'écriture. La consultation peut tolérer une légère obsolescence contrôlée par cache, mais la base transactionnelle demeure la source de vérité avant toute confirmation de vente.

Le projet consiste en une conception et une validation documentée ; aucun développement applicatif complet n'est demandé pour le Volet 3.

