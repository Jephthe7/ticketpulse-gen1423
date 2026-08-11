# Analyse des besoins — TicketPulse

## 1. Parties prenantes

| Partie prenante | Besoin principal |
|---|---|
| Acheteur | Rechercher un événement, voir les sièges, réserver et payer |
| Organisateur | Publier un événement et suivre son inventaire |
| Système de paiement | Autoriser ou refuser les transactions |
| Administrateur | Superviser les événements et les utilisateurs |
| Service de notification | Envoyer les confirmations |
| Équipe d'exploitation | Maintenir la disponibilité et diagnostiquer les incidents |

## 2. Besoins fonctionnels

Le système doit notamment permettre de :

1. rechercher des événements ;
2. consulter la disponibilité ;
3. consulter un plan de salle ;
4. réserver temporairement des sièges ;
5. empêcher les doubles achats ;
6. effectuer un paiement sécurisé ;
7. confirmer une réservation ;
8. générer un billet et un QR code ;
9. envoyer une confirmation par courriel ;
10. libérer les sièges lorsque le paiement échoue ou expire.

## 3. Besoins non fonctionnels

- 250 000 utilisateurs simultanés au pic ;
- 50 000 req/s au pic ;
- majorité des requêtes sous 2 s ;
- aucune surréservation ;
- disponibilité visée de 99,9% ;
- protection des données et du paiement ;
- comportement correct sous accès concurrents.

## 4. Contraintes

- équipe de trois ;
- délai académique court ;
- architecture explicable à l'oral ;
- absence de données de charge détaillées dans l'énoncé, d'où la nécessité d'hypothèses explicites.

## 5. Priorités

La priorité fonctionnelle est la réservation correcte : **un siège confirmé ne doit jamais être vendu deux fois**.

La performance de consultation vient ensuite : les lectures représentent 90% du trafic estimé et peuvent donc être optimisées par cache/indexation.

## 6. Traçabilité

Les hypothèses quantitatives sont centralisées dans `hypotheses.md`. Les décisions architecturales sont consignées dans `journal-decisions.md` et `fiche-decision-architecture.md`.
