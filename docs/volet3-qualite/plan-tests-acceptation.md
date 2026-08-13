# Plan de tests et scénarios d'acceptation

**Ticket source :** SCRUM-12  
**Contribution principale :** Djelia Hanifah Sanon

## Objectifs

La stratégie vérifie les fonctionnalités principales, les interactions entre composants et les propriétés critiques : absence de double-vente, paiement cohérent, génération du billet, expiration des réservations et temps de réponse.

## Niveaux de tests

| Niveau | Cible | Exemple |
|---|---|---|
| Unitaire | Règle métier isolée | transition `AVAILABLE → HELD`, expiration, QR Code |
| Intégration | Interaction entre modules | réservation–paiement, paiement–billetterie |
| Système | Parcours complet | recherche → siège → paiement → billet → courriel |
| Acceptation | Exigence observable | un seul gagnant pour deux demandes concurrentes |
| Régression | Stabilité après changement | paiement modifié sans casser réservation et billet |

## Scénarios prioritaires

| ID | Précondition et action | Résultat attendu |
|---|---|---|
| SA01 | Rechercher un événement existant | Résultats pertinents affichés. |
| SA02 | Sélectionner un siège disponible | Le siège passe temporairement à `HELD`. |
| SA03 | Deux clients réservent simultanément le même siège | Une seule réservation réussit. |
| SA04 | Paiement accepté pour une réservation valide | Achat confirmé une seule fois. |
| SA05 | Paiement refusé | Aucun billet ; siège libéré selon la règle d'échec. |
| SA06 | Paiement confirmé | Billet électronique et QR Code générés. |
| SA07 | Achat confirmé | Courriel de confirmation envoyé ou placé en reprise. |
| SA08 | Réservation arrivée à expiration | Siège remis à `AVAILABLE`. |
| SA09 | Parcours de paiement | Finalisation possible en moins de cinq minutes. |
| SA10 | Requête représentative sous charge | La majorité des réponses reste sous deux secondes. |

## Automatisation proposée

Les tests unitaires, d'intégration et les scénarios critiques sont exécutés dans une chaîne CI. Aucun résultat d'exécution réel n'est revendiqué : il s'agit d'une stratégie de validation pour l'architecture proposée.

