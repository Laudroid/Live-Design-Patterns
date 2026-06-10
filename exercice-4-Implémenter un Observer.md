Bonjour à toutes et à tous,

Ce TP a pour objectif de vous familiariser avec l'implémentation du pattern de conception **Observer**.

---

### TP : Implémentation du Pattern Observer - Système de Notification de Stock Produit

**Objectif du TP :**
Mettre en œuvre le pattern de conception Observer pour créer un système de notification réactif aux changements de stock d'un produit.

**Contexte :**
Dans un système de gestion de stock pour une boutique en ligne, plusieurs départements (Marketing, Logistique, Service Clientèle, etc.) doivent être informés en temps réel des changements de niveau de stock d'un produit. Chaque département réagit différemment à ces changements.

**Énoncé du Problème :**

Vous devez concevoir et implémenter un système où :
1.  Un `Produit` possède un niveau de stock (`stock`).
2.  Ce `Produit` est capable d'informer des entités intéressées (les "observateurs") chaque fois que son niveau de stock change.
3.  Différents types d'observateurs réagissent aux changements de stock :
    *   Le **Département Marketing** : Il doit être notifié si le stock d'un produit passe sous un seuil de 10 unités (pour envisager une promotion ou une alerte de réapprovisionnement).
    *   Le **Département Logistique** : Il doit être notifié si le stock d'un produit passe sous un seuil critique de 5 unités (pour déclencher une commande fournisseur).
    *   Le **Service Clientèle** : Il doit être notifié si le stock d'un produit atteint 0 (pour informer les clients en attente ou mettre à jour le site).

Votre implémentation doit démontrer comment le `Produit` (le sujet) notifie ses observateurs sans avoir une connaissance directe de leur type ou de leur logique spécifique.

---

**Instructions :**

1.  **Définir les interfaces du pattern Observer :**
    *   Créez une interface `Sujet` (ou `Observable`) avec les méthodes suivantes :
        *   `enregistrerObservateur(Observateur observateur)`
        *   `desenregistrerObservateur(Observateur observateur)`
        *   `notifierObservateurs()`
    *   Créez une interface `Observateur` avec une méthode `mettreAJour(Sujet sujet)` (ou `mettreAJour(String nomProduit, int nouveauStock)` si vous préférez passer directement les données pertinentes).

2.  **Implémenter le Sujet Concret :**
    *   Créez une classe `Produit` qui implémente l'interface `Sujet`.
    *   Cette classe doit avoir des attributs pour le `nom` du produit (String) et le `stock` (int).
    *   Elle doit maintenir une liste d'observateurs enregistrés.
    *   Implémentez les méthodes de l'interface `Sujet`.
    *   Ajoutez une méthode `setStock(int nouveauStock)` qui met à jour le stock et, si le stock a réellement changé, appelle `notifierObservateurs()`.

3.  **Implémenter les Observateurs Concrets :**
    *   Créez les classes `DepartementMarketing`, `DepartementLogistique`, et `ServiceClients`.
    *   Chacune de ces classes doit implémenter l'interface `Observateur`.
    *   Dans la méthode `mettreAJour()`, chaque département doit :
        *   Récupérer le nom et le niveau de stock du `Produit` (via l'objet `Sujet` passé en paramètre).
        *   Afficher un message pertinent sur la console en fonction de sa logique spécifique (seuils mentionnés dans l'énoncé).

4.  **Mettre en Scène (Classe Principale) :**
    *   Créez une classe `Main` (ou `Application`).
    *   Dans la méthode `main` :
        *   Instanciez un `Produit` (par exemple, "Ordinateur Portable" avec un stock initial de 20).
        *   Instanciez un `DepartementMarketing`, un `DepartementLogistique` et un `ServiceClients`.
        *   Enregistrez ces trois observateurs auprès du `Produit`.
        *   Simulez des changements de stock du produit en appelant `setStock()` plusieurs fois avec différentes valeurs (ex: 15, 8, 3, 0).
        *   Observez les notifications affichées par chaque département.
        *   (Optionnel) : Désenregistrez un observateur (par exemple, le Département Marketing) et simulez un nouveau changement de stock pour vérifier qu'il n'est plus notifié.

---

**Consignes et Conseils :**

*   **Langage de programmation :** Vous êtes libre d'utiliser le langage de votre choix (Java, C#, Python, PHP, etc.).
*   **Clarté du code :** Veillez à la lisibilité de votre code. Utilisez des noms de variables et de méthodes explicites.
*   **Utilisation de l'IA :** L'IA peut être un excellent assistant. N'hésitez pas à l'utiliser pour :
    *   Générer le boilerplate des interfaces et des classes.
    *   Demander des exemples de syntaxe pour des structures spécifiques.
    *   Explorer des implémentations alternatives ou des optimisations.
    *   Débugger des erreurs.
    *   **Cependant, votre rôle est de comprendre le code généré, de l'adapter à ce TP, et de vous assurer qu'il répond précisément aux exigences. L'objectif est d'apprendre le pattern, pas seulement de copier-coller une solution.**
*   **Réflexion :** Une fois le code fonctionnel, prenez un moment pour réfléchir :
    *   Quels sont les avantages de cette approche par rapport à une notification directe ?
    *   Quand est-il pertinent d'utiliser le pattern Observer ?
    *   Quelles sont les limites ou les inconvénients potentiels ?

---

**Rendu attendu :**

*   Le code source complet de votre implémentation (interfaces, classes `Produit`, `DepartementMarketing`, `DepartementLogistique`, `ServiceClients`, et `Main`).
*   La sortie console de l'exécution de votre programme, montrant les différentes notifications.
*   (Optionnel) Une brève explication des avantages que vous percevez dans l'utilisation de ce pattern pour ce type de problème.

---

Bon courage pour ce TP ! N'hésitez pas à expérimenter et à poser des questions si vous rencontrez des difficultés.