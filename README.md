# Hippios Specs - Sommaire et Contexte

## Contexte du Projet

Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux.
L’objectif est d’anticiper les problèmes de santé, réduire les coûts vétérinaires et améliorer le bien-être animal. L’application centralise toutes les informations liées à la santé, l’alimentation et le budget, tout en proposant des recommandations personnalisées et des outils connectés pour un suivi en temps réel.
Cette approche s’inscrit dans une volonté de moderniser la gestion quotidienne du cheval grâce à la data et aux objets connectés.

Ce dossier centralise l'ensemble des spécifications fonctionnelles et techniques du projet **Hippios**. L'objectif principal de ce projet est de proposer une application (et un site web) de suivi et de gestion connectée pour les chevaux. 

Mettant en relation des utilisateurs (propriétaires, cavaliers) avec un dispositif de santé/physique pour cheval, le système permet de :
- Gérer son profil utilisateur (inscription, modification, suppression, paiement).
- Gérer ses chevaux (ajout, modification, suppression) et y associer des capteurs.
- Consulter et analyser des données de santé remontées par des capteurs physiques attachés aux chevaux.
- Simplifier le suivi vétérinaire via un carnet de santé, l'envoi/stockage de documents, un agenda de rendez-vous et des fiches de contacts vétérinaires.

Le projet couvre deux pans majeurs :
1. **La partie vitrine/compte utilisateur (Site)** : pour s'inscrire, gérer son compte et son abonnement.
2. **L'application mobile/web (App)** : pour le pilotage quotidien, le dashboard, les métriques des capteurs et les données de l'animal.

---

## Sommaire des Spécifications

Voici la liste des spécifications détaillées classées par ordre de fonctionnalité :

### Site - Gestion Utilisateur et Paiement
- [Spec site: Creer user](README-spec-creer-user.md)
- [Spec site: Modifer user (Connexion)](README-spec-modifer-user.md)
- [Spec site: Modifer user (Profil)](README-spec-modifer-user.md)
- [Spec site: Supprimer user](README-spec-supprimer-user.md)
- [Spec site: Payement](README-spec-payement.md)
- [Spec : Connexion](README-spec-connexion.md)
- [Spec : Deconnexion](README-spec-deconnexion.md)

### App - Carnet de santé et Documents
- [Spec app: Carnet de santé agenda](README-spec-carnet-de-sante-agenda.md)
- [Spec app: Stockage des docs + téléchargements](README-spec-stockage-des-docs-telechargements.md)
- [Spec app: Veterinaire](README-spec-veterinaire.md)
- [Spec : Créer un rendez-vous](README-spec-creer-un-rendez-vous.md)
- [Spec : Consulter un rendez-vous](README-spec-consulter-un-rendez-vous.md)
- [Spec : Modifier un rendez-vous](README-spec-modifier-un-rendez-vous.md)
- [Spec : Supprimer un rendez-vous](README-spec-supprimer-un-rendez-vous.md)
- [Spec : Stockage des docs](README-spec-stockage-des-docs.md)
- [Spec app : Téléchargement des docs](README-spec-telechargement-des-docs.md)

### App - Gestion des Chevaux et Capteurs
- [Spec app: Ajouter un cheval](README-spec-ajouter-un-cheval.md)
- [Spec app: Modifier un cheval](README-spec-modifier-un-cheval.md)
- [Spec app: Supprimer un cheval](README-spec-supprimer-un-cheval.md)

### App - Dashboard et Métriques
- [Spec app: Dashboard “accueil”](README-spec-dashboard-accueil.md)
- [Spec app: Dashboard un cheval](README-spec-dashboard-un-cheval.md)
- [Spec app: Historique des métrics](README-spec-historique-des-metrics.md)
