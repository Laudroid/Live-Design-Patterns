Voici une proposition de solution pour le TP sur le Design Pattern Composite.

---

# Solution du TP : Implémentation du Pattern Composite pour la Gestion de Menus Interactifs

## Étape 1 : Le Composant Abstraite (`MenuComponent`)

Cette classe abstraite définit l'interface commune pour les éléments atomiques (`MenuItem`) et les éléments composites (`Menu`).




```java
// Fichier: MenuComponent.java
public abstract class MenuComponent {
    protected String name;

    public MenuComponent(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    // Méthodes pour les opérations de l'arbre (par défaut, non supportées pour les feuilles)
    public void add(MenuComponent component) {
        throw new UnsupportedOperationException("Opération 'add' non supportée pour cet élément.");
    }

    public void remove(MenuComponent component) {
        throw new UnsupportedOperationException("Opération 'remove' non supportée pour cet élément.");
    }

    public MenuComponent getChild(int i) {
        throw new UnsupportedOperationException("Opération 'getChild' non supportée pour cet élément.");
    }

    // Méthode d'affichage commune
    public abstract void display(String indent); // Ajout d'un paramètre d'indentation pour un affichage hiérarchique
}
```



## Étape 2 : L'Élément Feuille (`MenuItem`)

`MenuItem` représente un élément de menu qui ne peut pas contenir d'autres éléments. Il exécute une action.




```java
// Fichier: MenuItem.java
public class MenuItem extends MenuComponent {
    private Runnable action; // L'action à exécuter

    public MenuItem(String name, Runnable action) {
        super(name);
        this.action = action;
    }

    @Override
    public void display(String indent) {
        System.out.println(indent + "- " + getName());
    }

    public void executeAction() {
        System.out.println("Exécution de l'action pour: " + getName());
        action.run();
    }
}
```



## Étape 3 : Le Composite (`Menu`)

`Menu` représente un menu qui peut contenir d'autres `MenuComponent` (des `MenuItem` ou d'autres `Menu`).




```java
// Fichier: Menu.java
import java.util.ArrayList;
import java.util.List;

public class Menu extends MenuComponent {
    private List<MenuComponent> children;

    public Menu(String name) {
        super(name);
        this.children = new ArrayList<>();
    }

    @Override
    public void add(MenuComponent component) {
        children.add(component);
    }

    @Override
    public void remove(MenuComponent component) {
        children.remove(component);
    }

    @Override
    public MenuComponent getChild(int i) {
        return children.get(i);
    }

    @Override
    public void display(String indent) {
        System.out.println(indent + "+ " + getName() + ":");
        // Itère sur les enfants et les affiche avec une indentation supplémentaire
        for (MenuComponent component : children) {
            component.display(indent + "  ");
        }
    }
}
```



## Étape 4 : Construction et Test de la Structure

La classe `Main` assemble les composants pour créer une structure de menu et interagit avec elle.




```java
// Fichier: Main.java
public class Main {
    public static void main(String[] args) {
        // Création des éléments de menu (feuilles)
        MenuItem openItem = new MenuItem("Ouvrir", () -> System.out.println("Ouverture d'un fichier..."));
        MenuItem saveItem = new MenuItem("Enregistrer", () -> System.out.println("Enregistrement du fichier..."));
        MenuItem exitItem = new MenuItem("Quitter", () -> System.out.println("Fermeture de l'application..."));

        MenuItem copyItem = new MenuItem("Copier", () -> System.out.println("Copie de la sélection..."));
        MenuItem pasteItem = new MenuItem("Coller", () -> System.out.println("Collage du contenu..."));

        MenuItem aboutItem = new MenuItem("À propos", () -> System.out.println("Application de gestion de menus v1.0"));

        // Création des sous-menus
        Menu newMenu = new Menu("Nouveau");
        newMenu.add(new MenuItem("Nouveau Document", () -> System.out.println("Création d'un nouveau document...")));
        newMenu.add(new MenuItem("Nouvelle Fenêtre", () -> System.out.println("Création d'une nouvelle fenêtre...")));

        // Création des menus principaux (composites)
        Menu fileMenu = new Menu("Fichier");
        fileMenu.add(newMenu); // Ajout d'un sous-menu
        fileMenu.add(openItem);
        fileMenu.add(saveItem);
        fileMenu.add(exitItem);

        Menu editMenu = new Menu("Édition");
        editMenu.add(copyItem);
        editMenu.add(pasteItem);

        Menu helpMenu = new Menu("Aide");
        helpMenu.add(aboutItem);

        // Création du menu principal (racine de l'arbre)
        Menu mainMenu = new Menu("Menu Principal");
        mainMenu.add(fileMenu);
        mainMenu.add(editMenu);
        mainMenu.add(helpMenu);

        // 3. Affichage de toute la structure du menu
        System.out.println("--- Affichage du Menu Complet ---");
        mainMenu.display(""); // Appel sur la racine, sans indentation initiale

        // 4. Interaction : Simuler la sélection et l'exécution d'actions
        System.out.println("\n--- Simulation d'interactions ---");
        // Accéder à un MenuItem via la structure composite
        Menu fichier = (Menu) mainMenu.getChild(0); // Menu "Fichier"
        MenuItem ouvrir = (MenuItem) fichier.getChild(1); // "Ouvrir"
        ouvrir.executeAction();

        // Accéder à un MenuItem dans un sous-menu
        Menu nouveau = (Menu) fichier.getChild(0); // Sous-menu "Nouveau"
        MenuItem nouveauDoc = (MenuItem) nouveau.getChild(0); // "Nouveau Document"
        nouveauDoc.executeAction();

        // Exécuter une action directement sur un élément
        aboutItem.executeAction();
    }
}
```



### Sortie console du programme



```
--- Affichage du Menu Complet ---
+ Menu Principal:
  + Fichier:
    + Nouveau:
      - Nouveau Document
      - Nouvelle Fenêtre
    - Ouvrir
    - Enregistrer
    - Quitter
  + Édition:
    - Copier
    - Coller
  + Aide:
    - À propos

--- Simulation d'interactions ---
Exécution de l'action pour: Ouvrir
Ouverture d'un fichier...
Exécution de l'action pour: Nouveau Document
Création d'un nouveau document...
Exécution de l'action pour: À propos
Application de gestion de menus v1.0
```



## Explications et Justifications

Le pattern Composite est idéal pour ce problème de gestion de menus car il permet de traiter des objets individuels (`MenuItem`) et des compositions d'objets (`Menu`) de manière uniforme.

*   **`MenuComponent` (Composant) :** C'est l'interface commune qui définit les opérations que tous les éléments de la hiérarchie peuvent effectuer (`getName`, `display`). Elle déclare également les opérations de gestion des enfants (`add`, `remove`, `getChild`), mais les implémente par défaut en levant une `UnsupportedOperationException`. Cela garantit que les clients peuvent appeler ces méthodes sur n'importe quel `MenuComponent` sans se soucier de savoir s'il s'agit d'une feuille ou d'un composite, tout en empêchant les feuilles d'avoir des enfants.
*   **`MenuItem` (Feuille) :** Représente les éléments de menu individuels qui ne peuvent pas avoir d'enfants. Il implémente les opérations communes et fournit une méthode spécifique (`executeAction`) pour son comportement propre. Il ne supporte pas les opérations de gestion des enfants.
*   **`Menu` (Composite) :** Représente un menu qui peut contenir d'autres `MenuComponent`. Il implémente les opérations communes et les opérations de gestion des enfants en les déléguant à sa liste interne d'enfants. Sa méthode `display` est récursive, ce qui permet d'afficher toute la structure de l'arbre en appelant `display` sur la racine.

**Avantages du Composite dans ce scénario :**

*   **Uniformité :** Le code client peut traiter un `MenuItem` et un `Menu` de la même manière via l'interface `MenuComponent`. Il n'a pas besoin de `if/else` pour distinguer les types.
*   **Simplicité du client :** Le client n'a pas besoin de connaître la complexité de la structure de l'arbre. Il interagit avec la racine et les opérations se propagent automatiquement.
*   **Extensibilité :** L'ajout de nouveaux types d'éléments de menu (ex: un `SeparatorItem` ou un `CheckboxMenuItem`) est simple. Il suffit de créer une nouvelle classe feuille qui hérite de `MenuComponent`.
*   **Flexibilité :** La structure des menus peut être modifiée dynamiquement (ajouter/supprimer des éléments) à l'exécution.

## Pour aller plus loin (Optionnel)

### Recherche d'éléments

On pourrait ajouter une méthode `findComponent(String name)` à `MenuComponent` qui serait implémentée différemment par `MenuItem` et `Menu`.

**`MenuComponent` (mise à jour)**




```java
// Fichier: MenuComponent.java (mise à jour)
public abstract class MenuComponent {
    // ... (méthodes existantes)

    public abstract MenuComponent findComponent(String targetName);
}
```




**`MenuItem` (mise à jour)**




```java
// Fichier: MenuItem.java (mise à jour)
public class MenuItem extends MenuComponent {
    // ... (méthodes existantes)

    @Override
    public MenuComponent findComponent(String targetName) {
        return this.name.equals(targetName) ? this : null;
    }
}
```




**`Menu` (mise à jour)**




```java
// Fichier: Menu.java (mise à jour)
public class Menu extends MenuComponent {
    // ... (méthodes existantes)

    @Override
    public MenuComponent findComponent(String targetName) {
        if (this.name.equals(targetName)) {
            return this;
        }
        for (MenuComponent component : children) {
            MenuComponent found = component.findComponent(targetName);
            if (found != null) {
                return found;
            }
        }
        return null;
    }
}
```




**Test dans `Main`**




```java
// Fichier: Main.java (mise à jour)
// ... (code précédent)

        System.out.println("\n--- Recherche d'éléments ---");
        MenuComponent foundItem = mainMenu.findComponent("Enregistrer");
        if (foundItem != null) {
            System.out.println("Élément trouvé: " + foundItem.getName());
            if (foundItem instanceof MenuItem) {
                ((MenuItem) foundItem).executeAction();
            }
        }

        MenuComponent foundMenu = mainMenu.findComponent("Nouveau");
        if (foundMenu != null) {
            System.out.println("Menu trouvé: " + foundMenu.getName());
            foundMenu.display("  ");
        }
```



### Gestion des raccourcis clavier

Cela impliquerait d'ajouter un attribut `shortcut` à `MenuItem` et une méthode `activateByShortcut(String shortcut)` qui pourrait être implémentée dans `MenuComponent` et surchargée par `MenuItem` pour exécuter l'action si le raccourci correspond.

### Persistance

Pour la persistance, on pourrait utiliser des bibliothèques de sérialisation (comme Jackson pour JSON ou JAXB pour XML en Java). La structure récursive du Composite se prête bien à la sérialisation, car chaque `Menu` peut sérialiser ses enfants, et chaque `MenuItem` ses propres attributs. Il faudrait gérer la sérialisation de l'objet `Runnable` pour l'action, ce qui est plus complexe et nécessiterait des approches spécifiques (ex: sérialiser le nom de la méthode ou une chaîne de commande qui serait ensuite exécutée).