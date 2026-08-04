> **Règle :** une ADR documente une décision **déjà validée en équipe**, jamais une idée à l'étude. Ne pas créer d'ADR tant que la décision n'a pas été actée en réunion — utiliser `decisions-en-attente.md` en attendant.

# ADR-XXX : [Titre court de la décision]

**Date :** JJ mois AAAA
**Statut :** Proposée / Acceptée / Rejetée / Remplacée par ADR-YYY
**Auteurs :** [qui a proposé] · **Validée par :** [qui a validé en équipe]

## Contexte
Quel problème force cette décision ? Quelles contraintes du sujet ou du profil de charge sont en jeu ?

## Options considérées
1. **Option A** — description courte
2. **Option B** — description courte
3. (autres options si pertinent)

## Décision
Quelle option est retenue.

## Justification
Pourquoi celle-ci plutôt que les autres, selon quels critères (performance, coût, complexité, délai, cohérence avec les autres choix déjà faits...).

## Alternative(s) rejetée(s) et pourquoi
Ne pas juste dire "moins bon" — expliquer concrètement le compromis perdu (ex. "microservices rejeté à ce stade : complexité opérationnelle trop élevée pour une équipe de 3 sur 8 jours, malgré une meilleure scalabilité théorique").

## Conséquences
Ce que ce choix implique pour la suite (composants à construire, tests à prévoir, limites connues, dette technique assumée).
