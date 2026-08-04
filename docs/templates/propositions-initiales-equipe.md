# Propositions initiales de l'équipe — à valider ou ajuster (Réunion 1)

*Préparé par Fedjo avant la réunion. Rien n'est figé — chaque proposition doit être discutée, chacun peut proposer un ajustement. Pendant la réunion : marquer ✅ Acceptée / 🟡 Acceptée avec modification / ❌ Rejetée, et noter pourquoi dans `docs/volet2-architecture/journal-decisions.md`.*

## Proposition 1 — Hypothèses de charge (les 8 questions)

| # | Question | Proposition | Justification courte |
|---|---|---|---|
| 1 | Taux de conversion connectés → acheteurs réels | ~20% | Cohérent avec l'hypothèse 2 : 50 000 sièges vendus sur 250 000 connectés en pic |
| 2 | Taille moyenne d'un événement (sièges) | 50 000 sièges | Ordre de grandeur d'un grand stade/festival, cohérent avec le profil de charge donné |
| 3 | Sièges max par transaction | 8 | Limite courante dans l'industrie billetterie pour limiter le scalping |
| 4 | Comportement si paiement échoue après réservation | Libération immédiate du siège | Priorise l'équité (pas de siège bloqué inutilement) plutôt qu'un délai de grâce |
| 5 | Cohérence vs disponibilité (CAP) | Cohérence forte (CP) sur la réservation de siège ; disponibilité (AP) sur la recherche/consultation | La non-surréservation est une exigence dure ; la recherche peut tolérer une donnée légèrement obsolète |
| 6 | Ratio lecture/écriture pendant le pic | ~90% lecture / 10% écriture | Typique d'une vente flash : beaucoup de rafraîchissement du plan de salle pour peu de réservations effectives |
| 7 | Taille moyenne requête/réponse API | ~2 Ko requête / ~5 Ko réponse | Ordre de grandeur raisonnable pour un payload JSON avec fragment de plan de salle |
| 8 | Budget de disponibilité visé | 99.9% | Standard SaaS réaliste pour une équipe de 3, sans sur-ingénierie inutile |

**Statut :** 🟡 À discuter en réunion — proposition de départ seulement, chacun est invité à challenger un chiffre avant qu'on le fige.

## Proposition 2 — Calendrier
Voir `docs/templates/calendrier-interne.md`. Question à poser en réunion : est-ce réaliste vu vos disponibilités respectives cette semaine (autres cours) ? Faut-il décaler une journée ?

## Proposition 3 — Répartition des rôles
Voir `docs/templates/ordre-du-jour-reunion-1.md`, section 3. Question à poser : est-ce que ça correspond à vos forces/préférences, ou quelqu'un préfère échanger un volet ?

## Proposition 4 — Style architectural (à discuter, pas à trancher aujourd'hui)
Deux pistes à comparer sérieusement en Jalon 2 (5-6 août), pas aujourd'hui :
- Monolithe modulaire avec composant de réservation isolé et verrouillage optimiste
- Architecture orientée événements (file d'attente virtuelle + service de réservation dédié)
**Statut :** 🟡 Non tranché — nécessite le dimensionnement pour arbitrer correctement, prévu au Jalon 2.
