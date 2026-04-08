# Spec (6) app: Carnet de santé agenda

### **Contexte du projet :**

### **Objectifs de la fonctionnalité :**

Proposer un agenda mensuel permettant d'ajouter, consulter et supprimer des rendez-vous vétérinaires, avec la possibilité d'y associer les coordonnées du vétérinaire, des commentaires et des documents (ordonnances, comptes-rendus, etc.).

### **Acteurs impliqués :**

Utilisateur (propriétaire de l'animal)
Système

### **Fonctionnalité et description détaillée :**

Permet à l'utilisateur de visualiser ses rendez-vous sous forme d'agenda mensuel. Il peut créer un rendez-vous en renseignant les informations du vétérinaire (nom, téléphone, adresse, etc.), ajouter un commentaire libre et joindre un ou plusieurs documents (ordonnances, résultats d'analyses, etc.). Les rendez-vous passés restent consultables pour assurer une traçabilité médicale complète.

### **Etapes du flux principal :**

L'utilisateur accède au module agenda depuis le carnet de santé de son animal
Le système affiche la vue mensuelle avec les rendez-vous existants
L'utilisateur sélectionne un jour et clique sur "Ajouter un rendez-vous"
Le système affiche le formulaire de création
L'utilisateur renseigne les informations (date, heure, nom du vétérinaire, téléphone, adresse, commentaire)
L'utilisateur joint un document si nécessaire (ordonnance, compte-rendu, etc.)
L'utilisateur confirme la création du rendez-vous
Le système enregistre le rendez-vous et l'affiche dans l'agenda

### Scénario alternatifs et exception :

L'utilisateur consulte un rendez-vous existant → le système affiche le détail complet (infos vétérinaire, commentaire, documents joints)
L'utilisateur supprime un rendez-vous → une modale de confirmation est affichée, après confirmation le rendez-vous est supprimé définitivement
L'utilisateur annule la création ou la suppression → aucune modification n'est effectuée, retour à l'agenda
Le document joint dépasse la taille maximale autorisée → un message d'erreur est affiché, le document n'est pas uploadé
Le format du document joint n'est pas supporté → un message d'erreur est affiché sous le champ concerné
Aucun rendez-vous n'est présent sur le mois affiché → un message informatif invite l'utilisateur à en créer un

### **Règles de gestion :**

RG-01 : L'utilisateur doit être authentifié pour accéder à l'agenda et être abonné
RG-02 : Les champs date, heure et nom du vétérinaire sont obligatoires pour créer un rendez-vous
RG-03 : Le numéro de téléphone du vétérinaire doit respecter un format valide
RG-04 : Les documents joints doivent être au format PDF, JPG ou PNG
RG-05 : La taille maximale d'un document joint est de 10 Mo
RG-06 : Plusieurs documents peuvent être joints à un même rendez-vous
RG-07 : Les rendez-vous passés sont consultables mais ne peuvent pas être modifiés
RG-08 : La suppression d'un rendez-vous entraîne la suppression de tous les documents qui y sont associés
RG-09 : L'agenda est propre à chaque animal, un utilisateur possédant plusieurs animaux dispose d'un agenda distinct par animal

### **Interface utilisateur :**

La vue par défaut est la vue mensuelle, avec navigation vers le mois précédent et le mois suivant
Les jours ayant un rendez-vous sont mis en évidence visuellement sur le calendrier
Un clic sur un jour avec rendez-vous affiche la liste des rendez-vous du jour
Un clic sur un rendez-vous ouvre le détail complet (infos vétérinaire, commentaire, documents)
Les documents joints sont téléchargeables directement depuis la fiche du rendez-vous
Un bouton "Ajouter un rendez-vous" est accessible depuis la vue calendrier et depuis la fiche détail d'un jour
Une modale de confirmation est affichée avant toute suppression
Les rendez-vous passés sont affichés avec un style visuel distinct (grisé) pour les différencier des rendez-vous à venir

### **Cas de test pour la validation :**

CT-01 : Création d'un rendez-vous avec tous les champs obligatoires remplis → rendez-vous affiché dans l'agenda à la bonne date
CT-02 : Tentative de création sans les champs obligatoires → formulaire non soumis, champs manquants mis en évidence
CT-03 : Ajout d'un document au bon format et sous la limite de taille → document joint et téléchargeable depuis la fiche
CT-04 : Ajout d'un document dépassant 10 Mo → message d'erreur affiché, document non uploadé
CT-05 : Ajout d'un document dans un format non supporté → message d'erreur affiché
CT-06 : Consultation d'un rendez-vous existant → toutes les informations et documents sont affichés correctement
CT-07 : Suppression d'un rendez-vous après confirmation → rendez-vous et documents associés supprimés, agenda mis à jour
CT-08 : Annulation de la suppression depuis la modale → aucune modification effectuée
CT-09 : Navigation entre les mois → les rendez-vous du mois sélectionné sont bien affichés
CT-10 : Vérification que l'agenda d'un animal n'affiche pas les rendez-vous d'un autre animal

### **Post-conditions :**

En cas de création réussie : le rendez-vous est enregistré en base de données, les documents sont stockés et accessibles, le calendrier est mis à jour
En cas de suppression réussie : le rendez-vous et tous ses documents associés sont définitivement supprimés de la base de données
En cas d'échec : aucune donnée n'est modifiée, l'utilisateur reste sur le formulaire avec les messages d'erreur appropriés
