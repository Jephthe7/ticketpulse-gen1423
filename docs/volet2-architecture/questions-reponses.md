# Questions probables du professeur — Architecture

## Pourquoi un monolithe modulaire ?

Parce qu'il offre un meilleur compromis entre cohérence, simplicité, délai et maintenabilité pour une équipe de trois. Les modules restent séparés afin de permettre une extraction future si les mesures de charge le justifient.

## Pourquoi pas une architecture orientée événements ?

Elle absorbe mieux certains pics et permet un scaling indépendant, mais elle introduit une cohérence distribuée plus complexe. Pour l'exigence zéro double-vente, cette complexité n'est pas justifiée dans la première version.

## Comment empêchez-vous une double-vente ?

La base transactionnelle est la source de vérité. La réservation modifie l'état du siège de façon atomique, avec transaction et contrainte d'intégrité. Le cache ne participe pas à la décision finale de vente.

## Pourquoi une base relationnelle ?

Parce que la réservation nécessite des transactions ACID, des contraintes d'unicité et un état cohérent entre réservation, sièges et paiement.

## Que se passe-t-il si le trafic dépasse 50 000 req/s ?

Les lectures peuvent être absorbées par cache et réplication applicative. Si le module de réservation devient le goulot d'étranglement, un mécanisme de limitation/queue peut protéger la capacité transactionnelle. À plus long terme, le module pourrait être extrait.

## Que signifie 20% de conversion ?

Sur 250 000 utilisateurs simultanés, l'hypothèse donne 50 000 acheteurs. Elle ne signifie pas 50 000 achats par seconde : aucune durée de vente n'a été fournie.

## Pourquoi 99,9% ?

C'est un objectif réaliste pour le périmètre académique. Il correspond à environ 43,2 minutes d'indisponibilité théorique maximale sur 30 jours.

## Quelle est votre principale limite ?

Le dimensionnement est analytique. Le nombre exact d'instances, la capacité CPU/mémoire, les IOPS et la capacité réelle du cache doivent être validés par des tests de charge.
