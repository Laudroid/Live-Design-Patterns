Voici un sujet de TP conçu pour vous guider dans l'application du pattern Strategy, en tenant compte de l'utilisation d'outils d'IA.

---

## TP : Maîtrise du Pattern Strategy - Gestion des Coûts de Livraison

### Contexte

Imaginez que vous développez un système e-commerce. Le calcul des frais de livraison est une fonctionnalité essentielle, mais elle peut varier considérablement : livraison standard à prix fixe, livraison basée sur le poids du colis, livraison express avec un coût plus élevé, etc. De nouvelles méthodes de livraison peuvent apparaître à tout moment.

### Objectif Pédagogique

Appliquer le pattern de conception **Strategy** pour encapsuler et rendre interchangeables différents algorithmes de calcul de coûts de livraison, sans modifier le code central de traitement des commandes.

### Problématique

Comment gérer cette diversité d'algorithmes de calcul de frais de livraison de manière flexible, permettant d'ajouter de nouvelles stratégies ou de modifier les existantes sans impacter le reste du système ?

### Consignes Générales

*   **Langage de votre choix :** Vous êtes libre d'utiliser le langage de programmation avec lequel vous êtes le plus à l'aise (Java, C#, Python, PHP, TypeScript, etc.).
*   **Utilisation de l'IA :** L'utilisation d'outils d'IA générative (ChatGPT, Copilot, etc.) est encouragée pour vous aider dans la rédaction du code ou la compréhension des concepts. L'objectif est d'apprendre et de comprendre, pas de réinventer la roue. Soyez transparent sur son utilisation et, surtout, **assurez-vous de comprendre le code généré et de pouvoir l'expliquer.** C'est votre compréhension qui est évaluée.
*   **Clarté du code :** Structurez votre code de manière claire et lisible.
*   **Commentaires :** Ajoutez des commentaires pertinents pour expliquer vos choix de conception et les parties complexes.

---

### Étapes du TP

#### Étape 0 : Préparation (Classe `Commande`)

Pour simuler une commande, créez une classe simple `Commande` qui contiendra au minimum :
*   Un identifiant (`idCommande`).
*   Un poids total (`poidsTotal` en kg).
*   Un montant total des articles (`montantTotal` hors frais de livraison).

Cette classe servira de base pour les calculs.

#### Étape 1 : Définition de l'Interface (ou Classe Abstraite) de Stratégie

Créez une interface (ou une classe abstraite, selon votre langage) qui définira le contrat pour toutes les stratégies de calcul de frais de livraison.

*   Nommez-la `ICalculateurFraisLivraison` (ou `ShippingCostStrategy`).
*   Elle doit déclarer une méthode unique, par exemple `calculerFrais(Commande commande)`, qui retournera un `double` (le coût de livraison).

#### Étape 2 : Implémentation des Stratégies Concrètes

Implémentez au moins deux classes concrètes qui respectent l'interface `ICalculateurFraisLivraison` :

1.  **`LivraisonStandard` (Forfaitaire) :**
    *   Calcule un coût de livraison fixe (par exemple, 5.00 €), quelle que soit la commande.
2.  **`LivraisonParPoids` :**
    *   Calcule le coût de livraison en fonction du poids total de la commande. Par exemple : 2.00 € par kg.

#### Étape 3 : Le Contexte (La Classe Utilisatrice)

Créez une classe qui utilisera ces stratégies. C'est la classe qui représente le "contexte" dans le pattern Strategy.

*   Nommez-la `ProcesseurCommande` (ou `OrderProcessor`).
*   Cette classe doit contenir une référence à une instance de `ICalculateurFraisLivraison`.
*   Elle doit avoir une méthode pour définir la stratégie de livraison à utiliser, par exemple `setStrategieLivraison(ICalculateurFraisLivraison strategie)`.
*   Elle doit avoir une méthode pour traiter une commande, par exemple `traiterCommande(Commande commande)`, qui utilisera la stratégie de livraison actuellement configurée pour calculer les frais et afficher le montant total (articles + livraison).

#### Étape 4 : Mise en Pratique et Test

Dans une méthode `main` (ou un script de test) :

1.  Créez plusieurs instances de `Commande` avec des poids et montants différents.
2.  Instanciez votre `ProcesseurCommande`.
3.  Testez le `ProcesseurCommande` en lui assignant successivement différentes stratégies de livraison (`LivraisonStandard`, `LivraisonParPoids`) et en traitant les commandes.
4.  Affichez clairement les résultats pour chaque scénario (commande, stratégie utilisée, frais de livraison calculés, montant total final).

#### Étape 5 : Ajout d'une Nouvelle Stratégie

Démontrez la flexibilité du pattern en ajoutant une nouvelle stratégie sans modifier les classes existantes (`ICalculateurFraisLivraison`, `LivraisonStandard`, `LivraisonParPoids`, `ProcesseurCommande`).

1.  **`LivraisonExpress` :**
    *   Calcule un coût de livraison plus élevé, par exemple un forfait de 10.00 € plus 1% du montant total des articles.
2.  Testez cette nouvelle stratégie avec vos commandes existantes.

#### Étape 6 : Réflexion et Discussion

Rédigez un court paragraphe (quelques lignes) pour répondre aux questions suivantes :

1.  **Avantages :** Quels sont les principaux avantages de l'utilisation du pattern Strategy dans ce scénario par rapport à une approche utilisant des `if/else if` ou un `switch` pour choisir l'algorithme ?
2.  **Autres Contextes :** Dans quels autres contextes (en dehors des frais de livraison) pourriez-vous appliquer le pattern Strategy ? Donnez un exemple concret.
3.  **Inconvénients :** Y a-t-il des inconvénients ou des situations où ce pattern serait moins adapté ?
4.  **Retour sur l'IA :** Comment l'IA vous a-t-elle aidé(e) dans ce TP ? Avez-vous rencontré des difficultés spécifiques avec le code généré ou la compréhension des concepts, et comment les avez-vous surmontées ?

---

### Livrables

*   L'ensemble de votre code source, organisé dans des fichiers appropriés.
*   Un fichier texte ou Markdown contenant les réponses aux questions de l'Étape 6.

### Conseils

*   Commencez par les bases : l'interface et deux implémentations simples.
*   Testez chaque étape au fur et à mesure.
*   N'hésitez pas à expérimenter et à poser des questions à votre IA ou à vos pairs si vous êtes bloqué.
*   Le plus important est la compréhension des principes du pattern Strategy et de sa valeur ajoutée.

Amusez-vous bien avec ce TP !