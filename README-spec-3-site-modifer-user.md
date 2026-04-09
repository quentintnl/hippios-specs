# Spec (3) site: Modifer user

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux.
L’objectif est d’anticiper les problèmes de santé, réduire les coûts vétérinaires et améliorer le bien-être animal. L’application centralise toutes les informations liées à la santé, l’alimentation et le budget, tout en proposant des recommandations personnalisées et des outils connectés pour un suivi en temps réel.
Cette approche s’inscrit dans une volonté de moderniser la gestion quotidienne du cheval grâce à la data et aux objets connectés.

### **Objectifs de la fonctionnalité :**

Permettre à l'utilisateur de mettre à jour ses informations personnelles (nom, email, mot de passe, etc.) depuis son espace personnel.

### **Acteurs impliqués :**

Utilisateur connecté

Système

### **Fonctionnalité et description détaillée :**

Permet à un utilisateur authentifié d'accéder à son profil et de modifier ses informations personnelles. Les modifications sont enregistrées en base de données après validation des données saisies.

### **Etapes du flux principal :**

L'utilisateur accède à la page de modification de son profil
Le système affiche le formulaire pré-rempli avec les informations actuelles de l'utilisateur
L'utilisateur modifie les champs souhaités
L'utilisateur soumet le formulaire
Le système valide les données saisies
Le système enregistre les modifications en base de données
Le système affiche un message de confirmation de la mise à jour

### Scénario alternatifs et exception :

Le nouvel email saisi est déjà utilisé par un autre compte → message d'erreur affiché, modification bloquée
Le format de l'email est invalide → message d'erreur affiché sous le champ concerné
Le nouveau mot de passe ne respecte pas les critères de sécurité → message d'erreur affiché, modification bloquée
Les deux champs de confirmation du mot de passe ne correspondent pas → message d'erreur affiché, envoi bloqué
L'utilisateur annule la modification → aucune donnée n'est modifiée, l'utilisateur est redirigé vers son profil
La session a expiré avant la soumission → l'utilisateur est redirigé vers la page de connexion, les modifications sont perdues

### **Règles de gestion :**

RG-01 : L'utilisateur doit être authentifié pour accéder à cette fonctionnalité
RG-02 : L'utilisateur ne peut modifier que son propre profil
RG-03 : L'adresse email doit respecter un format valide (ex: [user@domaine.com](mailto:user@domaine.com))
RG-04 : La nouvelle adresse email doit être unique dans le système
RG-05 : Si l'utilisateur souhaite modifier son mot de passe, il doit saisir son mot de passe actuel pour confirmer son identité
RG-06 : Le nouveau mot de passe doit contenir au minimum 8 caractères, dont une majuscule, un chiffre et un caractère spécial
RG-07 : La confirmation du nouveau mot de passe doit être identique au nouveau mot de passe saisi
RG-08 : Le mot de passe est stocké hashé en base de données

### **Interface utilisateur :**

Le formulaire est pré-rempli avec les informations actuelles de l'utilisateur
Les champs mot de passe sont vides par défaut et facultatifs (à remplir uniquement en cas de changement)
Les messages d'erreur sont affichés directement sous le champ concerné
Un bouton "Annuler" permet de revenir au profil sans enregistrer les modifications
Un indicateur de chargement est affiché pendant le traitement de la mise à jour
Un message de succès est affiché après l'enregistrement des modifications

### **Cas de test pour la validation :**

CT-01 : Modification réussie des informations avec des données valides → modifications enregistrées, message de confirmation affiché
CT-02 : Modification de l'email avec une adresse déjà existante → message d'erreur affiché, modification bloquée
CT-03 : Modification du mot de passe avec un mot de passe actuel incorrect → message d'erreur affiché, modification bloquée
CT-04 : Nouveau mot de passe ne respectant pas les critères de sécurité → message d'erreur affiché, envoi bloqué
CT-05 : Les deux champs nouveau mot de passe ne correspondent pas → message d'erreur affiché, envoi bloqué
CT-06 : Annulation de la modification → aucune donnée modifiée, retour au profil
CT-07 : Tentative d'accès à la page sans être connecté → redirection vers la page de connexion

### **Post-conditions :**

En cas de succès : les nouvelles informations sont enregistrées en base de données et le profil affiché est mis à jour
En cas de changement d'email : un email de confirmation est envoyé à la nouvelle adresse avant que la modification soit effective
En cas d'échec : aucune donnée n'est modifiée, l'utilisateur reste sur le formulaire avec les messages d'erreur appropriés
