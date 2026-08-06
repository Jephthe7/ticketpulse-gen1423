# Réunion 2 — 05 août 2026

**Présents :** Fedjo, Ezekiel, Djelia

## Objectif
Trancher l'orientation architecturale à partir de la fiche de comparaison préparée (`docs/volet2-architecture/fiche-decision-architecture.md`).

## Décision
Architecture retenue : monolithe modulaire, instances multiples sans état derrière un répartiteur de charge, module de réservation isolé avec transactions strictes.
Détail et justification complète : `docs/volet2-architecture/ADR-001-choix-architecture.md`

## Compromis assumé
Le système reste sensible à une charge dépassant le volume dimensionné (au-delà des 250 000 utilisateurs/50 000 req/s de pointe). Ce n'est pas ignoré : ce sera chiffré dans le dimensionnement et vérifié par un test de charge dédié plutôt que laissé comme une faiblesse non traitée.

## Prochaines étapes (6-7 août)
- Dimensionnement séparé lecture/écriture (Ezekiel, appui Djelia)
- Diagramme d'architecture (répartiteur, instances, cache, base de données) et diagrammes de séquence
- Mise à jour de la matrice de traçabilité au fur et à mesure
