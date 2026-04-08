# Spec (10) app: Ajouter un cheval

### **Contexte du projet :**

### **Objectifs de la fonctionnalité :**

Permettre à l'utilisateur d'ajouter un cheval dans l'application en renseignant ses informations (photo, nom, âge, race, poids) puis de l'associer à un dispositif physique en scannant le QR code de ce dernier.

### **Acteurs impliqués :**

Utilisateur
Système
Dispositif physique

### **Fonctionnalité et description détaillée :**

Permet à un utilisateur authentifié de créer une fiche cheval en renseignant les informations obligatoires (photo, nom, âge, race) et optionnelles (poids). Une fois le formulaire validé, l'application propose à l'utilisateur de scanner le QR code présent sur le dispositif physique afin de relier le cheval à ce dispositif. Cette association permet au dispositif de transmettre les données de santé au bon profil animal dans l'application.

### **Etapes du flux principal :**

L'utilisateur accède à la section "Mes chevaux" et clique sur "Ajouter un cheval"
Le système affiche le formulaire de création
L'utilisateur ajoute une photo du cheval depuis sa galerie ou sa caméra
L'utilisateur renseigne les informations obligatoires (nom, âge, race) et optionnelles (poids)
L'utilisateur soumet le formulaire
Le système valide les données et enregistre la fiche cheval en base de données
L'application propose à l'utilisateur de scanner le QR code du dispositif physique
L'utilisateur scanne le QR code avec la caméra de son appareil
Le système récupère l'identifiant unique du dispositif et l'associe au cheval
Le système confirme l'association et affiche la fiche complète du cheval

### Scénario alternatifs et exception :

L'utilisateur choisit de passer l'étape du scan → le cheval est enregistré sans être associé à un dispositif, l'association pourra être effectuée ultérieurement depuis la fiche du cheval
Le QR code scanné est invalide ou ne correspond à aucun dispositif connu → un message d'erreur est affiché, l'utilisateur est invité à réessayer
Le QR code scanné correspond à un dispositif déjà associé à un autre cheval → un message d'erreur est affiché, l'association est bloquée
L'utilisateur refuse l'accès à la caméra → le scan est impossible, un message invite l'utilisateur à autoriser l'accès à la caméra dans les paramètres
Un champ obligatoire n'est pas rempli → le formulaire ne peut pas être soumis, les champs manquants sont mis en évidence
La photo dépasse la taille maximale autorisée ou est dans un format non supporté → un message d'erreur est affiché, l'upload est bloqué
Une erreur survient lors de l'enregistrement → un message d'erreur est affiché, aucune donnée n'est enregistrée

### **Règles de gestion :**

RG-01 : L'utilisateur doit être authentifié pour accéder à cette fonctionnalité
RG-02 : Les champs nom, âge et race sont obligatoires
RG-03 : Une photo du cheval est obligatoire
RG-04 : Le poids est facultatif, s'il est renseigné il doit être un nombre positif exprimé en kg
RG-05 : L'âge doit être un nombre entier positif
RG-06 : Les formats acceptés pour la photo sont JPG, JPEG et PNG
RG-07 : La taille maximale de la photo est de 5 Mo
RG-08 : Un dispositif ne peut être associé qu'à un seul cheval à la fois
RG-09 : Un cheval peut exister dans l'application sans être associé à un dispositif
RG-10 : L'identifiant unique du dispositif est extrait du QR code et stocké en base de données pour permettre la liaison avec les données transmises
RG-11 : Un utilisateur ne peut accéder qu'aux fiches des chevaux qu'il a lui-même créées

### **Interface utilisateur :**

Un bouton "Ajouter un cheval" est accessible depuis la liste des chevaux
Le formulaire indique clairement les champs obligatoires (marqués d'un astérisque)
Un sélecteur de photo permet de choisir entre la galerie et la caméra de l'appareil
Une prévisualisation de la photo sélectionnée est affichée dans le formulaire
Les messages d'erreur sont affichés directement sous le champ concerné
Le bouton de soumission est désactivé tant que les champs obligatoires ne sont pas remplis
Après validation du formulaire, une modale propose le scan du QR code avec un bouton "Scanner maintenant" et un bouton "Passer cette étape"
L'interface de scan affiche un cadre de visée pour guider l'utilisateur vers le QR code
Un message de confirmation est affiché une fois l'association dispositif-cheval réussie

### **Cas de test pour la validation :**

CT-01 : Création d'une fiche avec tous les champs obligatoires et scan réussi → cheval enregistré et associé au dispositif
CT-02 : Création d'une fiche avec tous les champs obligatoires sans scanner → cheval enregistré sans dispositif associé
CT-03 : Tentative de soumission sans nom, âge ou race → formulaire non soumis, champs manquants mis en évidence
CT-04 : Tentative de soumission sans photo → formulaire non soumis, message d'erreur affiché
CT-05 : Upload d'une photo dépassant 5 Mo → message d'erreur affiché, upload bloqué
CT-06 : Saisie d'un âge ou d'un poids non numérique ou négatif → message d'erreur affiché sous le champ concerné
CT-07 : Scan d'un QR code invalide → message d'erreur affiché, invitation à réessayer
CT-08 : Scan d'un QR code déjà associé à un autre cheval → message d'erreur affiché, association bloquée
CT-09 : Refus d'accès à la caméra → message informatif affiché, invitation à modifier les paramètres
CT-10 : Vérification qu'un utilisateur ne peut pas accéder aux fiches des chevaux d'un autre utilisateur

### **Post-conditions :**

En cas de création réussie avec association : la fiche cheval est enregistrée en base de données, la photo est stockée, l'identifiant du dispositif est lié au cheval et la fiche complète est affichée
En cas de création réussie sans association : la fiche cheval est enregistrée en base de données sans dispositif lié, l'association pourra être effectuée ultérieurement
En cas d'échec : aucune donnée n'est enregistrée, l'utilisateur reste sur le formulaire avec les messages d'erreur appropriés
