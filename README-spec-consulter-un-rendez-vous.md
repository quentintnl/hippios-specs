# Spec : Consulter un rendez-vous

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux. (Voir le README principal pour plus de détails).

### **Objectifs de la fonctionnalité :**
Permettre à l'utilisateur de consulter l'ensemble des informations d'un rendez-vous vétérinaire depuis la vue mensuelle de l'agenda, y compris les documents associés.

### **Acteurs impliqués :**
- Utilisateur (propriétaire de l'animal)
- Système

### **Fonctionnalité et description détaillée :**
Permet à l'utilisateur authentifié et abonné de naviguer dans l'agenda mensuel et de consulter le détail complet d'un rendez-vous existant. Les rendez-vous passés restent consultables pour assurer une traçabilité médicale complète. Les documents joints sont téléchargeables directement depuis la fiche.

### **Etapes du flux principal :**
1. L'utilisateur accède au module agenda depuis le carnet de santé de son animal
2. Le système affiche la vue mensuelle avec les jours ayant un rendez-vous mis en évidence
3. L'utilisateur clique sur un jour ayant un rendez-vous
4. Le système affiche la liste des rendez-vous du jour sélectionné
5. L'utilisateur clique sur un rendez-vous
6. Le système affiche la fiche détail complète (date, heure, infos vétérinaire, commentaire, documents joints)
7. L'utilisateur consulte les informations et peut télécharger les documents joints

### **Scénarios alternatifs et exceptions :**
- Aucun rendez-vous n'est présent sur le mois affiché → un message informatif invite l'utilisateur à en créer un
- L'utilisateur navigue vers un mois précédent ou suivant → le système affiche les rendez-vous du mois sélectionné
- La récupération des données échoue → un message d'erreur est affiché, l'utilisateur est invité à rafraîchir

### **Règles de gestion :**
- RG-01 : L'utilisateur doit être authentifié et abonné pour accéder à cette fonctionnalité
- RG-02 : L'utilisateur ne peut consulter que les rendez-vous rattachés à ses propres animaux
- RG-03 : Les rendez-vous passés sont consultables mais ne peuvent pas être modifiés
- RG-04 : L'agenda est propre à chaque animal, un utilisateur possédant plusieurs animaux dispose d'un agenda distinct par animal
- RG-05 : Les documents joints sont téléchargeables directement depuis la fiche du rendez-vous

### **Interface utilisateur :**
- La vue par défaut est la vue mensuelle avec navigation vers le mois précédent et le mois suivant
- Les jours ayant un rendez-vous sont mis en évidence visuellement sur le calendrier
- Un clic sur un jour avec rendez-vous affiche la liste des rendez-vous du jour
- Un clic sur un rendez-vous ouvre la fiche détail complète
- Les rendez-vous passés sont affichés avec un style visuel distinct (grisé) pour les différencier des rendez-vous à venir
- Un bouton de téléchargement est disponible pour chaque document joint

### **Cas de test pour la validation :**
- CT-01 : Consultation d'un rendez-vous existant → toutes les informations et documents sont affichés correctement
- CT-02 : Navigation entre les mois → les rendez-vous du mois sélectionné sont bien affichés
- CT-03 : Consultation d'un rendez-vous passé → fiche affichée correctement en mode lecture seule
- CT-04 : Téléchargement d'un document joint → fichier téléchargé correctement
- CT-05 : Aucun rendez-vous sur le mois → message informatif affiché
- CT-06 : Vérification que l'agenda d'un animal n'affiche pas les rendez-vous d'un autre animal

### **Post-conditions :**
- En cas de succès : la fiche détail du rendez-vous est affichée avec l'ensemble des informations et documents accessibles
- En cas d'erreur de chargement : un message d'erreur est affiché et l'utilisateur est invité à réessayer
