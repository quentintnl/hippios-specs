# Spec (7) app: Stockage des docs + téléchargements

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux.
L’objectif est d’anticiper les problèmes de santé, réduire les coûts vétérinaires et améliorer le bien-être animal. L’application centralise toutes les informations liées à la santé, l’alimentation et le budget, tout en proposant des recommandations personnalisées et des outils connectés pour un suivi en temps réel.
Cette approche s’inscrit dans une volonté de moderniser la gestion quotidienne du cheval grâce à la data et aux objets connectés.

### **Objectifs de la fonctionnalité :**
Permettre à l'utilisateur d'ajouter des documents liés à une consultation médicale afin de conserver une traçabilité complète et de pouvoir les consulter ou les télécharger à tout moment.

### **Acteurs impliqués :**
Utilisateur
Système
Base de données

### **Fonctionnalité et description détaillée :**
Permet à l'utilisateur d'uploader un ou plusieurs documents lors de la création ou de la consultation d'une fiche de consultation médicale. Les documents sont stockés de manière sécurisée sur un service de stockage externe. L'utilisateur peut à tout moment consulter la liste des documents associés à une consultation, les prévisualiser et les télécharger.

### **Etapes du flux principal :**
L'utilisateur accède à une fiche de consultation médicale
L'utilisateur clique sur "Ajouter un document"
Le système affiche un sélecteur de fichier
L'utilisateur sélectionne un ou plusieurs fichiers depuis son appareil
Le système vérifie le format et la taille des fichiers
Le système convertit le fichier et enregistre le document ainsi que ses métadonnées en base de données (nom, type, taille, date d'upload, données du fichier)
Les documents apparaissent dans la liste des pièces jointes de la consultation
L'utilisateur peut cliquer sur un document pour le prévisualiser ou le télécharger

### Scénario alternatifs et exception :
Le fichier dépasse la taille maximale autorisée → un message d'erreur est affiché, le fichier n'est pas enregistré
Le format du fichier n'est pas supporté → un message d'erreur est affiché sous le champ concerné, l'enregistrement est bloqué
Une erreur survient lors de l'enregistrement (erreur base de données, coupure réseau) → un message d'erreur est affiché, l'utilisateur est invité à réessayer, aucune donnée n'est enregistrée
L'utilisateur supprime un document → une modale de confirmation est affichée, après confirmation l'entrée est supprimée de la base de données
L'utilisateur annule l'upload ou la suppression → aucune modification n'est effectuée
Aucun document n'est encore associé à la consultation → un message informatif invite l'utilisateur à en ajouter un

### **Règles de gestion :**
RG-01 : L'utilisateur doit être authentifié pour accéder à cette fonctionnalité
RG-02 : Les formats acceptés sont PDF, JPG, JPEG et PNG uniquement
RG-03 : La taille maximale par fichier est de 5 Mo afin de ne pas surcharger la base de données
RG-04 : Plusieurs documents peuvent être associés à une même consultation
RG-05 : Les documents sont accessibles uniquement par le propriétaire du compte
RG-06 : Le fichier est stocké en base de données sous forme de données binaires (BLOB) ou encodé en base64
RG-07 : Les métadonnées du document (nom original, type MIME, taille, date d'upload) sont enregistrées en base de données alongside le fichier
RG-08 : La suppression d'un document est irréversible et entraîne la suppression de l'entrée complète en base de données
RG-09 : La suppression d'une consultation entraîne la suppression automatique de tous les documents qui y sont associés (cascade)

### **Interface utilisateur :**
Un bouton "Ajouter un document" est accessible depuis la fiche de consultation
La liste des documents associés affiche pour chaque fichier : le nom, le type, la taille et la date d'upload
Une icône de prévisualisation est disponible pour les fichiers JPG, JPEG et PNG
Un bouton de téléchargement est disponible pour chaque document
Un bouton de suppression est disponible pour chaque document avec une modale de confirmation
Une barre de progression est affichée pendant l'enregistrement en base de données
Les messages d'erreur sont affichés clairement sous le champ d'upload en cas de problème

### **Cas de test pour la validation :**
CT-01 : Upload d'un fichier PDF valide et sous la limite de taille → document enregistré en BDD, visible dans la liste avec ses métadonnées
CT-02 : Upload d'un fichier JPG ou PNG valide → document enregistré et prévisualisable depuis la fiche
CT-03 : Upload d'un fichier dépassant 5 Mo → message d'erreur affiché, document non enregistré
CT-04 : Upload d'un fichier dans un format non supporté (ex : .docx, .mp4) → message d'erreur affiché, upload bloqué
CT-05 : Téléchargement d'un document existant → fichier reconstruit depuis la BDD et téléchargé correctement sur l'appareil
CT-06 : Suppression d'un document après confirmation → entrée supprimée de la BDD, document retiré de la liste
CT-07 : Annulation de la suppression depuis la modale → aucune modification effectuée
CT-08 : Simulation d'une erreur base de données pendant l'upload → message d'erreur affiché, aucune donnée enregistrée
CT-09 : Suppression d'une consultation → vérification que tous les documents associés sont bien supprimés en cascade
CT-10 : Vérification qu'un utilisateur ne peut pas accéder aux documents d'un autre utilisateur

### **Post-conditions :**
En cas d'upload réussi : le fichier et ses métadonnées sont enregistrés en base de données et le document apparaît dans la liste de la consultation
En cas de suppression réussie : l'entrée est définitivement supprimée de la base de données et le document disparaît de la liste
En cas d'échec : aucune donnée n'est enregistrée en base de données, l'utilisateur est informé de l'erreur et invité à réessayer
