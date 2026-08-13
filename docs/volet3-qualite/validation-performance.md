# Plan de validation de la performance

## Objectif

Valider par expérimentation les hypothèses du dimensionnement, notamment la capacité supposée de 1 000 requêtes/s par instance, le nombre cible de 65 instances et le comportement de la base transactionnelle sous concurrence.

## Métriques

- débit total et débit lecture/écriture ;
- latences p50, p95 et p99 ;
- taux d'erreur et de timeout ;
- CPU, mémoire et saturation réseau ;
- connexions, temps de verrouillage et IOPS de la base ;
- taux de succès des réservations ;
- nombre de doubles ventes, qui doit rester à zéro.

## Scénarios proposés

| ID | Profil | Durée | Critères de succès proposés |
|---|---|---:|---|
| PERF-01 | Charge normale, montée progressive | 15 min | Erreurs < 1 %, p95 < 2 s. |
| PERF-02 | 45 000 lectures/s + 5 000 écritures/s | 10 min | Débit soutenu, p95 < 2 s pour la majorité des parcours, aucune double-vente. |
| PERF-03 | Concurrence extrême sur un même siège | 5 min | Un seul succès par siège, aucun état incohérent. |
| PERF-04 | Pic supérieur de 20 % | 5 min | Dégradation contrôlée, limitation du trafic, aucune corruption. |
| PERF-05 | Endurance à charge intermédiaire | 60 min | Pas de fuite mémoire ni croissance continue des files/connexions. |
| PERF-06 | Perte d'une instance applicative sous charge | 10 min | Retrait par health check, trafic repris par les instances saines. |

## Méthode

1. Préparer un jeu de données représentatif avec événements et sièges.
2. Exécuter un test de référence sur une instance.
3. Mesurer la capacité réelle plutôt que supposer 1 000 requêtes/s.
4. Répéter avec plusieurs instances et un cache.
5. Isoler la charge d'écriture et mesurer la contention de la base.
6. Comparer les résultats aux seuils et ajuster le dimensionnement.

Les seuils ci-dessus sont des objectifs de validation proposés. Ils ne sont pas des résultats mesurés.

