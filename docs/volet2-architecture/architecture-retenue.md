# Architecture retenue — TicketPulse

## Décision

L'équipe retient une **architecture de monolithe modulaire**.

L'application est déployée comme une unité, mais son code est séparé en modules ayant des responsabilités clairement définies.

## Modules

- **Recherche** : recherche et consultation des événements.
- **Réservation** : disponibilité réelle, réservation temporaire et concurrence.
- **Paiement** : interaction avec le prestataire externe et gestion des statuts.
- **Utilisateurs** : authentification, autorisation et profil.
- **Billetterie** : finalisation, billet et QR code.
- **Notification** : confirmations et envoi des informations.

## Flux critique

```text
Client
  ↓
API
  ↓
Module Réservation
  ↓
Base transactionnelle
  ↓
Réservation temporaire
  ↓
Module Paiement
  ↓
Prestataire de paiement
  ↓
Confirmation
  ↓
Réservation → SOLD
  ↓
Billet + QR code + notification
```

## Pourquoi le monolithe modulaire ?

### Cohérence

La réservation peut utiliser une même base transactionnelle et des transactions ACID, ce qui simplifie la garantie de zéro double-vente.

### Simplicité

Une équipe de trois personnes dispose d'un délai court. Le monolithe réduit le nombre de déploiements, contrats inter-services et mécanismes distribués.

### Évolution

Les frontières entre modules permettent d'extraire ultérieurement un module si les mesures de charge montrent qu'il devient un goulot d'étranglement.

### Charge

L'application peut être répliquée horizontalement. Les lectures peuvent être optimisées par cache et indexation.

## Pourquoi pas l'architecture orientée événements ?

Elle permettrait un scaling indépendant et une meilleure absorption de certains pics, mais introduirait :

- davantage de composants ;
- des problèmes de cohérence distribuée ;
- davantage de mécanismes de reprise ;
- une complexité opérationnelle plus élevée.

Pour une équipe de trois et un délai court, cette complexité est disproportionnée.

## Limite reconnue

Le monolithe est un compromis. Si les tests montrent qu'un module devient un goulot d'étranglement, il pourra être extrait ultérieurement.
