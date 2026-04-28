# Spec (4) site: Supprimer user

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux.
L’objectif est d’anticiper les problèmes de santé, réduire les coûts vétérinaires et améliorer le bien-être animal. L’application centralise toutes les informations liées à la santé, l’alimentation et le budget, tout en proposant des recommandations personnalisées et des outils connectés pour un suivi en temps réel.
Cette approche s’inscrit dans une volonté de moderniser la gestion quotidienne du cheval grâce à la data et aux objets connectés.

### **Objectifs de la fonctionnalité :**

Permettre la suppression définitive d'un compte utilisateur et de toutes les données associées, dans le respect des règles de confidentialité

### **Acteurs impliqués :**

Utilisateur connecté
Administrateur
Système

### **Fonctionnalité et description détaillée :**

Permet à un utilisateur authentifié de supprimer son propre compte. Après confirmation, toutes les données personnelles associées au compte sont supprimées de la base de données. L'action est irréversible.

### **Etapes du flux principal :**

L'utilisateur accède à la page de gestion de son profil
L'utilisateur clique sur le bouton "Supprimer mon compte"
Le système affiche une modale de confirmation avertissant que l'action est irréversible
L'utilisateur confirme la suppression en saisissant son mot de passe actuel
Le système vérifie le mot de passe
Le système supprime le compte et toutes les données associées
La session est détruite
L'utilisateur est redirigé vers la page d'accueil avec un message de confirmation

### Scénario alternatifs et exception :

L'utilisateur annule la suppression depuis la modale de confirmation → aucune action effectuée, l'utilisateur reste sur son profil
Le mot de passe de confirmation est incorrect → message d'erreur affiché, suppression bloquée
La session a expiré avant la confirmation → l'utilisateur est redirigé vers la page de connexion, aucune suppression effectuée
Un administrateur supprime le compte d'un utilisateur → la suppression est effectuée sans saisie de mot de passe, un email de notification est envoyé à l'utilisateur concerné

### **Règles de gestion :**

RG-01 : L'utilisateur doit être authentifié pour accéder à cette fonctionnalité
RG-02 : L'utilisateur ne peut supprimer que son propre compte
RG-03 : Une confirmation explicite est obligatoire avant toute suppression (saisie du mot de passe actuel)
RG-04 : La suppression est irréversible, aucune restauration du compte n'est possible après confirmation
RG-05 : Toutes les données personnelles associées au compte doivent être supprimées conformément au RGPD
RG-06 : La session active de l'utilisateur est immédiatement détruite après la suppression
RG-07 : Un email de confirmation de suppression est envoyé à l'adresse associée au compte
RG-08 : Un compte administrateur ne peut pas être supprimé via cette interface

### **Interface utilisateur :**

Le bouton "Supprimer mon compte" est clairement distinct visuellement (couleur rouge) pour signaler une action destructrice
Une modale de confirmation est affichée avec un message d'avertissement explicite sur le caractère irréversible de l'action
Un champ de saisie du mot de passe actuel est présent dans la modale pour confirmer l'identité
Le bouton de confirmation dans la modale est désactivé tant que le champ mot de passe est vide
Un bouton "Annuler" est disponible dans la modale pour abandonner l'action
Un indicateur de chargement est affiché pendant le traitement de la suppression

### **Cas de test pour la validation :**

CT-01 : Suppression réussie avec mot de passe correct → compte supprimé, session détruite, redirection vers la page d'accueil
CT-02 : Tentative de suppression avec mot de passe incorrect → message d'erreur affiché, suppression bloquée
CT-03 : Annulation depuis la modale de confirmation → aucune action effectuée, retour au profil
CT-04 : Tentative d'accès à la fonctionnalité sans être connecté → redirection vers la page de connexion
CT-05 : Vérification post-suppression que les données personnelles sont bien effacées de la base de données
CT-06 : Vérification que l'email de confirmation de suppression est bien reçu
CT-07 : Tentative de connexion avec le compte supprimé → connexion refusée, message d'erreur affiché

### **Post-conditions :**

En cas de succès : le compte et toutes les données associées sont définitivement supprimés de la base de données, la session est détruite, un email de confirmation est envoyé et l'utilisateur est redirigé vers la page d'accueil
En cas d'échec : aucune donnée n'est supprimée, l'utilisateur reste sur son profil avec un message d'erreur approprié
