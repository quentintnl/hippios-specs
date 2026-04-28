# Spec : Créer un rendez-vous

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux. (Voir le README principal pour plus de détails).

### **Acteurs impliqués :**
- Utilisateur (propriétaire de l'animal)
- Système

### **Fonctionnalité et description détaillée :**
Permet à l'utilisateur authentifié et abonné de créer un rendez-vous vétérinaire depuis la vue mensuelle de l'agenda. Il renseigne les informations du vétérinaire, un commentaire libre et peut joindre un ou plusieurs documents. Une fois confirmé, le rendez-vous est enregistré et visible dans l'agenda.

### **Etapes du flux principal :**
1. L'utilisateur accède au module agenda depuis le carnet de santé de son animal
2. Le système affiche la vue mensuelle
3. L'utilisateur sélectionne un jour et clique sur "Ajouter un rendez-vous"
4. Le système affiche le formulaire de création
5. L'utilisateur renseigne les informations obligatoires (date, heure, nom du vétérinaire) et optionnelles (téléphone, adresse, commentaire)
6. L'utilisateur joint un ou plusieurs documents si nécessaire (ordonnance, compte-rendu, etc.)
7. L'utilisateur confirme la création
8. Le système valide les données et enregistre le rendez-vous en base de données
9. Le rendez-vous apparaît dans l'agenda à la bonne date

### **Scénarios alternatifs et exceptions :**
- Un champ obligatoire n'est pas rempli → le formulaire ne peut pas être soumis, les champs manquants sont mis en évidence
- Le document joint dépasse la taille maximale autorisée → un message d'erreur est affiché, le document n'est pas enregistré
- Le format du document joint n'est pas supporté → un message d'erreur est affiché sous le champ concerné
- L'utilisateur annule la création → aucune donnée n'est enregistrée, retour à la vue mensuelle
- Le numéro de téléphone saisi est dans un format invalide → un message d'erreur est affiché sous le champ concerné

### **Règles de gestion :**
- RG-01 : L'utilisateur doit être authentifié et abonné pour accéder à cette fonctionnalité
- RG-02 : Les champs date, heure et nom du vétérinaire sont obligatoires
- RG-03 : Le numéro de téléphone du vétérinaire doit respecter un format valide s'il est renseigné
- RG-04 : Les documents joints doivent être au format PDF, JPG ou PNG
- RG-05 : La taille maximale par document joint est de 10 Mo
- RG-06 : Plusieurs documents peuvent être joints à un même rendez-vous
- RG-07 : Le rendez-vous est rattaché à l'animal depuis lequel l'agenda a été ouvert

### **Interface utilisateur :**
- Un bouton "Ajouter un rendez-vous" est accessible depuis la vue calendrier et depuis la fiche détail d'un jour
- Le formulaire indique clairement les champs obligatoires (marqués d'un astérisque)
- Les messages d'erreur sont affichés directement sous le champ concerné
- Un bouton "Annuler" permet de revenir à l'agenda sans enregistrer
- Le bouton de soumission est désactivé tant que les champs obligatoires ne sont pas remplis

### **Cas de test pour la validation :**
- CT-01 : Création avec tous les champs obligatoires remplis → rendez-vous enregistré et affiché dans l'agenda à la bonne date
- CT-02 : Tentative de création sans les champs obligatoires → formulaire non soumis, champs manquants mis en évidence
- CT-03 : Ajout d'un document au bon format et sous la limite de taille → document joint et accessible depuis la fiche
- CT-04 : Ajout d'un document dépassant 10 Mo → message d'erreur affiché, document non enregistré
- CT-05 : Ajout d'un document dans un format non supporté → message d'erreur affiché
- CT-06 : Annulation de la création → aucune donnée enregistrée, retour à l'agenda

### **Post-conditions :**
- En cas de succès : le rendez-vous est enregistré en base de données, les documents sont stockés, le calendrier est mis à jour et le jour concerné est mis en évidence
- En cas d'échec : aucune donnée n'est enregistrée, l'utilisateur reste sur le formulaire avec les messages d'erreur appropriés
