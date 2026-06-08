Absolument ! Voici une proposition de sujet de TP pour illustrer le Design Pattern Prototype, en tenant compte de vos directives.

---

## TP : Design Pattern Prototype - Clonage d'Objets Complexes

### Objectif du TP

Maîtriser l'utilisation du Design Pattern Prototype pour créer de nouveaux objets à partir d'instances existantes, en garantissant l'indépendance des états entre l'original et ses clones.

### Contexte

Vous développez un système où la création d'objets similaires, mais avec des états initiaux légèrement différents, est fréquente. Par exemple, dans un jeu vidéo, vous pourriez avoir un "personnage de base" et vouloir en créer de multiples variantes (un guerrier, un mage, un archer) sans avoir à réinitialiser toutes les propriétés à chaque fois.

Le pattern Prototype offre une solution élégante en permettant de "cloner" des objets existants (les prototypes) plutôt que de les instancier directement via leurs constructeurs. Cela simplifie la création d'objets complexes et découple le code client des classes concrètes.

L'enjeu principal de ce TP sera de s'assurer que le clonage crée une copie *indépendante* de l'original, surtout lorsque l'objet contient des références à d'autres objets (on parle alors de clonage superficiel vs. clonage profond).

### Énoncé du TP

Vous allez modéliser des personnages de jeu et leur inventaire pour explorer le clonage.

#### Partie 1 : Modélisation et Clonage Superficial (Shallow Copy)

1.  **Modélisation des Objets :**
    *   Créez une classe `Item` avec les propriétés suivantes :
        *   `name: string` (ex: "Épée en fer", "Potion de soin")
        *   `weight: number` (ex: 5, 0.2)
    *   Créez une classe `Character` avec les propriétés suivantes :
        *   `name: string` (ex: "Arthur", "Morgana")
        *   `health: number` (points de vie)
        *   `mana: number` (points de magie)
        *   `inventory: List<Item>` (une liste d'objets `Item`)

2.  **Implémentation du Clonage Superficial :**
    *   Dans la classe `Character`, implémentez une méthode `clone()` qui réalise un clonage *superficiel* (shallow copy). Cela signifie que les types primitifs (`name`, `health`, `mana`) seront copiés, mais les objets référencés (`inventory`) seront partagés entre l'original et le clone.

3.  **Test et Observation :**
    *   Créez une instance de `Character` (l'original) avec un nom, une santé, un mana et un inventaire contenant quelques `Item`.
    *   Appelez la méthode `clone()` pour créer un nouveau personnage (le clone).
    *   Modifiez l'inventaire du *clone* (ajoutez un nouvel `Item`, retirez-en un, ou modifiez un `Item` existant dans la liste).
    *   Affichez l'état complet de l'original et du clone *après* la modification du clone.
    *   **Question :** Observez-vous un effet sur l'original ? Expliquez pourquoi.

#### Partie 2 : Clonage Profond (Deep Copy)

1.  **Modification pour le Clonage Profond :**
    *   Modifiez la méthode `clone()` de la classe `Character` pour qu'elle réalise un clonage *profond* (deep copy).
    *   Cela implique que non seulement les propriétés primitives sont copiées, mais que tous les objets référencés (comme l'`inventory` et chaque `Item` à l'intérieur de l'inventaire) sont également clonés, créant ainsi des instances entièrement nouvelles et indépendantes.
    *   *Conseil :* Vous devrez probablement implémenter une méthode `clone()` également dans la classe `Item` si ce n'est pas déjà fait.

2.  **Nouveau Test et Observation :**
    *   Répétez l'expérience de la Partie 1 : créez un `Character` original, clonez-le, puis modifiez l'inventaire du clone.
    *   Affichez l'état complet de l'original et du clone *après* la modification du clone.
    *   **Question :** L'original est-il toujours affecté ? Expliquez la différence de comportement par rapport à la Partie 1.

#### Partie 3 : Utilisation d'un Registre de Prototypes (Optionnel mais recommandé)

Pour aller plus loin et illustrer une utilisation plus complète du pattern :

1.  **Création du Registre :**
    *   Implémentez une classe `CharacterRegistry` (ou `PrototypeRegistry`) qui permet d'enregistrer différents prototypes de personnages (ex: "Warrior", "Mage", "Rogue").
    *   Cette classe devrait avoir des méthodes pour :
        *   `addPrototype(type: string, character: Character)` : pour enregistrer un personnage prototype sous un certain type.
        *   `getCharacter(type: string)` : qui retourne un *clone* du prototype correspondant au type demandé.

2.  **Utilisation du Registre :**
    *   Créez plusieurs prototypes de `Character` (un guerrier avec une épée, un mage avec un bâton, etc.).
    *   Enregistrez-les dans votre `CharacterRegistry`.
    *   Utilisez le registre pour créer plusieurs instances de personnages de types différents (ex: `registry.getCharacter("Warrior")`, `registry.getCharacter("Mage")`).
    *   Vérifiez que chaque personnage créé est une instance indépendante et modifiable sans affecter les autres.

### Conseils pour l'apprentissage

*   N'hésitez pas à utiliser les outils d'IA pour vous aider dans la syntaxe ou la structure de base des classes et des méthodes de clonage. C'est un excellent moyen d'accélérer le développement.
*   Cependant, concentrez-vous sur la *compréhension* des concepts de clonage superficiel vs. profond. L'IA peut vous donner le "comment", mais votre rôle est de comprendre le "pourquoi" et le "quand" un type de clonage est approprié.
*   Soyez attentifs aux détails des implémentations de `clone()` : un simple `Object.assign()` ou une copie de référence pour une liste ne suffira pas pour un clonage profond.

### Livrables

*   Le code source de vos classes `Item`, `Character`, et `CharacterRegistry` (si implémentée).
*   Le code de test illustrant les deux types de clonage (superficiel et profond) et leurs effets.
*   Une brève explication (commentaires dans le code ou fichier texte séparé) des observations faites concernant le clonage superficiel et profond, répondant aux questions posées dans les Parties 1 et 2.

---