# Spec (2) site: Modifer user

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux.
L’objectif est d’anticiper les problèmes de santé, réduire les coûts vétérinaires et améliorer le bien-être animal. L’application centralise toutes les informations liées à la santé, l’alimentation et le budget, tout en proposant des recommandations personnalisées et des outils connectés pour un suivi en temps réel.
Cette approche s’inscrit dans une volonté de moderniser la gestion quotidienne du cheval grâce à la data et aux objets connectés.

### **Objectifs de la fonctionnalité :**

Permettre l’authentification des utilisateurs

### **Acteurs impliqués :**

Utilisateurs

Le système

### **Fonctionnalité et description détaillée :**

Permet a l’utilisateur de se connecter au site vitrine

### **Etapes du flux principal :**

L’utilisateur rentre ses informations dans le formulaire de connexion.
Le système vérifie les informations

Le système confirme la connexion

L’utilisateur est connecter sur le site

### Scénario alternatifs et exception :

Les informations du formulaire de connexion ne sont pas correctes. L’utilisateur ne peux pas se connecter

### **Règles de gestion :**

RG-01 : Les champs du formulaire doivent être rempli pour l’envoyer

RG-02 : Le mot de passe doit respecter un format sécurisé (minimum 8 caractères, au moins une majuscule, un chiffre et un caractère spécial)

**RG-03 : Après 5 tentatives de connexion échouées consécutives, le compte est temporairement bloqué pendant 15 minutes**

**RG-04 : La session utilisateur expire après 30 minutes d'inactivité**

**RG-05 : L'adresse email doit respecter un format valide (ex: [user@domaine.com](mailto:user@domaine.com))**

### **Interface utilisateur :**

Le bouton est désactivé si les champs sont vides

**Un message d'erreur générique est affiché en cas d'identifiants incorrects (ex : "Email ou mot de passe incorrect") sans préciser lequel des deux est erroné**

**Un indicateur de chargement est affiché pendant la vérification des identifiants**

**Un lien "Mot de passe oublié" est disponible sous le formulaire**

### **Cas de test pour la validation :**

- **CT-01 : Connexion réussie avec des identifiants valides → l'utilisateur est redirigé vers son espace**
- **CT-02 : Tentative de connexion avec un email invalide → message d'erreur affiché, connexion refusée**
- **CT-03 : Tentative de connexion avec un mot de passe incorrect → message d'erreur générique affiché**
- **CT-04 : Soumission du formulaire avec un ou plusieurs champs vides → bouton désactivé, envoi impossible**
- **CT-05 : 5 tentatives échouées consécutives → compte bloqué temporairement, message informatif affiché**
- **CT-06 : Inactivité de 30 minutes → session expirée, redirection vers la page de connexion**

### **Post-conditions :**

**En cas de succès : l'utilisateur est authentifié, une session est créée et il est redirigé vers la page d'accueil de son espace personnel**

**En cas d'échec : aucune session n'est créée, l'utilisateur reste sur la page de connexion avec un message d'erreur**
