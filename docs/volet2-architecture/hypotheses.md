# Hypothèses — source unique de vérité

Ce fichier est la référence pour toute hypothèse chiffrée du projet. Les autres documents (dimensionnement, rapport, architecture) le référencent plutôt que d'en dupliquer les valeurs.

| # | Hypothèse | Valeur retenue | Justification | Validée le |
|---|---|---|---|---|
| 1 | Proportion des connectés en pic qui achètent réellement | 20% | Cohérente avec un événement de 50 000 sièges entièrement vendu sur 250 000 connectés simultanément | 04 août |
| 2 | Taille moyenne d'un événement (nombre de sièges) | 50 000 | Ordre de grandeur d'un grand stade ou festival, cohérent avec le profil de charge fourni | 04 août |
| 3 | Nombre maximal de sièges par transaction | 8 | Pratique courante dans l'industrie de la billetterie pour limiter la revente abusive | 04 août |
| 4 | Comportement si le paiement échoue après réservation | Libération immédiate du siège | Priorise l'équité d'accès plutôt qu'un délai de grâce qui immobiliserait l'inventaire | 04 août |
| 5 | Priorité cohérence vs disponibilité en cas de conflit | Cohérence forte sur la réservation ; disponibilité prioritaire sur la consultation | La non-surréservation est une exigence explicite et non négociable du sujet | 04 août |
| 6 | Ratio lecture/écriture pendant le pic de trafic | 90% lecture / 10% écriture | Profil typique d'une vente flash à forte demande | 04 août |
| 7 | Taille moyenne d'une requête/réponse API | ~2 Ko / ~5 Ko | Ordre de grandeur raisonnable pour un payload JSON avec fragment de plan de salle | 04 août |
| 8 | Niveau de disponibilité visé | 99,9% | Objectif cohérent avec un service transactionnel soumis à des pics de forte demande | 04 août |
