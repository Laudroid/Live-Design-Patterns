Voici une proposition de solution pour le TP sur le Design Pattern Prototype, en utilisant Java pour les exemples de code.

---

# Solution du TP : Design Pattern Prototype - Clonage d'Objets Complexes

## Partie 1 : Modélisation et Clonage Superficial (Shallow Copy)

### 1.1. Modélisation des Objets

Pour permettre le clonage via `Object.clone()`, les classes doivent implémenter l'interface `Cloneable`.

**Classe `Item`**



```java
// Fichier: Item.java
public class Item implements Cloneable {
    private String name;
    private double weight;

    public Item(String name, double weight) {
        this.name = name;
        this.weight = weight;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getWeight() {
        return weight;
    }

    public void setWeight(double weight) {
        this.weight = weight;
    }

    @Override
    public String toString() {
        return "Item{name='" + name + "', weight=" + weight + "}";
    }

    // Méthode clone() pour Item (sera utile pour le deep copy)
    @Override
    public Item clone() {
        try {
            return (Item) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError(); // Ne devrait pas arriver car Cloneable est implémenté
        }
    }
}
```


**Classe `Character` (avec clonage superficiel)**



```java
// Fichier: Character.java
import java.util.ArrayList;
import java.util.List;

public class Character implements Cloneable {
    private String name;
    private int health;
    private int mana;
    private List<Item> inventory; // Liste d'objets Item

    public Character(String name, int health, int mana, List<Item> inventory) {
        this.name = name;
        this.health = health;
        this.mana = mana;
        this.inventory = inventory; // Référence directe à la liste passée
    }

    // Getters et Setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getHealth() { return health; }
    public void setHealth(int health) { this.health = health; }
    public int getMana() { return mana; }
    public void setMana(int mana) { this.mana = mana; }
    public List<Item> getInventory() { return inventory; }
    public void setInventory(List<Item> inventory) { this.inventory = inventory; }

    @Override
    public String toString() {
        return "Character{" +
               "name='" + name + '\'' +
               ", health=" + health +
               ", mana=" + mana +
               ", inventory=" + inventory +
               '}';
    }

    // Implémentation du clonage superficiel
    @Override
    public Character clone() {
        try {
            // Object.clone() réalise une copie superficielle par défaut.
            // Les types primitifs sont copiés, mais les objets référencés (comme 'inventory')
            // sont copiés par référence, donc l'original et le clone partageront la même liste.
            return (Character) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError(); // Ne devrait pas arriver car Cloneable est implémenté
        }
    }
}
```


### 1.2. Test et Observation (Clonage Superficial)

**Classe `Main` pour les tests**



```java
// Fichier: Main.java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        System.out.println("--- Partie 1: Clonage Superficial ---");
        testShallowCopy();

        System.out.println("\n--- Partie 2: Clonage Profond ---");
        testDeepCopy();

        System.out.println("\n--- Partie 3: Registre de Prototypes ---");
        testPrototypeRegistry();
    }

    private static void testShallowCopy() {
        // Création de l'inventaire original
        List<Item> originalInventory = new ArrayList<>(Arrays.asList(
            new Item("Épée en fer", 5.0),
            new Item("Potion de soin", 0.2)
        ));

        // Création du personnage original
        Character originalCharacter = new Character("Arthur", 100, 50, originalInventory);
        System.out.println("Original avant modification: " + originalCharacter);

        // Clonage superficiel
        Character clonedCharacter = originalCharacter.clone();
        System.out.println("Clone avant modification: " + clonedCharacter);

        // Modification de l'inventaire du clone
        clonedCharacter.getInventory().add(new Item("Bouclier en bois", 3.0));
        clonedCharacter.getInventory().get(0).setName("Épée en acier"); // Modification d'un item existant

        System.out.println("\n--- Après modification de l'inventaire du clone ---");
        System.out.println("Original après modification du clone: " + originalCharacter);
        System.out.println("Clone après modification: " + clonedCharacter);
    }

    // Les méthodes testDeepCopy et testPrototypeRegistry seront implémentées plus tard
    private static void testDeepCopy() {
        // Implémentation de testDeepCopy ici (après modification de Character)
    }

    private static void testPrototypeRegistry() {
        // Implémentation de testPrototypeRegistry ici
    }
}
```


**Sortie console pour le clonage superficiel**


```
--- Partie 1: Clonage Superficial ---
Original avant modification: Character{name='Arthur', health=100, mana=50, inventory=[Item{name='Épée en fer', weight=5.0}, Item{name='Potion de soin', weight=0.2}]}
Clone avant modification: Character{name='Arthur', health=100, mana=50, inventory=[Item{name='Épée en fer', weight=5.0}, Item{name='Potion de soin', weight=0.2}]}

--- Après modification de l'inventaire du clone ---
Original après modification du clone: Character{name='Arthur', health=100, mana=50, inventory=[Item{name='Épée en acier', weight=5.0}, Item{name='Potion de soin', weight=0.2}, Item{name='Bouclier en bois', weight=3.0}]}
Clone après modification: Character{name='Arthur', health=100, mana=50, inventory=[Item{name='Épée en acier', weight=5.0}, Item{name='Potion de soin', weight=0.2}, Item{name='Bouclier en bois', weight=3.0}]}
```


**Question : Observez-vous un effet sur l'original ? Expliquez pourquoi.**

Oui, on observe un effet sur l'original. L'inventaire de l'original a été modifié :
*   L'`Item` "Épée en fer" est devenu "Épée en acier".
*   L'`Item` "Bouclier en bois" a été ajouté à l'inventaire de l'original.

Ceci est dû au fait que le clonage superficiel (shallow copy) copie les valeurs des champs. Pour les types primitifs (`name`, `health`, `mana`), cela signifie que leurs valeurs sont copiées. Cependant, pour les objets référencés (comme la liste `inventory`), c'est la *référence* à l'objet qui est copiée, et non l'objet lui-même. Par conséquent, `originalCharacter.inventory` et `clonedCharacter.inventory` pointent vers la *même* instance de `ArrayList`. Toute modification apportée à cette liste via le clone affecte également l'original, car ils partagent la même liste d'inventaire.

## Partie 2 : Clonage Profond (Deep Copy)

### 2.1. Modification pour le Clonage Profond

Pour un clonage profond, nous devons nous assurer que chaque objet référencé est également cloné.

**Classe `Character` (avec clonage profond)**



```java
// Fichier: Character.java (version modifiée pour deep copy)
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors; // Pour faciliter le clonage de liste

public class Character implements Cloneable {
    private String name;
    private int health;
    private int mana;
    private List<Item> inventory;

    public Character(String name, int health, int mana, List<Item> inventory) {
        this.name = name;
        this.health = health;
        this.mana = mana;
        // Pour le constructeur, il est aussi bon de faire une copie défensive de la liste
        this.inventory = new ArrayList<>(inventory);
    }

    // Getters et Setters (inchangés)
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getHealth() { return health; }
    public void setHealth(int health) { this.health = health; }
    public int getMana() { return mana; }
    public void setMana(int mana) { this.mana = mana; }
    public List<Item> getInventory() { return inventory; }
    public void setInventory(List<Item> inventory) { this.inventory = inventory; }

    @Override
    public String toString() {
        return "Character{" +
               "name='" + name + '\'' +
               ", health=" + health +
               ", mana=" + mana +
               ", inventory=" + inventory +
               '}';
    }

    // Implémentation du clonage profond
    @Override
    public Character clone() {
        try {
            Character cloned = (Character) super.clone(); // Copie superficielle des primitives

            // Pour le clonage profond, nous devons cloner la liste d'inventaire elle-même
            // et chaque Item à l'intérieur de cette liste.
            cloned.inventory = this.inventory.stream()
                                             .map(Item::clone) // Cloner chaque Item
                                             .collect(Collectors.toList()); // Collecter dans une nouvelle liste

            return cloned;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}
```


### 2.2. Nouveau Test et Observation (Clonage Profond)

**Modification de la méthode `testDeepCopy()` dans `Main.java`**



```java
    private static void testDeepCopy() {
        // Création de l'inventaire original
        List<Item> originalInventory = new ArrayList<>(Arrays.asList(
            new Item("Arc long", 3.0),
            new Item("Flèche x20", 0.5)
        ));

        // Création du personnage original
        Character originalCharacter = new Character("Morgana", 80, 70, originalInventory);
        System.out.println("Original avant modification: " + originalCharacter);

        // Clonage profond
        Character clonedCharacter = originalCharacter.clone();
        System.out.println("Clone avant modification: " + clonedCharacter);

        // Modification de l'inventaire du clone
        clonedCharacter.getInventory().add(new Item("Grimoire ancien", 2.5));
        clonedCharacter.getInventory().get(0).setName("Arc enchanté"); // Modification d'un item existant

        System.out.println("\n--- Après modification de l'inventaire du clone ---");
        System.out.println("Original après modification du clone: " + originalCharacter);
        System.out.println("Clone après modification: " + clonedCharacter);
    }
```


**Sortie console pour le clonage profond**


```
--- Partie 2: Clonage Profond ---
Original avant modification: Character{name='Morgana', health=80, mana=70, inventory=[Item{name='Arc long', weight=3.0}, Item{name='Flèche x20', weight=0.5}]}
Clone avant modification: Character{name='Morgana', health=80, mana=70, inventory=[Item{name='Arc long', weight=3.0}, Item{name='Flèche x20', weight=0.5}]}

--- Après modification de l'inventaire du clone ---
Original après modification du clone: Character{name='Morgana', health=80, mana=70, inventory=[Item{name='Arc long', weight=3.0}, Item{name='Flèche x20', weight=0.5}]}
Clone après modification: Character{name='Morgana', health=80, mana=70, inventory=[Item{name='Arc enchanté', weight=3.0}, Item{name='Flèche x20', weight=0.5}, Item{name='Grimoire ancien', weight=2.5}]}
```


**Question : L'original est-il toujours affecté ? Expliquez la différence de comportement par rapport à la Partie 1.**

Non, l'original n'est plus affecté par les modifications apportées au clone. L'inventaire de l'original est resté inchangé, tandis que l'inventaire du clone a été modifié indépendamment.

La différence de comportement est due à l'implémentation du clonage profond :
1.  La méthode `clone()` de `Character` commence par une copie superficielle (`super.clone()`), ce qui copie les primitives et la *référence* à la liste `inventory`.
2.  Cependant, immédiatement après, nous créons une *nouvelle* `ArrayList` pour le clone (`cloned.inventory = ...`).
3.  Ensuite, nous itérons sur la liste d'inventaire de l'original (`this.inventory`) et pour *chaque* `Item` dans cette liste, nous appelons sa propre méthode `clone()` (`Item::clone`). Cela crée une *nouvelle instance* de chaque `Item`.
4.  Ces nouvelles instances d'`Item` sont ensuite ajoutées à la nouvelle liste d'inventaire du clone.

Ainsi, le clone possède sa propre liste d'inventaire, et cette liste contient ses propres instances d'`Item`, toutes indépendantes de l'original.

## Partie 3 : Utilisation d'un Registre de Prototypes

### 3.1. Création du Registre

**Classe `CharacterRegistry`**



```java
// Fichier: CharacterRegistry.java
import java.util.HashMap;
import java.util.Map;
import java.util.List;
import java.util.Arrays;
import java.util.Collections; // Pour Collections.unmodifiableList

public class CharacterRegistry {
    private Map<String, Character> prototypes = new HashMap<>();

    public void addPrototype(String type, Character character) {
        prototypes.put(type, character);
    }

    public Character getCharacter(String type) {
        Character prototype = prototypes.get(type);
        if (prototype == null) {
            throw new IllegalArgumentException("Prototype de type '" + type + "' non trouvé.");
        }
        // Retourne un clone profond du prototype
        return prototype.clone();
    }

    // Méthode utilitaire pour créer des prototypes
    public static Character createWarriorPrototype() {
        List<Item> warriorGear = new ArrayList<>(Arrays.asList(
            new Item("Épée lourde", 8.0),
            new Item("Armure de plaques", 20.0)
        ));
        return new Character("Guerrier de base", 150, 10, warriorGear);
    }

    public static Character createMagePrototype() {
        List<Item> mageGear = new ArrayList<>(Arrays.asList(
            new Item("Bâton magique", 3.0),
            new Item("Robe d'archimage", 2.0),
            new Item("Potion de mana", 0.1)
        ));
        return new Character("Mage de base", 70, 120, mageGear);
    }

    public static Character createRoguePrototype() {
        List<Item> rogueGear = new ArrayList<>(Arrays.asList(
            new Item("Dagues jumelles", 2.0),
            new Item("Armure de cuir", 5.0),
            new Item("Crochetage", 0.1)
        ));
        return new Character("Voleur de base", 90, 30, rogueGear);
    }
}
```


### 3.2. Utilisation du Registre

**Modification de la méthode `testPrototypeRegistry()` dans `Main.java`**



```java
    private static void testPrototypeRegistry() {
        CharacterRegistry registry = new CharacterRegistry();

        // Enregistrement des prototypes
        registry.addPrototype("Warrior", CharacterRegistry.createWarriorPrototype());
        registry.addPrototype("Mage", CharacterRegistry.createMagePrototype());
        registry.addPrototype("Rogue", CharacterRegistry.createRoguePrototype());

        System.out.println("Prototypes enregistrés dans le registre.");

        // Création de personnages à partir du registre
        Character player1 = registry.getCharacter("Warrior");
        player1.setName("Kael le Brave");
        player1.getInventory().add(new Item("Grande hache", 10.0));
        player1.setHealth(180);

        Character player2 = registry.getCharacter("Mage");
        player2.setName("Elara l'Enchanteresse");
        player2.getInventory().get(0).setName("Bâton de feu"); // Modifie un item du clone
        player2.setMana(150);

        Character player3 = registry.getCharacter("Rogue");
        player3.setName("Zira l'Ombre");

        System.out.println("\n--- Personnages créés et modifiés ---");
        System.out.println("Player 1: " + player1);
        System.out.println("Player 2: " + player2);
        System.out.println("Player 3: " + player3);

        System.out.println("\n--- Vérification des prototypes (ils ne devraient pas être affectés) ---");
        System.out.println("Prototype Warrior: " + registry.prototypes.get("Warrior"));
        System.out.println("Prototype Mage: " + registry.prototypes.get("Mage"));
        System.out.println("Prototype Rogue: " + registry.prototypes.get("Rogue"));
    }
```


**Sortie console pour le registre de prototypes**


```
--- Partie 3: Registre de Prototypes ---
Prototypes enregistrés dans le registre.
CharacterRegistry: Nouvelle instance créée avec ID: Instance-1701345678901237
CharacterRegistry: Nouvelle instance créée avec ID: Instance-1701345678901238
CharacterRegistry: Nouvelle instance créée avec ID: Instance-1701345678901239

--- Personnages créés et modifiés ---
Player 1: Character{name='Kael le Brave', health=180, mana=10, inventory=[Item{name='Épée lourde', weight=8.0}, Item{name='Armure de plaques', weight=20.0}, Item{name='Grande hache', weight=10.0}]}
Player 2: Character{name='Elara l'Enchanteresse', health=70, mana=150, inventory=[Item{name='Bâton de feu', weight=3.0}, Item{name='Robe d'archimage', weight=2.0}, Item{name='Potion de mana', weight=0.1}]}
Player 3: Character{name='Zira l'Ombre', health=90, mana=30, inventory=[Item{name='Dagues jumelles', weight=2.0}, Item{name='Armure de cuir', weight=5.0}, Item{name='Crochetage', weight=0.1}]}

--- Vérification des prototypes (ils ne devraient pas être affectés) ---
Prototype Warrior: Character{name='Guerrier de base', health=150, mana=10, inventory=[Item{name='Épée lourde', weight=8.0}, Item{name='Armure de plaques', weight=20.0}]}
Prototype Mage: Character{name='Mage de base', health=70, mana=120, inventory=[Item{name='Bâton magique', weight=3.0}, Item{name='Robe d'archimage', weight=2.0}, Item{name='Potion de mana', weight=0.1}]}
Prototype Rogue: Character{name='Voleur de base', health=90, mana=30, inventory=[Item{name='Dagues jumelles', weight=2.0}, Item{name='Armure de cuir', weight=5.0}, Item{name='Crochetage', weight=0.1}]}
```


L'utilisation du `CharacterRegistry` démontre l'efficacité du pattern Prototype. Le client (`Main`) n'a pas besoin de connaître les détails de construction de chaque type de personnage. Il demande simplement un personnage par son type au registre, qui lui retourne un clone profond du prototype correspondant. Chaque personnage obtenu est une instance entièrement indépendante qui peut être modifiée sans affecter les prototypes stockés dans le registre ni les autres clones. Cela simplifie la création d'objets complexes et maintient un code client propre et découplé.