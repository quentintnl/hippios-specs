# Spec : Connexion

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux. (Voir le README principal pour plus de détails).

### **Objectifs de la fonctionnalité :**
Permettre à un utilisateur possédant un compte de s'authentifier sur l'application en saisissant son adresse email et son mot de passe afin d'accéder à son espace personnel.

### **Acteurs impliqués :**
- Utilisateur
- Système

### **Fonctionnalité et description détaillée :**
Permet à un utilisateur disposant d'un compte de se connecter à l'application via un formulaire composé d'un champ email et d'un champ mot de passe. Le système vérifie les identifiants saisis, crée une session et redirige l'utilisateur vers le dashboard d'accueil. Si les identifiants sont incorrects, un message d'erreur générique est affiché sans préciser lequel des deux champs est erroné pour des raisons de sécurité.

### **Etapes du flux principal :**
1. L'utilisateur ouvre l'application et accède à la page de connexion
2. L'utilisateur saisit son adresse email
3. L'utilisateur saisit son mot de passe
4. L'utilisateur appuie sur le bouton "Se connecter"
5. Le système vérifie le format de l'email
6. Le système vérifie les identifiants en base de données
7. Le système crée une session utilisateur
8. L'utilisateur est redirigé vers le dashboard d'accueil

### **Scénarios alternatifs et exceptions :**
- L'adresse email n'est pas au bon format → un message d'erreur est affiché sous le champ email, le formulaire n'est pas soumis
- Les identifiants sont incorrects (email inconnu ou mot de passe erroné) → un message d'erreur générique est affiché ("Email ou mot de passe incorrect") sans préciser lequel est erroné
- L'un des deux champs est vide → le bouton "Se connecter" est désactivé, le formulaire ne peut pas être soumis
- Le compte est temporairement bloqué suite à trop de tentatives échouées → un message informe l'utilisateur que son compte est bloqué temporairement et lui indique la durée restante
- L'utilisateur n'a pas encore de compte → un lien vers la page d'inscription est disponible
- L'utilisateur a oublié son mot de passe → un lien "Mot de passe oublié" est disponible sous le formulaire
- Une erreur serveur survient lors de la vérification → un message d'erreur générique est affiché, l'utilisateur est invité à réessayer

### **Règles de gestion :**
- RG-01 : Les deux champs email et mot de passe sont obligatoires pour soumettre le formulaire
- RG-02 : L'adresse email doit respecter un format valide (ex : [user@domaine.com](mailto:user@domaine.com))
- RG-03 : Le message d'erreur en cas d'identifiants incorrects est volontairement générique pour ne pas indiquer à un tiers si un email est enregistré sur la plateforme
- RG-04 : Après 5 tentatives de connexion échouées consécutives, le compte est temporairement bloqué pendant 15 minutes
- RG-05 : La session utilisateur expire après 30 minutes d'inactivité
- RG-06 : Le mot de passe n'est jamais affiché en clair, un bouton permet de le rendre visible temporairement
- RG-07 : Les échanges entre l'application et le serveur sont chiffrés (HTTPS)
- RG-08 : Un utilisateur déjà connecté accédant à la page de connexion est automatiquement redirigé vers le dashboard

### **Interface utilisateur :**
- Un champ email avec clavier adapté (type email) est affiché
- Un champ mot de passe masqué avec un bouton pour afficher/masquer la saisie est affiché
- Le bouton "Se connecter" est désactivé tant que les deux champs sont vides
- Un message d'erreur est affiché sous le formulaire en cas d'identifiants incorrects
- Un lien "Mot de passe oublié" est disponible sous le champ mot de passe
- Un lien "Créer un compte" est disponible pour rediriger vers la page d'inscription
- Un indicateur de chargement est affiché pendant la vérification des identifiants
- En cas de blocage du compte, un message indique la durée restante avant de pouvoir réessayer

### **Cas de test pour la validation :**
- CT-01 : Connexion réussie avec email et mot de passe valides → session créée, redirection vers le dashboard
- CT-02 : Tentative de connexion avec un email au format invalide → message d'erreur sous le champ email, formulaire non soumis
- CT-03 : Tentative de connexion avec un email inconnu → message d'erreur générique affiché
- CT-04 : Tentative de connexion avec un mot de passe incorrect → message d'erreur générique affiché
- CT-05 : Tentative de soumission avec un ou plusieurs champs vides → bouton désactivé, formulaire non soumis
- CT-06 : 5 tentatives échouées consécutives → compte bloqué temporairement, message informatif avec durée affiché
- CT-07 : Tentative de connexion avec un compte bloqué → accès refusé, message de blocage affiché
- CT-08 : Clic sur "Mot de passe oublié" → redirection vers la page de réinitialisation du mot de passe
- CT-09 : Clic sur "Créer un compte" → redirection vers la page d'inscription
- CT-10 : Utilisateur déjà connecté accédant à la page de connexion → redirection automatique vers le dashboard
- CT-11 : Erreur serveur lors de la vérification → message d'erreur générique affiché, aucune session créée

### **Post-conditions :**
- En cas de succès : une session utilisateur est créée, l'utilisateur est authentifié et redirigé vers le dashboard d'accueil
- En cas d'échec : aucune session n'est créée, l'utilisateur reste sur la page de connexion avec le message d'erreur approprié
- En cas de blocage : aucune session n'est créée, le compte reste bloqué jusqu'à expiration du délai défini
