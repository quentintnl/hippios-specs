# Spec (15) app: Historique des métrics

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux.
L’objectif est d’anticiper les problèmes de santé, réduire les coûts vétérinaires et améliorer le bien-être animal. L’application centralise toutes les informations liées à la santé, l’alimentation et le budget, tout en proposant des recommandations personnalisées et des outils connectés pour un suivi en temps réel.
Cette approche s’inscrit dans une volonté de moderniser la gestion quotidienne du cheval grâce à la data et aux objets connectés.

### **Objectifs de la fonctionnalité :**
Permettre à l'utilisateur de consulter les métriques de santé de son cheval sur différentes périodes (1 jour, 7 jours, 15 jours, 1 mois) via un graphique interactif affichant deux courbes : la moyenne générale de tous les chevaux de la plateforme et les données propres au cheval de l'utilisateur.

### **Acteurs impliqués :**
- Utilisateur (propriétaire du cheval)
- Système
- Dispositif physique (capteur)

### **Fonctionnalité et description détaillée :**
Permet à l'utilisateur de visualiser l'évolution des métriques de santé de son cheval (fréquence cardiaque, température, etc.) sous forme de graphique en courbes sur une période sélectionnable. Deux courbes sont affichées simultanément : l'une représentant les données du cheval de l'utilisateur, l'autre représentant la moyenne de tous les chevaux enregistrés sur la plateforme, servant de référence comparative. L'utilisateur peut naviguer dans le temps pour consulter des périodes passées et changer de métrique à visualiser.

### **Etapes du flux principal :**
L'utilisateur accède à la page des métriques depuis le dashboard du cheval
Le système affiche par défaut le graphique sur la période 1 jour pour la métrique principale
Le système récupère les données du cheval de l'utilisateur pour la période sélectionnée
Le système récupère la moyenne de tous les chevaux de la plateforme pour la même période
Le système affiche les deux courbes sur le graphique
L'utilisateur peut changer la période d'affichage (1j, 7j, 15j, 1 mois)
L'utilisateur peut naviguer vers les périodes précédentes ou suivantes via les flèches de navigation
L'utilisateur peut changer la métrique visualisée
Le système met à jour le graphique en fonction des sélections

### Scénario alternatifs et exception :
Aucune donnée n'est disponible pour la période sélectionnée → un message informatif est affiché à la place du graphique, invitant l'utilisateur à sélectionner une autre période
Le capteur est éteint ou injoignable → les données du cheval s'arrêtent à la dernière valeur connue, un indicateur d'inactivité est affiché
Aucun capteur n'est associé au cheval → un message invite l'utilisateur à associer un dispositif, le graphique de la moyenne générale reste affiché seul
La période "suivante" correspond à une date future → la navigation vers le futur est bloquée, la période courante est la plus récente accessible
La récupération des données échoue (erreur réseau) → un message d'erreur est affiché, l'utilisateur est invité à rafraîchir
Aucune donnée de moyenne générale n'est disponible (plateforme vide) → seule la courbe du cheval de l'utilisateur est affichée

### **Règles de gestion :**
RG-01 : L'utilisateur doit être authentifié pour accéder à cette fonctionnalité
RG-02 : L'utilisateur ne peut consulter que les métriques des chevaux lui appartenant
RG-03 : Les périodes de visualisation disponibles sont : 1 jour, 7 jours, 15 jours et 1 mois
RG-04 : La période affichée par défaut au chargement est 1 jour
RG-05 : La navigation dans le temps est limitée à la date du jour comme borne maximale, la navigation vers le futur est impossible
RG-06 : La courbe de moyenne générale est calculée à partir des données anonymisées de l'ensemble des chevaux de la plateforme
RG-07 : La granularité des données affichées s'adapte à la période sélectionnée : à la minute pour 1j, à l'heure pour 7j et 15j, au jour pour 1 mois
RG-08 : Les deux courbes partagent le même axe temporel et le même axe de valeurs pour permettre une comparaison directe
RG-09 : Les données affichées sur le graphique sont en lecture seule, aucune modification n'est possible depuis cette vue
RG-10 : Les zones hors seuil de normalité sont mises en évidence sur le graphique (fond coloré ou zone ombrée)

### **Interface utilisateur :**
Un sélecteur de période est affiché en haut du graphique avec les options : 1j, 7j, 15j, 1 mois
Des flèches de navigation gauche/droite permettent de se déplacer dans le temps selon la période sélectionnée
La période actuellement affichée (dates de début et de fin) est indiquée clairement au-dessus du graphique
La flèche de navigation vers le futur est désactivée lorsque la période courante est atteinte
Deux courbes sont affichées avec des couleurs distinctes et une légende claire : "Mon cheval" et "Moyenne générale"
Un sélecteur de métrique permet de basculer entre les différentes métriques disponibles (fréquence cardiaque, température, etc.)
Au survol d'un point du graphique, une infobulle affiche la valeur exacte, la date et l'heure correspondantes
Les zones hors seuil de normalité sont signalées par un fond coloré sur le graphique
Un indicateur de dernière mise à jour est affiché sous le graphique
Un bouton de rafraîchissement manuel est disponible pour forcer la mise à jour des données

### **Cas de test pour la validation :**
CT-01 : Chargement de la page → graphique affiché sur 1 jour avec les deux courbes visibles
CT-02 : Sélection de la période 7j → graphique mis à jour avec les données sur 7 jours et granularité à l'heure
CT-03 : Sélection de la période 15j → graphique mis à jour avec les données sur 15 jours
CT-04 : Sélection de la période 1 mois → graphique mis à jour avec les données sur 1 mois et granularité au jour
CT-05 : Navigation vers la période précédente → graphique mis à jour avec les données de la période précédente
CT-06 : Tentative de navigation vers une période future → flèche désactivée, navigation bloquée
CT-07 : Aucune donnée disponible sur la période sélectionnée → message informatif affiché à la place du graphique
CT-08 : Capteur éteint → courbe du cheval s'arrête à la dernière valeur connue, indicateur d'inactivité affiché
CT-09 : Aucun capteur associé → message d'invitation affiché, seule la courbe de moyenne générale est visible
CT-10 : Survol d'un point de courbe → infobulle affichant la valeur exacte, la date et l'heure
CT-11 : Changement de métrique → graphique mis à jour avec les données de la nouvelle métrique sélectionnée
CT-12 : Erreur réseau lors de la récupération des données → message d'erreur affiché, invitation à rafraîchir
CT-13 : Vérification que la courbe de moyenne est bien anonymisée et ne permet pas d'identifier un cheval en particulier

### **Post-conditions :**
En cas de chargement réussi : le graphique affiche les deux courbes sur la période sélectionnée avec les données à jour
En cas de changement de période ou de navigation : le graphique est mis à jour instantanément avec les données correspondantes
En cas d'erreur de chargement : un message d'erreur est affiché et les dernières données affichées sont conservées à l'écran
