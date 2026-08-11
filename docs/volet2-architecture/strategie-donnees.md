# Stratégie de données — TicketPulse

## 1. Objectifs

La stratégie doit :

- empêcher toute double-vente ;
- conserver un état fiable de l'inventaire ;
- supporter un trafic de lecture élevé ;
- permettre une récupération après incident.

La réservation est la zone où la cohérence est prioritaire.

## 2. Source de vérité

Une base relationnelle transactionnelle est retenue comme source de vérité pour les événements, sièges, réservations, commandes, paiements et données nécessaires aux utilisateurs.

Le choix est motivé par les garanties ACID et les contraintes d'unicité nécessaires à l'inventaire.

## 3. Modèle logique

### Event
`id`, `name`, `venue_id`, `starts_at`, `status`

### Seat
`id`, `event_id`, `section`, `row`, `number`, `status`

Contrainte : `UNIQUE(event_id, section, row, number)`.

### Reservation
`id`, `event_id`, `user_id`, `status`, `expires_at`, `created_at`

### ReservationItem
`reservation_id`, `seat_id`

Un siège ne peut être associé à deux réservations actives incompatibles.

### Payment
`id`, `reservation_id`, `provider_reference`, `status`, `created_at`

Aucune donnée bancaire sensible n'est stockée directement par TicketPulse.

### Ticket
Après paiement confirmé : commande finalisée, siège `SOLD`, billet généré et QR code associé.

## 4. Réservation concurrente

Flux :

1. vérifier l'état du siège ;
2. démarrer une transaction ;
3. verrouiller ou modifier atomiquement le siège ;
4. créer la réservation temporaire ;
5. valider la transaction ;
6. initier le paiement ;
7. après succès, passer à `SOLD` ;
8. après échec/expiration, revenir à `AVAILABLE`.

Une lecture suivie d'une écriture non protégée est interdite, car deux utilisateurs peuvent lire simultanément `AVAILABLE`.

## 5. Expiration

Chaque réservation temporaire possède `expires_at`.

Un traitement périodique libère les réservations expirées. Le traitement doit être idempotent : une même expiration traitée deux fois ne doit pas créer d'état incorrect.

## 6. Cache

Le cache peut servir pour :

- recherche d'événements ;
- informations statiques ;
- configuration de salle ;
- consultations non critiques.

Le cache ne peut jamais être la source d'autorité pour confirmer une vente.

## 7. Paiement

Le paiement est confié à un prestataire externe. TicketPulse conserve uniquement une référence de transaction, son état et les informations d'audit nécessaires.

Les notifications de paiement sont traitées de manière idempotente afin qu'une répétition ne finalise pas deux fois la même réservation.

## 8. Reprise après incident

Les données transactionnelles doivent bénéficier de sauvegardes, d'une restauration testée et d'une stratégie de réplication adaptée à l'objectif de disponibilité.

Le système doit pouvoir reconstruire son état transactionnel à partir de la base plutôt qu'à partir du cache.

## 9. Règles d'intégrité

- Un siège appartient à un seul événement.
- Un siège ne peut être vendu qu'une fois.
- Une réservation expirée ne peut pas être finalisée comme nouvelle réservation.
- Un paiement déjà traité ne doit pas être appliqué deux fois.
- Les opérations critiques doivent être idempotentes lorsque nécessaire.

## 10. Compromis

La cohérence est privilégiée sur le chemin de réservation, au prix d'un coût transactionnel plus élevé.

Les lectures peuvent accepter une légère obsolescence grâce au cache, mais cette obsolescence ne doit jamais permettre une double-vente.
