# Dimensionnement du système — TicketPulse

## 1. Objectif

Le dimensionnement vérifie que l'architecture retenue peut absorber le profil de charge imposé par le sujet, tout en respectant les contraintes de réservation concurrente et de temps de réponse.

Les hypothèses chiffrées utilisées proviennent de `hypotheses.md`, qui constitue la source unique de vérité.

## 2. Données de référence

| Paramètre | Valeur |
|---|---:|
| Utilisateurs simultanés au pic | 250 000 |
| Requêtes/seconde au pic | 50 000 req/s |
| Ratio lecture/écriture | 90% / 10% |
| Requêtes de lecture au pic | 45 000 req/s |
| Requêtes d'écriture au pic | 5 000 req/s |
| Taille moyenne requête | ~2 Ko |
| Taille moyenne réponse | ~5 Ko |
| Conversion connectés → acheteurs | 20% |
| Taille moyenne d'un événement | 50 000 sièges |
| Maximum de sièges par transaction | 8 |
| Disponibilité visée | 99,9% |
| Majorité des réponses | < 2 s |

## 3. Débit applicatif

`QPS_total = 50 000 req/s`

`QPS_lecture = 50 000 × 0,90 = 45 000 req/s`

`QPS_écriture = 50 000 × 0,10 = 5 000 req/s`

Les 5 000 écritures/s représentent les requêtes d'écriture au niveau API. Elles ne doivent pas être interprétées comme 5 000 achats/s : une réservation ou un paiement peut produire plusieurs opérations internes.

## 4. Volume de données réseau

### Requêtes entrantes

`50 000 × 2 Ko = 100 000 Ko/s ≈ 100 Mo/s`

Soit environ `0,8 Gbit/s`.

### Réponses sortantes

`50 000 × 5 Ko = 250 000 Ko/s ≈ 250 Mo/s`

Soit environ `2,0 Gbit/s`.

### Total applicatif approximatif

`100 + 250 = 350 Mo/s`, soit environ `2,8 Gbit/s`.

Cette estimation exclut les en-têtes HTTP, TLS, retransmissions, logs, métriques et trafic interne. Une marge opérationnelle est donc nécessaire dans un déploiement réel.

## 5. Dimensionnement de l'inventaire

Un événement moyen contient 50 000 sièges.

États minimaux proposés :

- `AVAILABLE` : disponible ;
- `HELD` : réservation temporaire ;
- `SOLD` : vendu.

Transitions :

`AVAILABLE → HELD → SOLD`

En cas d'échec ou d'expiration :

`HELD → AVAILABLE`

La transition critique doit être atomique et protégée par une contrainte d'intégrité afin d'empêcher deux ventes du même siège.

## 6. Conversion et volume d'acheteurs

`250 000 × 20% = 50 000 acheteurs`

Ce résultat représente le nombre d'acheteurs correspondant à l'hypothèse de conversion. Il ne permet pas de calculer directement des transactions/seconde, car la durée de la vente et la répartition temporelle des achats ne sont pas fournies.

Il serait donc incorrect d'affirmer que 50 000 acheteurs impliquent 50 000 transactions/s.

## 7. Latence

L'objectif est que la majorité des requêtes réponde en moins de 2 secondes.

Les lectures non critiques doivent être optimisées par indexation, cache et séparation du chemin de lecture.

Pour la réservation, la priorité est la cohérence : une réponse légèrement plus lente vaut mieux qu'une double-vente.

## 8. Disponibilité

Objectif : 99,9%.

Sur 30 jours :

`30 × 24 × 60 = 43 200 minutes`

Budget d'indisponibilité :

`43 200 × 0,001 = 43,2 minutes`

L'objectif correspond donc à environ **43 minutes d'indisponibilité maximale par mois**.

## 9. Conséquences architecturales

1. Optimiser fortement les lectures.
2. Conserver une frontière transactionnelle forte autour de la réservation.
3. Isoler logiquement le module de réservation.
4. Permettre la réplication horizontale de l'application.
5. Utiliser la base transactionnelle comme source de vérité.
6. Ajouter un mécanisme de limitation/queue devant les opérations critiques si les tests montrent un dépassement de capacité.

## 10. Limites

Le dimensionnement est analytique et doit être validé par un prototype.

Restent à mesurer :

- CPU et mémoire par instance ;
- IOPS de la base ;
- taille physique des données ;
- capacité réelle du cache ;
- nombre de connexions simultanées ;
- latence réelle de la base ;
- comportement réel sous 50 000 req/s.

Ces éléments relèvent des tests de charge du volet 3.
