# Spec (8) app: Veterinaire

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux.
L’objectif est d’anticiper les problèmes de santé, réduire les coûts vétérinaires et améliorer le bien-être animal. L’application centralise toutes les informations liées à la santé, l’alimentation et le budget, tout en proposant des recommandations personnalisées et des outils connectés pour un suivi en temps réel.
Cette approche s’inscrit dans une volonté de moderniser la gestion quotidienne du cheval grâce à la data et aux objets connectés.

### **Objectifs de la fonctionnalité :**

Permettre à l'utilisateur de créer et gérer une fiche vétérinaire contenant les informations essentielles du professionnel (identité, profession, coordonnées) afin de les retrouver facilement et de les associer aux consultations et rendez-vous.

### **Acteurs impliqués :**

Utilisateur
Système

### **Fonctionnalité et description détaillée :**

Permet à l'utilisateur authentifié de créer une fiche vétérinaire en renseignant les informations obligatoires (nom, prénom, profession, au moins un contact) et optionnelles (adresse). La fiche peut être consultée, modifiée ou supprimée à tout moment. Un vétérinaire enregistré peut être associé à un rendez-vous ou une consultation dans les autres modules de l'application.

### **Etapes du flux principal :**

L'utilisateur accède à la section "Vétérinaires" de l'application
Le système affiche la liste des vétérinaires déjà enregistrés
L'utilisateur clique sur "Ajouter un vétérinaire"
Le système affiche le formulaire de création
L'utilisateur renseigne les informations obligatoires (nom, prénom, profession, téléphone et/ou email) et optionnelles (adresse)
L'utilisateur soumet le formulaire
Le système valide les données saisies
Le système enregistre la fiche vétérinaire en base de données
La fiche apparaît dans la liste des vétérinaires

### Scénario alternatifs et exception :

L'utilisateur consulte une fiche vétérinaire existante → le système affiche le détail complet de la fiche
L'utilisateur modifie une fiche vétérinaire → le formulaire est pré-rempli avec les informations actuelles, les modifications sont enregistrées après validation
L'utilisateur supprime une fiche vétérinaire → une modale de confirmation est affichée, après confirmation la fiche est supprimée définitivement
L'utilisateur annule la création, la modification ou la suppression → aucune modification n'est effectuée, retour à la liste
Un champ obligatoire n'est pas rempli → le formulaire ne peut pas être soumis, les champs manquants sont mis en évidence
Ni le téléphone ni l'email ne sont renseignés → le formulaire ne peut pas être soumis, un message indique qu'au moins un contact est obligatoire
Le vétérinaire est associé à des consultations ou rendez-vous existants et est supprimé → les références au vétérinaire dans ces entrées sont conservées sous forme d'archive pour ne pas perdre l'historique
Aucun vétérinaire n'est encore enregistré → un message informatif invite l'utilisateur à en ajouter un

### **Règles de gestion :**

RG-01 : L'utilisateur doit être authentifié pour accéder à cette fonctionnalité
RG-02 : Les champs nom, prénom et profession sont obligatoires
RG-03 : Au moins un moyen de contact doit être renseigné : téléphone ou email (ou les deux)
RG-04 : Le format de l'email doit être valide (ex : [contact@clinique.fr](mailto:contact@clinique.fr))
RG-05 : Le format du numéro de téléphone doit être valide
RG-06 : L'adresse est facultative et peut être renseignée partiellement
RG-07 : Un utilisateur ne peut accéder qu'aux fiches vétérinaires qu'il a lui-même créées
RG-08 : La suppression d'une fiche vétérinaire est irréversible
RG-09 : Un même vétérinaire peut être associé à plusieurs animaux et plusieurs consultations

### **Interface utilisateur :**

La liste des vétérinaires affiche pour chaque fiche : le nom, le prénom, la profession et le contact principal
Un bouton "Ajouter un vétérinaire" est accessible depuis la liste
Le formulaire indique clairement les champs obligatoires (marqués d'un astérisque)
Un message d'erreur est affiché sous chaque champ invalide ou manquant
Un message global indique qu'au moins un contact (téléphone ou email) est requis si aucun n'est renseigné
Les boutons "Modifier" et "Supprimer" sont accessibles depuis la fiche détail du vétérinaire
Une modale de confirmation est affichée avant toute suppression
Un bouton "Annuler" est disponible sur le formulaire pour revenir à la liste sans enregistrer

### **Cas de test pour la validation :**

CT-01 : Création d'une fiche avec tous les champs obligatoires remplis et un email valide → fiche enregistrée et visible dans la liste
CT-02 : Création d'une fiche avec tous les champs obligatoires remplis et un téléphone valide → fiche enregistrée et visible dans la liste
CT-03 : Création d'une fiche sans nom ou prénom → formulaire non soumis, champs manquants mis en évidence
CT-04 : Création d'une fiche sans aucun contact (ni téléphone ni email) → formulaire non soumis, message d'erreur affiché
CT-05 : Création d'une fiche avec un email au format invalide → message d'erreur affiché sous le champ email
CT-06 : Création d'une fiche avec un numéro de téléphone au format invalide → message d'erreur affiché sous le champ téléphone
CT-07 : Modification d'une fiche existante avec des données valides → modifications enregistrées, fiche mise à jour dans la liste
CT-08 : Suppression d'une fiche après confirmation → fiche supprimée, retrait de la liste
CT-09 : Annulation de la suppression depuis la modale → aucune modification effectuée
CT-10 : Vérification qu'un utilisateur ne peut pas accéder aux fiches vétérinaires d'un autre utilisateur

### **Post-conditions :**

En cas de création réussie : la fiche vétérinaire est enregistrée en base de données et apparaît dans la liste de l'utilisateur
En cas de modification réussie : les nouvelles informations sont enregistrées en base de données et la fiche affichée est mise à jour
En cas de suppression réussie : la fiche est définitivement supprimée de la base de données et retirée de la liste
En cas d'échec : aucune donnée n'est modifiée, l'utilisateur reste sur le formulaire avec les messages d'erreur appropriés
