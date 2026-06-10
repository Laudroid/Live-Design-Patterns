Voici un sujet de TP conçu pour vous guider dans l'implémentation du pattern Composite.

---

## TP : Implémentation du Pattern Composite pour la Gestion de Menus Interactifs

### Objectif du TP

Appliquer le pattern de conception Composite pour structurer et manipuler une hiérarchie d'éléments de menu (menus et sous-menus) de manière uniforme.

### Contexte

Vous êtes chargé de développer un système de gestion de menus pour une application. Ce système doit permettre de créer des menus principaux, des sous-menus, et des éléments de menu individuels (qui déclenchent une action). La particularité est que le code client doit pouvoir interagir avec un menu ou un élément de menu de la même manière, sans avoir à distinguer leur nature composite ou atomique.

### Prérequis

*   Connaissance des principes de la Programmation Orientée Objet (interfaces, classes abstraites, héritage).
*   Maîtrise des collections (Listes, par exemple).
*   Compréhension de base des lambdas ou des interfaces fonctionnelles (pour les actions).

### Énoncé

Implémentez le pattern Composite pour modéliser la structure d'un système de menus.

#### Étape 1 : Le Composant Abstraite (`MenuComponent`)

Créez une classe abstraite ou une interface nommée `MenuComponent`. Elle définira l'interface commune pour tous les éléments de votre structure de menu.

1.  **Méthodes communes :**
    *   `String getName()`: Retourne le nom de l'élément (ex: "Fichier", "Ouvrir", "Édition").
    *   `void display()`: Affiche l'élément. Pour un menu, cela affichera son nom et ses enfants. Pour un élément de menu, cela affichera juste son nom.
    *   `void add(MenuComponent component)`: Ajoute un composant enfant. Par défaut, cette méthode devrait lever une `UnsupportedOperationException` car seuls les composites peuvent ajouter des enfants.
    *   `void remove(MenuComponent component)`: Supprime un composant enfant. Par défaut, cette méthode devrait lever une `UnsupportedOperationException`.
    *   `MenuComponent getChild(int i)`: Retourne un enfant à un index donné. Par défaut, cette méthode devrait lever une `UnsupportedOperationException`.

#### Étape 2 : L'Élément Feuille (`MenuItem`)

Créez une classe concrète `MenuItem` qui représente un élément de menu individuel (une "feuille" dans l'arbre).

1.  **Héritage :** `MenuItem` doit hériter de `MenuComponent`.
2.  **Attributs :**
    *   Un nom (`String`).
    *   Une action à exécuter (`Runnable` ou une interface fonctionnelle similaire).
3.  **Constructeur :** Permet d'initialiser le nom et l'action.
4.  **Implémentation des méthodes de `MenuComponent` :**
    *   `getName()`: Retourne le nom de l'élément.
    *   `display()`: Affiche le nom de l'élément (ex: "  - Ouvrir").
    *   `add()`, `remove()`, `getChild()`: Maintenez l'implémentation par défaut de `MenuComponent` (lever `UnsupportedOperationException`).
5.  **Méthode spécifique :**
    *   `void executeAction()`: Exécute l'action associée à cet élément de menu.

#### Étape 3 : Le Composite (`Menu`)

Créez une classe concrète `Menu` qui représente un menu (un "nœud composite" dans l'arbre), pouvant contenir d'autres `MenuComponent` (des `MenuItem` ou d'autres `Menu`).

1.  **Héritage :** `Menu` doit hériter de `MenuComponent`.
2.  **Attributs :**
    *   Un nom (`String`).
    *   Une liste de `MenuComponent` enfants (`List<MenuComponent>`).
3.  **Constructeur :** Permet d'initialiser le nom.
4.  **Implémentation des méthodes de `MenuComponent` :**
    *   `getName()`: Retourne le nom du menu.
    *   `display()`: Affiche le nom du menu (ex: "Menu Fichier:") puis itère sur sa liste d'enfants pour appeler `display()` sur chacun d'eux.
    *   `add(MenuComponent component)`: Ajoute le composant à la liste des enfants.
    *   `remove(MenuComponent component)`: Supprime le composant de la liste des enfants.
    *   `getChild(int i)`: Retourne l'enfant à l'index `i` de la liste.

#### Étape 4 : Construction et Test de la Structure

Dans une classe principale (par exemple, `Main` ou `Application`), créez une structure de menus complexe et testez son affichage et l'exécution d'actions.

1.  **Création d'éléments :**
    *   Créez plusieurs `MenuItem` avec des actions simples (ex: afficher un message sur la console).
    *   Créez des `Menu` (ex: "Fichier", "Édition", "Aide").
2.  **Assemblage :**
    *   Ajoutez des `MenuItem` directement à des `Menu`.
    *   Créez des sous-menus (ex: un `Menu` "Nouveau" dans le `Menu` "Fichier") et ajoutez-y des `MenuItem`.
    *   Construisez un menu principal qui contient tous les menus de premier niveau.
3.  **Affichage :** Appelez la méthode `display()` sur le menu principal pour voir toute la structure s'afficher.
4.  **Interaction :** Simulez la sélection d'un `MenuItem` et appelez sa méthode `executeAction()`.

### Consignes Générales

*   **Langage :** Utilisez le langage de programmation de votre choix (Java, C#, Python, etc.). Les exemples ci-dessus sont orientés Java, mais le concept est universel.
*   **Clarté du code :** Votre code doit être propre, bien structuré et commenté si nécessaire.
*   **Tests :** Assurez-vous que votre implémentation fonctionne comme prévu en créant des scénarios de test variés.

### Utilisation de l'IA

L'utilisation d'outils d'IA pour vous aider dans ce TP est encouragée. Voici quelques pistes pour une utilisation efficace :

*   **Génération de squelettes de code :** Demandez à l'IA de générer la structure de base des classes `MenuComponent`, `MenuItem`, et `Menu` en vous basant sur les descriptions des étapes.
*   **Explication de concepts :** Si une partie du pattern Composite ou une notion de POO vous échappe, interrogez l'IA pour obtenir des clarifications ou des exemples supplémentaires.
*   **Refactoring et amélioration :** Une fois votre code écrit, demandez à l'IA des suggestions pour l'améliorer, le rendre plus idiomatique ou plus performant (sans altérer la logique du pattern).
*   **Débogage :** Si vous rencontrez un bug, décrivez-le à l'IA avec votre code pour obtenir des pistes de résolution.

**Important :** L'objectif est d'apprendre et de comprendre le pattern. Ne vous contentez pas de copier-coller. Prenez le temps de lire, de comprendre et d'adapter le code généré par l'IA. Vérifiez toujours la pertinence et l'exactitude des réponses de l'IA. C'est votre compréhension qui est évaluée, pas la capacité de l'IA à écrire du code.

### Pour aller plus loin (Optionnel)

*   **Recherche d'éléments :** Implémentez une méthode dans `MenuComponent` (ou une classe utilitaire) qui permet de rechercher un `MenuItem` par son nom dans toute la structure arborescente.
*   **Gestion des raccourcis clavier :** Ajoutez la possibilité d'associer un raccourci clavier à un `MenuItem` et une méthode pour "activer" un élément via son raccourci.
*   **Persistance :** Réfléchissez à la manière dont vous pourriez sauvegarder et charger votre structure de menus (par exemple, en JSON ou XML).

---