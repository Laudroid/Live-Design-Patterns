Voici une proposition de solution pour le TP sur le Design Pattern Decorator.

---

# Solution du TP : Maîtriser le Pattern Decorator - La Cafeteria Personnalisée

## 1. Définir l'Interface `Boisson` (Component)

Cette interface définit le contrat que toutes les boissons et leurs décorateurs doivent respecter.



```java
// Fichier: Boisson.java
public interface Boisson {
    double getCout();
    String getDescription();
}
```



## 2. Implémenter la Boisson de Base (Concrete Component)

`CafeSimple` est notre boisson de base, l'objet que nous allons décorer.



```java
// Fichier: CafeSimple.java
public class CafeSimple implements Boisson {
    @Override
    public double getCout() {
        return 2.00;
    }

    @Override
    public String getDescription() {
        return "Café simple";
    }
}
```



## 3. Créer la Classe Abstraite `DecorateurBoisson` (Decorator)

Cette classe abstraite est la base de tous nos décorateurs. Elle implémente `Boisson` et contient une référence à l'objet `Boisson` qu'elle décore.



```java
// Fichier: DecorateurBoisson.java
public abstract class DecorateurBoisson implements Boisson {
    protected Boisson boissonDecorée; // Référence au composant que nous décorons

    public DecorateurBoisson(Boisson boisson) {
        this.boissonDecorée = boisson;
    }

    // Délègue les appels aux méthodes de la boisson décorée par défaut
    // Les décorateurs concrets surchargeront ces méthodes pour ajouter leur comportement
    @Override
    public double getCout() {
        return boissonDecorée.getCout();
    }

    @Override
    public String getDescription() {
        return boissonDecorée.getDescription();
    }
}
```



## 4. Implémenter les Décorateurs Concrets (Concrete Decorators)

Chaque décorateur ajoute un coût et une description spécifiques à la boisson qu'il enveloppe.

**Décorateur `Lait`**



```java
// Fichier: Lait.java
public class Lait extends DecorateurBoisson {
    public Lait(Boisson boisson) {
        super(boisson);
    }

    @Override
    public double getCout() {
        return super.getCout() + 0.50; // Ajoute le coût du lait
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", avec Lait"; // Ajoute "avec Lait" à la description
    }
}
```



**Décorateur `Sucre`**



```java
// Fichier: Sucre.java
public class Sucre extends DecorateurBoisson {
    public Sucre(Boisson boisson) {
        super(boisson);
    }

    @Override
    public double getCout() {
        return super.getCout() + 0.20; // Ajoute le coût du sucre
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", avec Sucre"; // Ajoute "avec Sucre" à la description
    }
}
```



**Décorateur `CremeFouettee`**



```java
// Fichier: CremeFouettee.java
public class CremeFouettee extends DecorateurBoisson {
    public CremeFouettee(Boisson boisson) {
        super(boisson);
    }

    @Override
    public double getCout() {
        return super.getCout() + 0.75; // Ajoute le coût de la crème fouettée
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", avec Crème Fouettée"; // Ajoute "avec Crème Fouettée"
    }
}
```



## 5. Tester et Vérifier

La classe `Main` montre comment combiner dynamiquement les décorateurs.



```java
// Fichier: Main.java
public class Main {
    public static void main(String[] args) {
        // 1. Un CaféSimple seul
        Boisson cafeSimple = new CafeSimple();
        System.out.println("Boisson: " + cafeSimple.getDescription() + " | Coût: " + String.format("%.2f", cafeSimple.getCout()) + "€");

        // 2. Un CaféSimple décoré avec du Lait
        Boisson cafeAvecLait = new Lait(new CafeSimple());
        System.out.println("Boisson: " + cafeAvecLait.getDescription() + " | Coût: " + String.format("%.2f", cafeAvecLait.getCout()) + "€");

        // 3. Un CaféSimple décoré avec du Lait puis du Sucre
        Boisson cafeAvecLaitEtSucre = new Sucre(new Lait(new CafeSimple()));
        System.out.println("Boisson: " + cafeAvecLaitEtSucre.getDescription() + " | Coût: " + String.format("%.2f", cafeAvecLaitEtSucre.getCout()) + "€");

        // 4. Un CaféSimple décoré avec de la Crème Fouettée, puis du Lait, puis du Sucre
        Boisson cafeComplexe = new Sucre(new Lait(new CremeFouettee(new CafeSimple())));
        System.out.println("Boisson: " + cafeComplexe.getDescription() + " | Coût: " + String.format("%.2f", cafeComplexe.getCout()) + "€");

        // Exemple d'une autre boisson de base (pour montrer la flexibilité)
        // Supposons une classe TheSimple (coût 1.50, description "Thé simple")
        // Boisson theAvecLait = new Lait(new TheSimple());
        // System.out.println("Boisson: " + theAvecLait.getDescription() + " | Coût: " + String.format("%.2f", theAvecLait.getCout()) + "€");
    }
}
```



### Sortie console attendue



```
Boisson: Café simple | Coût: 2,00€
Boisson: Café simple, avec Lait | Coût: 2,50€
Boisson: Café simple, avec Lait, avec Sucre | Coût: 2,70€
Boisson: Café simple, avec Crème Fouettée, avec Lait, avec Sucre | Coût: 3,45€
```



## Justification du Pattern Decorator

Le pattern Decorator est parfaitement adapté à ce problème de cafeteria personnalisée pour les raisons suivantes :

*   **Extension dynamique des fonctionnalités :** Il permet d'ajouter des responsabilités (coût et description d'un ingrédient) à un objet (`Boisson`) de manière dynamique et transparente. On peut empiler les décorateurs pour créer des combinaisons infinies.
*   **Alternative à l'héritage :** Sans le Decorator, pour gérer toutes les combinaisons possibles (Café simple, Café au lait, Café sucré, Café au lait sucré, Café crème fouettée, etc.), il faudrait créer une explosion de sous-classes via l'héritage. Par exemple, `CafeAuLaitSucre` hériterait de `CafeAuLait` qui hériterait de `CafeSimple`. Cela mènerait à une hiérarchie de classes rigide, difficile à maintenir et à étendre (le "class explosion problem").
*   **Respect du Principe Ouvert/Fermé (Open/Closed Principle - OCP) :** Le code des classes `Boisson` et `CafeSimple` est "fermé" à la modification (on ne le touche pas pour ajouter de nouveaux ingrédients), mais "ouvert" à l'extension (on ajoute de nouveaux décorateurs pour de nouveaux ingrédients).
*   **Flexibilité :** L'ordre des décorateurs peut parfois avoir une signification (bien que dans cet exemple de coût, l'ordre n'importe pas pour le total, mais il pourrait pour d'autres logiques).
*   **Découplage :** Le client (`Main`) manipule toujours l'interface `Boisson`, sans se soucier de savoir s'il s'agit d'une boisson de base ou d'une boisson décorée.

## Pour aller plus loin (Optionnel)

### Ajouter un nouveau décorateur : `DoubleDoseExpresso`

Pour cela, nous allons d'abord ajouter un attribut `force` à l'interface `Boisson`.

**Interface `Boisson` (mise à jour)**



```java
// Fichier: Boisson.java (mise à jour)
public interface Boisson {
    double getCout();
    String getDescription();
    int getForce(); // Nouvelle méthode
}
```



**`CafeSimple` (mise à jour)**



```java
// Fichier: CafeSimple.java (mise à jour)
public class CafeSimple implements Boisson {
    @Override
    public double getCout() {
        return 2.00;
    }

    @Override
    public String getDescription() {
        return "Café simple";
    }

    @Override
    public int getForce() {
        return 1; // Force de base pour un café simple
    }
}
```



**`DecorateurBoisson` (mise à jour)**



```java
// Fichier: DecorateurBoisson.java (mise à jour)
public abstract class DecorateurBoisson implements Boisson {
    protected Boisson boissonDecorée;

    public DecorateurBoisson(Boisson boisson) {
        this.boissonDecorée = boisson;
    }

    @Override
    public double getCout() {
        return boissonDecorée.getCout();
    }

    @Override
    public String getDescription() {
        return boissonDecorée.getDescription();
    }

    @Override
    public int getForce() {
        return boissonDecorée.getForce(); // Délègue par défaut
    }
}
```



**Nouveau décorateur `DoubleDoseExpresso`**



```java
// Fichier: DoubleDoseExpresso.java
public class DoubleDoseExpresso extends DecorateurBoisson {
    public DoubleDoseExpresso(Boisson boisson) {
        super(boisson);
    }

    @Override
    public double getCout() {
        return super.getCout() + 1.00; // Coût supplémentaire pour la double dose
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", double expresso";
    }

    @Override
    public int getForce() {
        return super.getForce() + 1; // Augmente la force
    }
}
```



**Test dans `Main`**



```java
// Fichier: Main.java (mise à jour avec DoubleDoseExpresso)
// ... (code précédent)

        System.out.println("\n--- Test avec DoubleDoseExpresso ---");
        Boisson cafeDouble = new DoubleDoseExpresso(new CafeSimple());
        System.out.println("Boisson: " + cafeDouble.getDescription() + " | Coût: " + String.format("%.2f", cafeDouble.getCout()) + "€ | Force: " + cafeDouble.getForce());

        Boisson cafeDoubleLaitSucre = new Sucre(new Lait(new DoubleDoseExpresso(new CafeSimple())));
        System.out.println("Boisson: " + cafeDoubleLaitSucre.getDescription() + " | Coût: " + String.format("%.2f", cafeDoubleLaitSucre.getCout()) + "€ | Force: " + cafeDoubleLaitSucre.getForce());
```



### Gestion des quantités ("deux sucres")

*   **Approche 1 : Créer un décorateur `DoubleSucre` :**
    *   **Avantages :** Simple à implémenter si les quantités sont fixes et peu nombreuses.
    *   **Inconvénients :** Explosion de classes si on veut gérer "trois sucres", "quatre sucres", etc. Ne gère pas les quantités arbitraires.
*   **Approche 2 : Modifier le décorateur `Sucre` pour qu'il prenne une quantité en paramètre :**
    *   **Avantages :** Très flexible, gère n'importe quelle quantité. Moins de classes.
    *   **Inconvénients :** Le décorateur `Sucre` devient plus complexe (il doit stocker la quantité et l'utiliser dans ses calculs). Le client doit spécifier la quantité à chaque fois.

Pour une flexibilité maximale, l'approche 2 est généralement préférable. Le décorateur `Sucre` pourrait alors ressembler à ceci :



```java
// Fichier: Sucre.java (version avec quantité)
public class Sucre extends DecorateurBoisson {
    private int quantite;

    public Sucre(Boisson boisson, int quantite) {
        super(boisson);
        this.quantite = quantite;
    }

    @Override
    public double getCout() {
        return super.getCout() + (0.20 * quantite);
    }

    @Override
    public String getDescription() {
        return super.getDescription() + ", avec " + quantite + " Sucre(s)";
    }

    @Override
    public int getForce() {
        return super.getForce(); // Le sucre n'affecte pas la force
    }
}
```



### Validation (empêcher double ajout d'ingrédient)

Le pattern Decorator par nature permet d'empiler les décorateurs, même si cela n'a pas toujours de sens métier. Pour empêcher des ajouts "illogiques" (ex: deux fois `Lait` si un seul est suffisant), il faudrait ajouter une logique de validation *en dehors* des décorateurs eux-mêmes, généralement au niveau du code client ou d'un "constructeur" de boisson plus intelligent.

Une approche pourrait être :
1.  **Utiliser un `BoissonBuilder` :** Un Builder pourrait être utilisé pour construire la boisson. Le Builder aurait une logique interne pour vérifier si un ingrédient a déjà été ajouté et refuser ou ignorer l'ajout.
2.  **Vérification au moment de l'ajout :** Dans le code client, avant d'appliquer un décorateur, on pourrait vérifier la description ou un attribut spécifique de la boisson décorée pour voir si l'ingrédient est déjà présent.

Exemple simplifié de vérification dans le client :



```java
// Dans Main.java
Boisson maBoisson = new CafeSimple();
if (!maBoisson.getDescription().contains("Lait")) {
    maBoisson = new Lait(maBoisson);
} else {
    System.out.println("Le lait est déjà ajouté !");
}
```


Cependant, cette logique de validation peut devenir complexe si elle est dispersée. Un `BoissonBuilder` serait une solution plus robuste pour gérer les règles de composition.