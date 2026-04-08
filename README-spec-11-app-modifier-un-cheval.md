# Spec (11) app: Modifier un cheval

### **Contexte du projet :**

### **Objectifs de la fonctionnalité :**

Permettre à l'utilisateur de modifier les informations de son cheval (nom, image, poids, race) et de le relier à un nouveau dispositif physique en scannant un nouveau QR code.

### **Acteurs impliqués :**

- Utilisateur
- Système
- Dispositif physique

### **Fonctionnalité et description détaillée :**

Permet à un utilisateur authentifié d'accéder à la fiche d'un cheval existant et de modifier ses informations. Le formulaire est pré-rempli avec les données actuelles. L'utilisateur peut également remplacer le dispositif physique associé au cheval en scannant le QR code d'un nouveau dispositif. L'ancien lien avec le dispositif précédent est alors supprimé et remplacé par le nouveau.

### **Etapes du flux principal :**

L'utilisateur accède à la fiche du cheval depuis la liste des chevaux
L'utilisateur clique sur "Modifier"
Le système affiche le formulaire pré-rempli avec les informations actuelles du cheval
L'utilisateur modifie les champs souhaités (nom, image, poids, race)
L'utilisateur soumet le formulaire
Le système valide les données saisies
Le système enregistre les modifications en base de données
Le système affiche un message de confirmation et redirige vers la fiche mise à jour

### Scénario alternatifs et exception :

L'utilisateur souhaite associer un nouveau dispositif → il clique sur "Changer de dispositif", l'application ouvre l'interface de scan de QR code
Le QR code scanné est invalide ou ne correspond à aucun dispositif connu → un message d'erreur est affiché, l'utilisateur est invité à réessayer, l'ancien dispositif reste associé
Le QR code scanné correspond à un dispositif déjà associé à un autre cheval → un message d'erreur est affiché, l'association est bloquée, l'ancien dispositif reste associé
L'utilisateur refuse l'accès à la caméra → le scan est impossible, un message invite l'utilisateur à autoriser l'accès à la caméra dans les paramètres
L'utilisateur souhaite dissocier le dispositif actuel sans en associer un nouveau → il clique sur "Dissocier le dispositif", une modale de confirmation est affichée, après confirmation le lien est supprimé
Un champ obligatoire est vidé lors de la modification → le formulaire ne peut pas être soumis, les champs manquants sont mis en évidence
La nouvelle photo dépasse la taille maximale ou est dans un format non supporté → un message d'erreur est affiché, l'ancienne photo est conservée
L'utilisateur annule la modification → aucune donnée n'est modifiée, retour à la fiche du cheval

### **Règles de gestion :**

RG-01 : L'utilisateur doit être authentifié pour accéder à cette fonctionnalité
RG-02 : L'utilisateur ne peut modifier que les fiches des chevaux qu'il a lui-même créées
RG-03 : Les champs nom et race restent obligatoires lors de la modification
RG-04 : Le poids est facultatif, s'il est renseigné il doit être un nombre positif exprimé en kg
RG-05 : Les formats acceptés pour la photo sont JPG, JPEG et PNG
RG-06 : La taille maximale de la photo est de 5 Mo
RG-07 : Si une nouvelle photo est uploadée, elle remplace l'ancienne en base de données
RG-08 : Un dispositif ne peut être associé qu'à un seul cheval à la fois
RG-09 : Lors de l'association d'un nouveau dispositif, le lien avec l'ancien dispositif est automatiquement supprimé
RG-10 : La dissociation d'un dispositif est irréversible, le cheval peut exister sans dispositif associé
RG-11 : L'âge n'est pas modifiable depuis cette interface car il est calculé automatiquement à partir de la date de naissance

### **Interface utilisateur :**

Le formulaire est pré-rempli avec les informations actuelles du cheval
La photo actuelle est affichée avec un bouton permettant de la remplacer
Les champs obligatoires sont clairement indiqués (marqués d'un astérisque)
Les messages d'erreur sont affichés directement sous le champ concerné
Une section "Dispositif associé" affiche l'identifiant du dispositif actuel avec deux options : "Changer de dispositif" et "Dissocier le dispositif"
Si aucun dispositif n'est associé, un bouton "Associer un dispositif" est affiché à la place
L'interface de scan affiche un cadre de visée pour guider l'utilisateur vers le QR code
Un bouton "Annuler" permet de revenir à la fiche sans enregistrer les modifications
Un message de confirmation est affiché après l'enregistrement des modifications
Un message de confirmation est affiché après une association ou dissociation de dispositif réussie

### **Cas de test pour la validation :**

CT-01 : Modification réussie du nom avec une valeur valide → modification enregistrée, fiche mise à jour
CT-02 : Modification réussie de la race avec une valeur valide → modification enregistrée, fiche mise à jour
CT-03 : Modification réussie du poids avec une valeur numérique positive → modification enregistrée, fiche mise à jour
CT-04 : Remplacement de la photo par un fichier valide → nouvelle photo enregistrée, ancienne supprimée
CT-05 : Tentative de remplacement de la photo par un fichier dépassant 5 Mo → message d'erreur affiché, ancienne photo conservée
CT-06 : Tentative de soumission avec le champ nom ou race vide → formulaire non soumis, champs manquants mis en évidence
CT-07 : Association d'un nouveau dispositif via scan réussi → ancien lien supprimé, nouveau dispositif associé
CT-08 : Scan d'un QR code invalide → message d'erreur affiché, ancien dispositif conservé
CT-09 : Scan d'un QR code déjà associé à un autre cheval → message d'erreur affiché, association bloquée
CT-10 : Dissociation du dispositif après confirmation → lien supprimé, section dispositif mise à jour
CT-11 : Annulation de la modification → aucune donnée modifiée, retour à la fiche du cheval
CT-12 : Vérification qu'un utilisateur ne peut pas modifier la fiche d'un cheval appartenant à un autre utilisateur

### **Post-conditions :**

En cas de modification réussie : les nouvelles informations sont enregistrées en base de données et la fiche affichée est mise à jour
En cas de changement de dispositif réussi : l'ancien lien est supprimé, le nouveau dispositif est associé au cheval en base de données
En cas de dissociation réussie : le lien entre le cheval et le dispositif est supprimé en base de données, le cheval reste accessible sans dispositif associé
En cas d'échec : aucune donnée n'est modifiée, l'utilisateur reste sur le formulaire avec les messages d'erreur appropriés
