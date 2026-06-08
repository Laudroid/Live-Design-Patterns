Bonjour à toutes et à tous !

Ce TP est conçu pour vous permettre de mettre en pratique le pattern de conception **Builder**. L'objectif est de construire des objets complexes de manière flexible et maintenable, en séparant clairement les étapes de construction de la représentation finale de l'objet.

---

### TP : Construction d'Objets Complexes avec le Pattern Builder

**Objectif du TP :**
Maîtriser l'implémentation du pattern Builder pour construire des objets complexes de manière flexible et maintenable.

**Contexte :**
Lorsqu'un objet possède de nombreux attributs, dont certains sont optionnels, ou que sa construction implique une séquence d'étapes spécifiques, les constructeurs classiques peuvent devenir lourds et difficiles à gérer (le fameux "telescoping constructor anti-pattern"). Le pattern Builder offre une solution élégante pour séparer la construction de la représentation d'un objet, permettant ainsi de créer différentes variations du même produit avec le même processus de construction.

**Énoncé du Problème :**
Imaginez que vous développez un système de commande pour une pizzeria. Une pizza est un objet complexe : elle a une pâte, une sauce, du fromage, et une multitude de garnitures optionnelles. De plus, la pizzeria propose des "recettes" prédéfinies (Margherita, Végétarienne, etc.) mais aussi la possibilité de créer sa propre pizza personnalisée.

Votre tâche est d'implémenter ce système de construction de pizzas en utilisant le pattern Builder.

**Caractéristiques d'une Pizza :**

*   **Obligatoires :**
    *   Type de pâte (ex: "fine", "épaisse", "sans gluten")
    *   Sauce (ex: "tomate", "crème fraîche", "pesto")
    *   Fromage (ex: "mozzarella", "chèvre", "emmental")
*   **Optionnelles :**
    *   Garnitures (une liste de chaînes de caractères, ex: "champignons", "jambon", "olives", "poivrons", "oignons", "ananas" - *oui, même l'ananas !*)
    *   Bordure (ex: "simple", "fourrée au fromage", "fourrée au chèvre")

La classe `Pizza` devra également disposer d'une méthode `afficherDescription()` qui affichera la composition complète de la pizza.

---

**Travail à Réaliser :**

1.  **Le Produit : `Pizza`**
    *   Créez une classe `Pizza` avec les attributs nécessaires pour stocker les caractéristiques mentionnées ci-dessus.
    *   Le constructeur de la classe `Pizza` devra être **privé**. C'est une caractéristique clé du pattern Builder pour s'assurer que l'objet ne peut être instancié que via le Builder.
    *   Implémentez des méthodes "setter" (qui peuvent être privées ou avec une visibilité package) pour que le Builder puisse modifier les attributs de la pizza.
    *   Implémentez la méthode `afficherDescription()` qui affichera de manière lisible tous les composants de la pizza.

2.  **L'Interface Builder : `PizzaBuilder`**
    *   Créez une interface `PizzaBuilder`.
    *   Cette interface définira les méthodes pour chaque étape de construction de la pizza. Par exemple :
        *   `setTypePate(String typePate)`
        *   `setSauce(String sauce)`
        *   `setFromage(String fromage)`
        *   `addGarniture(String garniture)` (cette méthode pourra être appelée plusieurs fois)
        *   `setBordure(String bordure)`
    *   L'interface devra également inclure une méthode `build()` qui retournera l'objet `Pizza` final.

3.  **Le Builder Concret : `ConcretePizzaBuilder`**
    *   Créez une classe `ConcretePizzaBuilder` qui implémente l'interface `PizzaBuilder`.
    *   Cette classe maintiendra une instance de `Pizza` qu'elle construira progressivement.
    *   Chaque méthode de l'interface `PizzaBuilder` sera implémentée pour modifier l'état de l'objet `Pizza` interne.
    *   La méthode `build()` retournera l'instance de `Pizza` entièrement construite. Pensez à réinitialiser l'état du builder après un `build()` si vous souhaitez le réutiliser pour construire une nouvelle pizza.

4.  **Le Directeur (Optionnel mais recommandé) : `PizzeriaDirector`**
    *   Créez une classe `PizzeriaDirector`.
    *   Cette classe n'est pas obligatoire pour le pattern Builder, mais elle est très utile pour encapsuler des séquences de construction courantes (nos "recettes" de pizzas).
    *   Elle aura des méthodes comme `buildMargherita(PizzaBuilder builder)` ou `buildVegetarienne(PizzaBuilder builder)` qui prendront un `PizzaBuilder` en paramètre et l'utiliseront pour construire une pizza spécifique.

5.  **Utilisation : `Main`**
    *   Dans une classe `Main` (ou un fichier de test), démontrez l'utilisation de votre Builder :
        *   Construisez une pizza "Margherita" en utilisant le `PizzeriaDirector`.
        *   Construisez une pizza "Végétarienne" en utilisant le `PizzeriaDirector`.
        *   Construisez une pizza entièrement personnalisée directement avec le `ConcretePizzaBuilder` (ex: pâte épaisse, sauce crème, mozzarella, jambon, ananas, bordure fourrée).
        *   Affichez la description de chaque pizza construite.

---

**Critères d'Évaluation / Attentes :**

*   **Respect du pattern Builder :** La structure du code doit clairement refléter les composants du pattern (Produit, Interface Builder, Concrete Builder, Director).
*   **Clarté et lisibilité du code :** Le code doit être bien organisé, commenté si nécessaire, et facile à comprendre.
*   **Gestion des attributs :** Les attributs obligatoires et optionnels de la pizza doivent être gérés correctement.
*   **Fonctionnalité :** Les pizzas doivent être construites comme attendu et leurs descriptions affichées correctement.
*   **Utilisation du `Director` :** Le `PizzeriaDirector` doit être utilisé pour les recettes prédéfinies, démontrant l'encapsulation des processus de construction.

---

**Conseils pour l'Apprentissage (avec l'IA) :**

L'intelligence artificielle est un outil puissant pour générer du code et explorer des concepts. Cependant, l'objectif de ce TP est de *comprendre* et *appliquer* le pattern, pas seulement de copier-coller une solution.

Utilisez l'IA de manière constructive :

*   **Générez des ébauches :** Demandez-lui la structure de base du pattern Builder pour une `Pizza`. Cela peut vous faire gagner du temps sur la syntaxe de base.
*   **Explorez des alternatives :** "Comment pourrais-je gérer les garnitures de manière plus flexible ?" ou "Y a-t-il d'autres façons d'implémenter le `Director` ?"
*   **Comprenez des concepts :** "Explique-moi pourquoi le constructeur de `Pizza` doit être privé dans ce contexte." ou "Quelle est la différence entre le Builder et la Factory Method ?"
*   **Déboguez :** Si vous rencontrez un problème, demandez à l'IA : "Pourquoi cette partie de mon code ne fonctionne-t-elle pas comme prévu ?"

**Votre rôle reste essentiel :** Analysez le code généré, modifiez-le, adaptez-le à vos besoins spécifiques, et surtout, soyez capable d'expliquer chaque partie et les choix de conception que vous avez faits. N'hésitez pas à expérimenter et à poser des questions à l'IA pour approfondir votre compréhension plutôt que de simplement lui demander la solution complète.

Bon courage pour ce TP ! C'est une excellente occasion de renforcer vos compétences en conception logicielle.