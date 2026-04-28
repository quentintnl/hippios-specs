# Spec (12) app: Supprimer un cheval

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux.
L’objectif est d’anticiper les problèmes de santé, réduire les coûts vétérinaires et améliorer le bien-être animal. L’application centralise toutes les informations liées à la santé, l’alimentation et le budget, tout en proposant des recommandations personnalisées et des outils connectés pour un suivi en temps réel.
Cette approche s’inscrit dans une volonté de moderniser la gestion quotidienne du cheval grâce à la data et aux objets connectés.

### **Objectifs de la fonctionnalité :**

Permettre à l'utilisateur de supprimer définitivement un cheval et toutes ses données associées (consultations, rendez-vous, documents, données du dispositif) après confirmation sécurisée par mot de passe.

### **Acteurs impliqués :**

- Utilisateur
- Système

### **Fonctionnalité et description détaillée :**

Permet à un utilisateur authentifié de supprimer la fiche d'un cheval lui appartenant. L'action est irréversible et entraîne la suppression en cascade de toutes les données liées au cheval (photo, consultations, rendez-vous, documents médicaux, fiche vétérinaire associée, données transmises par le dispositif). Afin de prévenir toute suppression accidentelle, l'utilisateur doit saisir son mot de passe pour confirmer l'action. Le dispositif physique éventuellement associé est dissocié mais n'est pas supprimé.

### **Etapes du flux principal :**

L'utilisateur accède à la fiche du cheval depuis la liste des chevaux
L'utilisateur clique sur "Supprimer ce cheval"
Le système affiche une modale d'avertissement indiquant que l'action est irréversible et listera les données qui seront supprimées
L'utilisateur saisit son mot de passe dans le champ de confirmation
L'utilisateur confirme la suppression
Le système vérifie le mot de passe
Le système supprime en cascade toutes les données associées au cheval
Le dispositif éventuellement associé est dissocié
La session reste active et l'utilisateur est redirigé vers la liste des chevaux
Un message de confirmation de suppression est affiché

### Scénario alternatifs et exception :

Le mot de passe saisi est incorrect → un message d'erreur est affiché, la suppression est bloquée, l'utilisateur peut réessayer
L'utilisateur annule depuis la modale de confirmation → aucune action effectuée, retour à la fiche du cheval
La session a expiré avant la confirmation → l'utilisateur est redirigé vers la page de connexion, aucune suppression effectuée
Une erreur survient lors de la suppression en base de données → un message d'erreur est affiché, aucune donnée n'est supprimée, l'intégrité des données est préservée
Le cheval est le seul enregistré sur le compte → la suppression est autorisée, la liste des chevaux affiche un message invitant l'utilisateur à en ajouter un nouveau

### **Règles de gestion :**

RG-01 : L'utilisateur doit être authentifié pour accéder à cette fonctionnalité
RG-02 : L'utilisateur ne peut supprimer que les fiches des chevaux qu'il a lui-même créées
RG-03 : La saisie du mot de passe est obligatoire pour confirmer la suppression
RG-04 : La suppression est irréversible, aucune restauration des données n'est possible après confirmation
RG-05 : La suppression du cheval entraîne la suppression en cascade de toutes ses données associées : photo, consultations, rendez-vous, documents médicaux et données du dispositif
RG-06 : Le dispositif physique éventuellement associé est dissocié mais n'est pas supprimé, il peut être réassocié à un autre cheval
RG-07 : La suppression est une opération atomique : soit toutes les données sont supprimées, soit aucune ne l'est en cas d'erreur
RG-08 : La session de l'utilisateur reste active après la suppression

### **Interface utilisateur :**

Le bouton "Supprimer ce cheval" est clairement distinct visuellement (couleur rouge) pour signaler une action destructrice
Une modale de confirmation affiche un message d'avertissement explicite sur le caractère irréversible de l'action
La modale liste explicitement les données qui seront supprimées (consultations, rendez-vous, documents, etc.)
Un champ de saisie du mot de passe est présent dans la modale pour confirmer l'identité
Le bouton de confirmation dans la modale est désactivé tant que le champ mot de passe est vide
Un bouton "Annuler" est disponible dans la modale pour abandonner l'action
Un indicateur de chargement est affiché pendant le traitement de la suppression
Un message de confirmation est affiché sur la liste des chevaux après suppression réussie

### **UX/UI**
![Texte alternatif](./wireframe/app/suppression_chevla_1.png "Titre de l'image")
![Texte alternatif](./wireframe/app/supression_cheval.png "Titre de l'image")

### **Cas de test pour la validation :**

CT-01 : Suppression réussie avec mot de passe correct → cheval et toutes ses données supprimés, redirection vers la liste des chevaux
CT-02 : Tentative de suppression avec mot de passe incorrect → message d'erreur affiché, suppression bloquée
CT-03 : Annulation depuis la modale → aucune donnée supprimée, retour à la fiche du cheval
CT-04 : Vérification post-suppression que toutes les données associées sont bien supprimées en cascade (consultations, rendez-vous, documents)
CT-05 : Vérification que le dispositif associé est bien dissocié mais toujours disponible pour être réassocié
CT-06 : Tentative d'accès à la fonctionnalité sans être connecté → redirection vers la page de connexion
CT-07 : Simulation d'une erreur base de données pendant la suppression → message d'erreur affiché, aucune donnée supprimée
CT-08 : Suppression du dernier cheval du compte → liste vide affichée avec message d'invitation à en ajouter un nouveau
CT-09 : Vérification qu'un utilisateur ne peut pas supprimer la fiche d'un cheval appartenant à un autre utilisateur

### **Post-conditions :**

En cas de succès : le cheval et l'intégralité de ses données associées sont définitivement supprimés de la base de données, le dispositif éventuellement lié est dissocié et l'utilisateur est redirigé vers la liste des chevaux avec un message de confirmation
En cas d'échec : aucune donnée n'est supprimée grâce à l'opération atomique, l'utilisateur reste sur la modale avec le message d'erreur approprié
