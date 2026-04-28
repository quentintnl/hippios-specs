# Spec (5) site: Payement

### **Contexte du projet :**
Notre projet vise à développer une application de suivi équestre permettant aux propriétaires et aux professionnels d’assurer un suivi complet et continu de la santé de leurs chevaux.
L’objectif est d’anticiper les problèmes de santé, réduire les coûts vétérinaires et améliorer le bien-être animal. L’application centralise toutes les informations liées à la santé, l’alimentation et le budget, tout en proposant des recommandations personnalisées et des outils connectés pour un suivi en temps réel.
Cette approche s’inscrit dans une volonté de moderniser la gestion quotidienne du cheval grâce à la data et aux objets connectés.

### **Objectifs de la fonctionnalité :**

Payer son abonnement

### **Acteurs impliqués :**

Utilisateur

Stripe

Système

### **Fonctionnalité et description détaillée :**

Permet à un utilisateur authentifié de souscrire à un abonnement en saisissant ses informations de paiement. Le traitement de la transaction est délégué à Stripe qui gère la sécurisation des données bancaires. Une fois le paiement validé, l'abonnement est activé sur le compte de l'utilisateur.

### **Etapes du flux principal :**

L'utilisateur accède à la page de souscription et sélectionne un abonnement
L'utilisateur est redirigé vers le formulaire de paiement Stripe (Stripe Checkout ou widget intégré)
L'utilisateur saisit ses informations bancaires (numéro de carte, date d'expiration, CVV)
L'utilisateur confirme le paiement
Stripe traite la transaction et retourne le résultat au système
Le système reçoit la confirmation de paiement via webhook Stripe
Le système active l'abonnement sur le compte de l'utilisateur
L'utilisateur est redirigé vers une page de confirmation avec le récapitulatif de sa souscription

### Scénario alternatifs et exception :

Le paiement est refusé par Stripe (fonds insuffisants, carte expirée, etc.) → un message d'erreur est affiché, l'abonnement n'est pas activé et l'utilisateur est invité à réessayer avec un autre moyen de paiement
L'utilisateur abandonne le formulaire de paiement avant confirmation → aucune transaction n'est effectuée, l'abonnement reste inactif
Le webhook Stripe n'est pas reçu par le système → un mécanisme de retry est déclenché, l'activation de l'abonnement est différée jusqu'à confirmation
La session expire pendant le processus de paiement → l'utilisateur est redirigé vers la page de connexion, la transaction est annulée
Une erreur technique survient côté Stripe → un message d'erreur générique est affiché, aucune somme n'est débitée

### **Règles de gestion :**

RG-01 : L'utilisateur doit être authentifié pour accéder au paiement
RG-02 : Les données bancaires ne transitent jamais par le système, elles sont gérées exclusivement par Stripe (conformité PCI-DSS)
RG-03 : L'abonnement est activé uniquement après réception et validation du webhook de confirmation Stripe
RG-04 : Chaque transaction donne lieu à l'émission automatique d'une facture envoyée par email à l'utilisateur
RG-05 : Un utilisateur ne peut pas souscrire à un abonnement déjà actif sur son compte
RG-06 : En cas d'échec de paiement, aucun montant ne doit être débité
RG-07 : Le renouvellement de l'abonnement est automatique à échéance, sauf résiliation explicite de l'utilisateur
RG-08 : L'utilisateur peut résilier son abonnement à tout moment, la résiliation prend effet à la fin de la période en cours

### **Interface utilisateur :**

La page de sélection affiche clairement les différentes offres d'abonnement avec leur tarif et leurs avantages
Le formulaire de paiement est fourni par Stripe (Stripe Checkout ou Stripe Elements) pour garantir la sécurité
Un récapitulatif de la commande (offre choisie, montant, fréquence) est affiché avant la confirmation du paiement
Un indicateur de chargement est affiché pendant le traitement de la transaction
Les messages d'erreur Stripe sont traduits en messages compréhensibles pour l'utilisateur
La page de confirmation affiche le détail de l'abonnement activé et un lien vers la facture

### **Cas de test pour la validation :**

CT-01 : Paiement réussi avec une carte valide → abonnement activé, email de confirmation et facture reçus
CT-02 : Paiement refusé pour carte expirée → message d'erreur affiché, abonnement non activé
CT-03 : Paiement refusé pour fonds insuffisants → message d'erreur affiché, abonnement non activé
CT-04 : Abandon du formulaire avant confirmation → aucune transaction effectuée, abonnement inactif
CT-05 : Tentative de souscription avec un abonnement déjà actif → action bloquée, message informatif affiché
CT-06 : Simulation d'une non-réception du webhook → vérification du mécanisme de retry et de l'activation différée
CT-07 : Tentative d'accès à la page de paiement sans être connecté → redirection vers la page de connexion
CT-08 : Vérification que les données bancaires ne sont pas stockées en base de données du système

### **Post-conditions :**

En cas de succès : la transaction est enregistrée, l'abonnement est activé sur le compte utilisateur, une facture est générée et envoyée par email, et l'utilisateur est redirigé vers la page de confirmation
En cas d'échec : aucun montant n'est débité, l'abonnement reste inactif, l'utilisateur est informé de la raison de l'échec et invité à réessayer
