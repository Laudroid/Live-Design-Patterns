Voici un sujet de TP conçu pour illustrer le pattern Decorator, en tenant compte de l'utilisation de l'IA par les apprenants.

---

## TP : Maîtriser le Pattern Decorator - La Cafeteria Personnalisée

### Objectif du TP

Appliquer le pattern Decorator pour étendre dynamiquement les fonctionnalités d'un objet existant, en ajoutant des comportements de manière flexible et sans modifier sa structure initiale.

### Contexte

Vous travaillez sur le système de commande d'une cafeteria moderne. Les clients aiment personnaliser leurs boissons avec divers ingrédients (lait, sucre, crème fouettée, sirop, etc.). Le système doit être capable de calculer le prix total de chaque boisson personnalisée et de générer une description détaillée de sa composition.

L'enjeu est de pouvoir ajouter de nouveaux ingrédients ou de modifier les prix/descriptions des ingrédients existants sans avoir à modifier les classes de base des boissons ni les classes des autres ingrédients.

### Énoncé du Problème

Concevez une architecture logicielle permettant d'ajouter des ingrédients supplémentaires à une boisson de base (par exemple, un café simple) de manière dynamique. Chaque ajout doit impacter à la fois le coût total de la boisson et sa description. L'objectif est de démontrer comment le pattern Decorator permet cette flexibilité et cette extensibilité.

### Étapes du TP

1.  **Définir l'Interface `Boisson` (Component)**
    *   Créez une interface ou une classe abstraite `Boisson` qui déclare les méthodes essentielles pour toutes les boissons, par exemple :
        *   `getCout()` : Retourne le coût de la boisson.
        *   `getDescription()` : Retourne une chaîne de caractères décrivant la boisson.

2.  **Implémenter la Boisson de Base (Concrete Component)**
    *   Créez une classe concrète `CafeSimple` qui implémente l'interface `Boisson`.
    *   Initialisez son coût et sa description de base (par exemple, "Café simple", 2.00€).

3.  **Créer la Classe Abstraite `DecorateurBoisson` (Decorator)**
    *   Créez une classe abstraite `DecorateurBoisson` qui implémente également l'interface `Boisson`.
    *   Cette classe doit contenir une référence à un objet `Boisson` (le composant qu'elle va décorer).
    *   Les méthodes `getCout()` et `getDescription()` de cette classe abstraite doivent être implémentées pour déléguer l'appel à l'objet `Boisson` qu'elle contient. Cela permet aux décorateurs concrets de surcharger ces méthodes pour ajouter leur propre comportement.

4.  **Implémenter les Décorateurs Concrets (Concrete Decorators)**
    *   Créez plusieurs classes de décorateurs concrets, par exemple :
        *   `Lait` : Ajoute 0.50€ au coût et "avec Lait" à la description.
        *   `Sucre` : Ajoute 0.20€ au coût et "avec Sucre" à la description.
        *   `CremeFouettee` : Ajoute 0.75€ au coût et "avec Crème Fouettée" à la description.
    *   Chaque décorateur doit étendre `DecorateurBoisson` et surcharger `getCout()` et `getDescription()` pour ajouter son propre comportement *en plus* de celui de la boisson décorée.

5.  **Tester et Vérifier**
    *   Dans votre programme principal, créez différentes combinaisons de boissons et de décorateurs :
        *   Un `CafeSimple` seul.
        *   Un `CafeSimple` décoré avec du `Lait`.
        *   Un `CafeSimple` décoré avec du `Lait` puis du `Sucre`.
        *   Un `CafeSimple` décoré avec de la `CremeFouettee`, puis du `Lait`, puis du `Sucre`.
    *   Pour chaque combinaison, affichez le coût total et la description finale de la boisson.

### Critères d'Évaluation

*   **Respect du Pattern :** L'implémentation suit-elle correctement la structure du pattern Decorator ?
*   **Fonctionnalité :** Le coût et la description sont-ils calculés correctement pour toutes les combinaisons ?
*   **Clarté du Code :** Le code est-il lisible, bien commenté et structuré ?
*   **Justification :** Êtes-vous capable d'expliquer pourquoi le pattern Decorator a été choisi pour ce problème et quels sont ses avantages par rapport à d'autres approches (comme l'héritage multiple ou l'extension de classes) ?

### Pour aller plus loin (Optionnel)

*   **Ajouter un nouveau décorateur :** Implémentez un décorateur `DoubleDoseExpresso` qui modifie non seulement le coût et la description, mais aussi la "force" de la boisson (si vous ajoutez un attribut `getForce()` à `Boisson`).
*   **Gestion des quantités :** Comment pourriez-vous gérer l'ajout de "deux sucres" ou "deux doses de lait" ? Faut-il créer un décorateur `DoubleSucre` ou modifier le décorateur `Sucre` pour qu'il prenne une quantité en paramètre ? Discutez des avantages et inconvénients de chaque approche.
*   **Validation :** Comment empêcherait-on un client d'ajouter deux fois le même ingrédient si cela n'a pas de sens (par exemple, "double crème fouettée" est ok, mais "double café simple" n'a pas de sens si le café est la base) ?