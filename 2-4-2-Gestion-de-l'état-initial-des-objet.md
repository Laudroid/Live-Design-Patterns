# Article 2-4-2 : Gestion de l’état initial des objets avec le pattern Prototype

## Introduction

Dans le cadre du pattern **Prototype**, la gestion de l’état initial des objets clonés est un enjeu fondamental pour garantir la cohérence et la fiabilité des instances dupliquées. Cette gestion s’assure que les objets clonés possèdent un état valide, évitant ainsi des erreurs liées au partage ou à la réutilisation inappropriée des données.

---

## Problématique de l'état initial lors du clonage

Lorsqu’un objet est cloné, deux cas peuvent se présenter :

- **L’état de l’objet est partagé** entre l’original et la copie (partage de références), ce qui peut entraîner des effets secondaires non souhaités.  
- **L’état doit être réinitialisé** ou modifié dans la copie pour s’adapter à un nouveau contexte.

Le prototype doit gérer correctement cet état pour éviter les erreurs, notamment dans le cas des attributs mutables ou des ressources externes (fichiers, connexions, etc.).

---

## Approches pour gérer l’état initial

### 1. Utiliser un clonage profond (Deep Clone)

Le clonage profond crée une copie indépendante de tous les objets référencés. Cela garantit que les changements dans le clone n’affecteront pas l’original.

**Exemple** : ici, la classe `Person` clone également son objet `Address` pour garantir un état indépendant.

### 2. Personnaliser l’état après clonage

Souvent, il est nécessaire d’implémenter une logique d’initialisation post-clonage pour adapter certains attributs (ex : identifiants uniques, états de connexion).

Cette approche peut se faire via :

- Un constructeur de copie.  
- Une méthode d’`init` ou `reset` appelée après le clonage.  
- L’implémentation personnalisée de la méthode `clone()`.

### 3. Immutabilité partielle

Dans certains cas, rendre un objet partiellement ou totalement immutable minimise les problèmes liés à l’état partagé.

---

## Exemple concret en Java

```java
public class Person implements Cloneable {
    private String id;
    private String name;
    private Address address;

    public Person(String id, String name, Address address) {
        this.id = id;
        this.name = name;
        this.address = address;
    }

    @Override
    protected Person clone() throws CloneNotSupportedException {
        Person cloned = (Person) super.clone();
        cloned.address = address.clone(); // deep copy
        cloned.id = generateNewId();      // reset ID pour un nouvel état
        return cloned;
    }

    private String generateNewId() {
        return "ID-" + System.currentTimeMillis();
    }

    @Override
    public String toString() {
        return "Person{id='" + id + "', name='" + name + "', address=" + address + "}";
    }
}

public class Address implements Cloneable {
    private String city;
    private String street;

    public Address(String city, String street) {
        this.city = city;
        this.street = street;
    }

    @Override
    protected Address clone() throws CloneNotSupportedException {
        return (Address) super.clone();
    }

    @Override
    public String toString() {
        return "Address{city='" + city + "', street='" + street + "'}";
    }
}
```

**Utilisation :**

```java
Person original = new Person("ID-123", "Alice", new Address("Paris", "Rue A"));
Person clone = original.clone();
System.out.println(original);
System.out.println(clone);
```

La méthode `clone()` réinitialise ici l’attribut `id` dans l’objet cloné, garantissant un état initial différent de l’original.

---

## Diagramme Mermaid illustrant la gestion de l’état dans Prototype

```mermaid
classDiagram
    class Prototype {
        <<interface>>
        + clone() Prototype
        + resetState()
    }
    class ConcretePrototype {
        - state: State
        + clone() Prototype
        + resetState()
    }
    class Client {
        - prototype: Prototype
        + createClone()
    }

    Prototype <|.. ConcretePrototype
    Client ..> Prototype

    note "resetState permet d'adapter/modifier l'état du clone après duplication" as N
```

---

## Points clés à retenir

- Le clonage ne se limite pas à copier les données, il inclut aussi la gestion fine de l’état.  
- Le clonage profond est souvent nécessaire pour prévenir les erreurs liées aux références partagées.  
- L’état initial du clone peut nécessiter une réinitialisation ou adaptation spécifique selon le contexte métier.  
- Implémenter une logique claire et explicite pour gérer l’état garantit la robustesse du pattern Prototype.

---

## Sources utilisées

- Refactoring Guru, "Prototype Pattern — When and How to Use It", https://refactoring.guru/design-patterns/prototype  
- Oracle Java Tutorials, "Cloneable Interface", https://docs.oracle.com/javase/tutorial/java/IandI/objectclone.html  
- Wikipedia, "Prototype pattern", https://en.wikipedia.org/wiki/Prototype_pattern  

---

La gestion soignée de l’état initial des objets clonés maximise les bénéfices du pattern Prototype en assurant la création d’instances fiables, distinctes et adaptées à leurs usages spécifiques.