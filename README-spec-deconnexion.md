# Spec: Deconnexion

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux. (Voir le README principal pour plus de détails).

### **Objectifs de la fonctionnalité :**
Permettre à un utilisateur connecté de se déconnecter de l'application afin de mettre fin à sa session et de sécuriser l'accès à son compte.

### **Acteurs impliqués :**
- Utilisateur
- Système

### **Fonctionnalité et description détaillée :**
Permet à un utilisateur authentifié de se déconnecter explicitement de l'application. La session est détruite côté serveur et les données locales de session sont effacées de l'appareil. L'utilisateur est redirigé vers la page de connexion. La déconnexion peut également survenir automatiquement en cas d'expiration de la session.

### **Etapes du flux principal :**
1. L'utilisateur accède aux paramètres ou au menu de son profil
2. L'utilisateur appuie sur le bouton "Se déconnecter"
3. Le système affiche une modale de confirmation
4. L'utilisateur confirme la déconnexion
5. Le système invalide le token de session côté serveur
6. Le système efface les données de session stockées localement sur l'appareil
7. L'utilisateur est redirigé vers la page de connexion

### **Scénarios alternatifs et exceptions :**
- L'utilisateur annule depuis la modale de confirmation → aucune action effectuée, retour à l'écran précédent
- La session a déjà expiré au moment de la déconnexion → le système efface les données locales et redirige vers la page de connexion sans erreur
- Une erreur serveur survient lors de l'invalidation du token → les données locales sont tout de même effacées et l'utilisateur est redirigé vers la page de connexion par sécurité

### **Règles de gestion :**
- RG-01 : L'utilisateur doit être authentifié pour accéder à cette fonctionnalité
- RG-02 : Une confirmation explicite est demandée avant de procéder à la déconnexion
- RG-03 : Le token de session est invalidé côté serveur lors de la déconnexion
- RG-04 : Toutes les données de session stockées localement sur l'appareil sont effacées à la déconnexion
- RG-05 : Après déconnexion, l'accès aux pages protégées est impossible sans nouvelle authentification
- RG-07 : En cas d'erreur serveur, la déconnexion locale est tout de même effectuée par sécurité
- RG-08 : Un utilisateur déconnecté tentant d'accéder à une page protégée est redirigé vers la page de connexion

### **Interface utilisateur :**
- Le bouton "Se déconnecter" est accessible depuis le menu profil ou les paramètres de l'application
- Le bouton est affiché en rouge pour signaler une action importante
- Une modale de confirmation est affichée avec un message explicite ("Êtes-vous sûr de vouloir vous déconnecter ?")
- Un bouton "Annuler" est disponible dans la modale pour abandonner l'action
- Un indicateur de chargement est affiché pendant le traitement de la déconnexion
- En cas de déconnexion automatique par expiration de session, un message informatif est affiché sur la page de connexion ("Votre session a expiré, veuillez vous reconnecter")

### **Cas de test pour la validation :**
- CT-01 : Déconnexion réussie après confirmation → session invalidée, données locales effacées, redirection vers la page de connexion
- CT-02 : Annulation depuis la modale → aucune action effectuée, retour à l'écran précédent
- CT-03 : Tentative d'accès à une page protégée après déconnexion → redirection vers la page de connexion
- CT-05 : Erreur serveur lors de l'invalidation du token → données locales effacées, redirection vers la page de connexion malgré l'erreur
- CT-06 : Vérification que le token invalidé ne permet plus d'accéder aux routes protégées de l'API
- CT-07 : Déconnexion alors que la session est déjà expirée → données locales effacées, redirection sans message d'erreur

### **Post-conditions :**
- En cas de succès : le token de session est invalidé côté serveur, les données locales sont effacées et l'utilisateur est redirigé vers la page de connexion
- En cas d'expiration automatique : les données locales sont effacées, un message d'information est affiché et l'utilisateur est redirigé vers la page de connexion
- En cas d'erreur serveur : les données locales sont effacées par sécurité et l'utilisateur est redirigé vers la page de connexion
