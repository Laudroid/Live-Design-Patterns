# Article 1-3-1 : Rappel des principes SOLID : Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion

## Introduction

Les principes **SOLID** forment un ensemble de directives fondamentales pour concevoir des logiciels orientés objet robustes, modulaires et faciles à maintenir. Ces cinq principes permettent de structurer le code pour faciliter l’extensibilité, la compréhension et la réutilisation.

---

## Présentation des 5 principes SOLID

### 1. Single Responsibility Principle (SRP) - Principe de responsabilité unique

Chaque classe doit avoir une seule raison de changer, c’est-à-dire qu’elle doit être responsable d’une seule fonctionnalité.

**Exemple :**

```java
class InvoicePrinter {
    public void printInvoice(Invoice invoice) {
        // Impression
    }
}

class InvoicePersistence {
    public void saveToDatabase(Invoice invoice) {
        // Sauvegarde
    }
}
```

*Ici, l’impression et la persistance sont séparées en deux classes distinctes.*

---

### 2. Open/Closed Principle (OCP) - Principe Ouvert/Fermé

Les entités (classes, modules, fonctions) doivent être **ouvertes à l’extension**, mais **fermées à la modification**. Cela signifie que pour ajouter un comportement, on doit pouvoir étendre sans modifier le code existant.

**Exemple avec une stratégie :**

```java
interface Shape {
    double area();
}

class Circle implements Shape {
    double radius;
    public double area() { return Math.PI * radius * radius; }
}

class Rectangle implements Shape {
    double width, height;
    public double area() { return width * height; }
}
```

*Pour ajouter une nouvelle forme, on crée une nouvelle classe qui implémente `Shape` sans modifier les autres.*

---

### 3. Liskov Substitution Principle (LSP) - Principe de substitution de Liskov

Les objets d’une classe dérivée doivent pouvoir remplacer les objets de la classe mère sans altérer le fonctionnement du programme.

**Violation classique :**

```java
class Rectangle {
    void setWidth(int w);
    void setHeight(int h);
}

class Square extends Rectangle {
    void setWidth(int w) {
        // modifie la largeur et la hauteur
    }
    void setHeight(int h) {
        // modifie largeur et hauteur
    }
}
```

*Un `Square` ne peut pas être substitué à un `Rectangle` sans changer les attentes liées aux méthodes.*

---

### 4. Interface Segregation Principle (ISP) - Principe de séparation des interfaces

Il vaut mieux plusieurs interfaces spécifiques que d’imposer une interface unique générale. Les clients ne doivent dépendre que des méthodes qu’ils utilisent.

**Exemple :**

```java
interface Printer {
    void print();
}

interface Scanner {
    void scan();
}

class MultiFunctionPrinter implements Printer, Scanner {
    public void print() {}
    public void scan() {}
}

class OldPrinter implements Printer {
    public void print() {}
}
```

---

### 5. Dependency Inversion Principle (DIP) - Principe d’inversion des dépendances

Les modules de haut niveau ne doivent pas dépendre des modules de bas niveau. Tous les deux doivent dépendre d’abstractions.

**Exemple avec une dépendance inversée :**

```java
interface MessageService {
    void sendMessage(String message);
}

class EmailService implements MessageService {
    public void sendMessage(String message) {
        // envoie email
    }
}

class Notification {
    private MessageService service;
    public Notification(MessageService s) { service = s; }
    public void notifyUser(String msg) {
        service.sendMessage(msg);
    }
}
```

*`Notification` dépend de l’interface `MessageService`, non de la classe concrète.*

---

## Diagramme Mermaid résumant SOLID

```mermaid
graph LR
    SRP[Single Responsibility Principle]
    OCP[Open/Closed Principle]
    LSP[Liskov Substitution Principle]
    ISP[Interface Segregation Principle]
    DIP[Dependency Inversion Principle]

    SRP --> OCP --> LSP --> ISP --> DIP
```

---

## Résumé

| Principe               | But principal                                      |
|-----------------------|----------------------------------------------------|
| SRP                   | Une classe = une responsabilité                     |
| OCP                   | Étendre sans modifier                                |
| LSP                   | Substituabilité garantie                             |
| ISP                   | Interfaces fines, spécifiques                        |
| DIP                   | Dépendre d’abstractions, pas de concrétions         |

---

## Sources utilisées

- Robert C. Martin, "Principles of Object-Oriented Design", https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html  
- Refactoring Guru, "SOLID Principles", https://refactoring.guru/design-patterns/solid  
- Wikipedia, "SOLID", https://en.wikipedia.org/wiki/SOLID  

---

Les principes SOLID orientent la conception vers des logiciels mieux structurés, prêtant à une évolution agile et facilitant la maintenance grâce à une séparation claire des responsabilités et un découplage efficace entre composants.