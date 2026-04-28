# Spec : Stockage des docs

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux. (Voir le README principal pour plus de détails).

### **Objectifs de la fonctionnalité :**
Cette fonctionnalité permet à l'utilisateur d'uploader un ou plusieurs documents lors de la création ou de la consultation d'une fiche médicale. Les fichiers sont convertis, validés, puis stockés de manière sécurisée en base de données avec leurs métadonnées associées.

### **Acteurs impliqués :**
- Utilisateur
- Système
- Base de données

### **Fonctionnalité et description détaillée :**
Permet à l'utilisateur d'uploader un ou plusieurs documents lors de la création ou de la consultation d'une fiche de consultation médicale. Les documents sont stockés de manière sécurisée sur un service de stockage externe.

### **Etapes du flux principal :**
1. L'utilisateur accède à une fiche de consultation médicale
2. L'utilisateur clique sur "Ajouter un document"
3. Le système affiche un sélecteur de fichier
4. L'utilisateur sélectionne un ou plusieurs fichiers depuis son appareil
5. Le système vérifie le format et la taille des fichiers (RG-02, RG-03)
6. Le système convertit le fichier et enregistre les données binaires + métadonnées en BDD
7. Le document apparaît dans la liste des pièces jointes avec ses informations

### **Scénario alternatifs et exception :**
- Le fichier dépasse la taille maximale autorisée → un message d'erreur est affiché, le fichier n'est pas enregistré
- Le format du fichier n'est pas supporté → un message d'erreur est affiché sous le champ concerné, l'enregistrement est bloqué
- Une erreur survient lors de l'enregistrement (erreur base de données, coupure réseau) → un message d'erreur est affiché, l'utilisateur est invité à réessayer, aucune donnée n'est enregistrée
- L'utilisateur supprime un document → une modale de confirmation est affichée, après confirmation l'entrée est supprimée de la base de données
- L'utilisateur annule l'upload ou la suppression → aucune modification n'est effectuée
- Aucun document n'est encore associé à la consultation → un message informatif invite l'utilisateur à en ajouter un

### **Règles de gestion :**
- RG-01: Authentification. L'utilisateur doit être authentifié pour accéder à cette fonctionnalité
- RG-02: Formats acceptés. PDF, JPG, JPEG et PNG uniquement
- RG-03: Taille maximale. 5 Mo par fichier afin de ne pas surcharger la base de données
- RG-04: Multi-documents. Plusieurs documents peuvent être associés à une même consultation
- RG-05: Accès restreint. Les documents sont accessibles uniquement par le propriétaire du compte
- RG-06: Format de stockage. Le fichier est stocké en BDD sous forme de données binaires (BLOB) ou encodé en base64
- RG-07: Métadonnées. Nom original, type MIME, taille et date d'upload enregistrés alongside le fichier
- RG-08: Suppression irréversible. La suppression d'un document entraîne la suppression définitive de l'entrée en BDD
- RG-09: Cascade. La suppression d'une consultation entraîne la suppression automatique de tous ses documents associés

### **Interface utilisateur :**
- Bouton "Ajouter un document" accessible depuis la fiche de consultation
- Liste des documents affichant : nom, type, taille et date d'upload
- Barre de progression affichée pendant l'enregistrement en base de données
- Messages d'erreur affichés clairement sous le champ d'upload en cas de problème
- Bouton de suppression avec modale de confirmation pour chaque document

### **Cas de test pour la validation :**
- CT-01 : Upload d'un fichier PDF valide et sous la limite de taille → Document enregistré en BDD, visible dans la liste avec ses métadonnées
- CT-03 : Upload d'un fichier dépassant 5 Mo → Message d'erreur affiché, document non enregistré
- CT-04 : Upload d'un fichier dans un format non supporté (.docx, .mp4) → Message d'erreur affiché, upload bloqué
- CT-06 : Suppression d'un document après confirmation → Entrée supprimée de la BDD, document retiré de la liste
- CT-07 : Annulation de la suppression depuis la modale → Aucune modification effectuée
- CT-08 : Simulation d'une erreur BDD pendant l'upload → Message d'erreur affiché, aucune donnée enregistrée
- CT-09 : Suppression d'une consultation → Tous les documents associés sont supprimés en cascade
- CT-10 : Accès aux documents d'un autre utilisateur → Accès refusé - documents non accessibles

### **Post-conditions :**
- Upload réussi : Le fichier et ses métadonnées sont enregistrés en base de données. Le document apparaît dans la liste de la consultation
- Suppression réussie : L'entrée est définitivement supprimée de la base de données. Le document disparaît de la liste
- Échec : Aucune donnée n'est enregistrée en base de données. L'utilisateur est informé de l'erreur et invité à réessayer
