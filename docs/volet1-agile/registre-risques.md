# Registre des risques — TicketPulse

| # | Catégorie | Risque | Probabilité | Impact | Mitigation | Statut |
|---|---|---|---|---|---|---|
| 1 | Technique | Le monolithe modulaire ne tient pas la charge au-delà du volume dimensionné (250k connectés / 50k req/s) | Moyenne | Élevé | Instances applicatives sans état derrière un répartiteur de charge, mise à l'échelle horizontale, cache pour les lectures | Ouvert |
| 2 | Technique | Mauvaise gestion de la concurrence sur la réservation → un même siège vendu deux fois | Moyenne | Élevé | Transactions strictes / verrouillage sur le module de réservation isolé | Ouvert |
| 3 | Organisationnel/humain | Indisponibilité d'un membre (conflit avec d'autres cours) retardant une tâche critique | Élevée | Moyen | Plan de continuité nommé dans CONTRIBUTING.md, tâches redistribuées au point quotidien | Ouvert |
| 4 | Organisationnel/humain | Dépendance excessive à une seule personne sur une partie clé (bus factor) | Moyenne | Élevé | Co-responsabilité sur le Volet 2 (Ezekiel + Djelia), revue croisée avant tout merge | Ouvert |
| 5 | Sujet/exigences | Hypothèses de charge mal estimées, faussant le dimensionnement | Moyenne | Élevé | Hypothèses documentées et justifiées dans `hypotheses.md`, validées en équipe | Ouvert |
| 6 | Sujet/exigences | Paiement non sécurisé correctement (conformité, données sensibles) | Faible | Élevé | Délégation du traitement de carte à un fournisseur tiers (tokenisation), aucune donnée de carte stockée | Ouvert |
| 7 | Sujet/exigences | Achats automatisés massifs par des bots (scalping) faussant l'équité de la vente | Moyenne | Moyen | Limite de 8 sièges/transaction, rate limiting, file d'attente virtuelle | Ouvert |
| 8 | Sujet/exigences | Non-respect du délai de finalisation de 5 minutes sous forte charge | Moyenne | Moyen | Réservation temporaire avec expiration stricte, libération immédiate en cas d'échec de paiement | Ouvert |

*Un risque traité/écarté reste dans le registre, marqué "Traité" — jamais supprimé.*
