Voici une proposition de solution pour le TP sur le Design Pattern Builder.

---

# Solution du TP : Construction d'Objets Complexes avec le Pattern Builder

## 1. Le Produit : `Pizza`

La classe `Pizza` contient les attributs et la logique d'affichage. Son constructeur est privé pour forcer l'utilisation du Builder.


```java
// Fichier: Pizza.java
import java.util.ArrayList;
import java.util.List;

public class Pizza {
    private String typePate;
    private String sauce;
    private String fromage;
    private List<String> garnitures;
    private String bordure;

    // Constructeur privé pour forcer l'utilisation du Builder
    private Pizza() {
        this.garnitures = new ArrayList<>();
    }

    // Méthodes "setter" avec visibilité package pour être utilisées par le Builder
    void setTypePate(String typePate) {
        this.typePate = typePate;
    }

    void setSauce(String sauce) {
        this.sauce = sauce;
    }

    void setFromage(String fromage) {
        this.fromage = fromage;
    }

    void addGarniture(String garniture) {
        this.garnitures.add(garniture);
    }

    void setBordure(String bordure) {
        this.bordure = bordure;
    }

    public void afficherDescription() {
        System.out.println("--- Description de la Pizza ---");
        System.out.println("Pâte: " + typePate);
        System.out.println("Sauce: " + sauce);
        System.out.println("Fromage: " + fromage);
        System.out.print("Garnitures: ");
        if (garnitures.isEmpty()) {
            System.out.println("Aucune");
        } else {
            System.out.println(String.join(", ", garnitures));
        }
        System.out.println("Bordure: " + (bordure != null ? bordure : "simple"));
        System.out.println("-----------------------------\n");
    }
}
```


## 2. L'Interface Builder : `PizzaBuilder`

Cette interface définit les étapes de construction d'une pizza. Chaque méthode de construction retourne l'instance du Builder pour permettre le chaînage des appels.


```java
// Fichier: PizzaBuilder.java
public interface PizzaBuilder {
    PizzaBuilder setTypePate(String typePate);
    PizzaBuilder setSauce(String sauce);
    PizzaBuilder setFromage(String fromage);
    PizzaBuilder addGarniture(String garniture);
    PizzaBuilder setBordure(String bordure);
    Pizza build(); // Méthode finale pour obtenir la pizza construite
    void reset(); // Pour réinitialiser le builder et construire une nouvelle pizza
}
```


## 3. Le Builder Concret : `ConcretePizzaBuilder`

Cette classe implémente l'interface `PizzaBuilder` et gère l'instance de `Pizza` en cours de construction.


```java
// Fichier: ConcretePizzaBuilder.java
public class ConcretePizzaBuilder implements PizzaBuilder {
    private Pizza pizza;

    public ConcretePizzaBuilder() {
        reset();
    }

    @Override
    public void reset() {
        this.pizza = new Pizza(); // Crée une nouvelle instance de Pizza pour la construction
    }

    @Override
    public PizzaBuilder setTypePate(String typePate) {
        pizza.setTypePate(typePate);
        return this; // Retourne l'instance du builder pour le chaînage
    }

    @Override
    public PizzaBuilder setSauce(String sauce) {
        pizza.setSauce(sauce);
        return this;
    }

    @Override
    public PizzaBuilder setFromage(String fromage) {
        pizza.setFromage(fromage);
        return this;
    }

    @Override
    public PizzaBuilder addGarniture(String garniture) {
        pizza.addGarniture(garniture);
        return this;
    }

    @Override
    public PizzaBuilder setBordure(String bordure) {
        pizza.setBordure(bordure);
        return this;
    }

    @Override
    public Pizza build() {
        Pizza builtPizza = this.pizza; // Récupère la pizza construite
        reset(); // Réinitialise le builder pour une future construction
        return builtPizza;
    }
}
```


## 4. Le Directeur : `PizzeriaDirector`

Le `PizzeriaDirector` encapsule les séquences de construction pour les pizzas prédéfinies.


```java
// Fichier: PizzeriaDirector.java
public class PizzeriaDirector {

    public void buildMargherita(PizzaBuilder builder) {
        builder.reset(); // S'assure que le builder est propre
        builder.setTypePate("fine")
               .setSauce("tomate")
               .setFromage("mozzarella");
        // Pas de garnitures spécifiques, bordure simple par défaut
    }

    public void buildVegetarienne(PizzaBuilder builder) {
        builder.reset();
        builder.setTypePate("épaisse")
               .setSauce("crème fraîche")
               .setFromage("chèvre")
               .addGarniture("champignons")
               .addGarniture("poivrons")
               .addGarniture("oignons")
               .setBordure("simple");
    }

    // On pourrait ajouter d'autres recettes ici
}
```


## 5. Utilisation : `Main`

La classe `Main` démontre comment utiliser le Builder et le Director pour construire différentes pizzas.


```java
// Fichier: Main.java
public class Main {
    public static void main(String[] args) {
        ConcretePizzaBuilder builder = new ConcretePizzaBuilder();
        PizzeriaDirector director = new PizzeriaDirector();

        // 1. Construction d'une pizza Margherita via le Director
        System.out.println("Construction d'une pizza Margherita:");
        director.buildMargherita(builder);
        Pizza margherita = builder.build();
        margherita.afficherDescription();

        // 2. Construction d'une pizza Végétarienne via le Director
        System.out.println("Construction d'une pizza Végétarienne:");
        director.buildVegetarienne(builder);
        Pizza vegetarienne = builder.build();
        vegetarienne.afficherDescription();

        // 3. Construction d'une pizza entièrement personnalisée directement avec le Builder
        System.out.println("Construction d'une pizza personnalisée:");
        builder.reset(); // Réinitialise le builder pour une nouvelle construction
        Pizza personnalisee = builder.setTypePate("épaisse")
                                     .setSauce("crème fraîche")
                                     .setFromage("mozzarella")
                                     .addGarniture("jambon")
                                     .addGarniture("ananas") // Oui, même l'ananas !
                                     .addGarniture("olives")
                                     .setBordure("fourrée au fromage")
                                     .build();
        personnalisee.afficherDescription();
    }
}
```


### Sortie du programme


```
Construction d'une pizza Margherita:
--- Description de la Pizza ---
Pâte: fine
Sauce: tomate
Fromage: mozzarella
Garnitures: Aucune
Bordure: simple
-----------------------------

Construction d'une pizza Végétarienne:
--- Description de la Pizza ---
Pâte: épaisse
Sauce: crème fraîche
Fromage: chèvre
Garnitures: champignons, poivrons, oignons
Bordure: simple
-----------------------------

Construction d'une pizza personnalisée:
--- Description de la Pizza ---
Pâte: épaisse
Sauce: crème fraîche
Fromage: mozzarella
Garnitures: jambon, ananas, olives
Bordure: fourrée au fromage
-----------------------------
```


## Explications et Justifications

*   **`Pizza` (Produit) :** Son constructeur est privé, garantissant que seule une instance de `PizzaBuilder` peut créer une `Pizza`. Les setters sont de visibilité `package` pour limiter leur accès au `ConcretePizzaBuilder` dans le même package, renforçant l'encapsulation. La méthode `afficherDescription()` est la représentation finale de l'objet.
*   **`PizzaBuilder` (Interface Builder) :** Définit un contrat pour toutes les étapes de construction. Le retour de `this` dans chaque méthode de construction permet le chaînage fluide des appels (`builder.setTypePate(...).setSauce(...)`). La méthode `build()` est le point final où l'objet construit est retourné. La méthode `reset()` est ajoutée pour permettre la réutilisation du builder pour de nouvelles constructions.
*   **`ConcretePizzaBuilder` (Builder Concret) :** Implémente les étapes définies par l'interface. Il maintient une instance de `Pizza` et la modifie au fur et à mesure des appels. `build()` retourne la pizza et réinitialise le builder, le rendant prêt pour une nouvelle construction.
*   **`PizzeriaDirector` (Directeur) :** Ce composant est optionnel mais très utile. Il encapsule les logiques de construction complexes ou récurrentes (nos "recettes" de pizzas). Le client (`Main`) n'a pas besoin de connaître les étapes exactes pour construire une Margherita ; il demande simplement au directeur de le faire via le builder. Cela découple le client des détails de construction et permet de gérer facilement de nouvelles recettes.
*   **`Main` (Client) :** Le client interagit avec le `PizzeriaDirector` ou directement avec le `ConcretePizzaBuilder` via l'interface `PizzaBuilder`. Il ne manipule jamais directement le constructeur de `Pizza`, ce qui rend la création d'objets complexes plus lisible, plus flexible et moins sujette aux erreurs (pas de constructeurs avec de nombreux paramètres).

Ce pattern respecte le principe d'ouverture/fermeture (Open/Closed Principle) car l'ajout de nouvelles variations de pizzas (par exemple, une nouvelle recette) peut se faire en ajoutant une méthode au `Director` ou en utilisant directement le `Builder`, sans modifier les classes `Pizza` ou `PizzaBuilder` existantes.