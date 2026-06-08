Bonjour à toutes et à tous,

Ce TP a pour but de vous confronter à des situations de code réelles, où les bonnes intentions peuvent parfois mener à des structures complexes et difficiles à maintenir. L'objectif n'est pas de juger, mais de comprendre comment améliorer l'architecture logicielle.

---

### **TP : Débusquer et Corriger les Anti-Patterns dans un Système Existant**

**Objectifs Pédagogiques :**

À l'issue de ce TP, vous serez capable de :
*   Identifier les anti-patterns courants dans un code existant.
*   Comprendre les conséquences négatives de ces anti-patterns sur la maintenabilité, l'évolutivité et la testabilité du code.
*   Proposer des solutions de refactoring concrètes en vous appuyant sur les Design Patterns et les principes de conception logicielle (SOLID, DRY, KISS).

**Ressources :**

N'hésitez pas à consulter la documentation sur les Design Patterns et les Anti-Patterns. L'utilisation d'outils d'IA est encouragée pour vous aider dans votre analyse et vos propositions, à condition de toujours valider et justifier leurs suggestions par votre propre compréhension.

---

**Mise en Situation : Le Système "CommerceLegacy"**

Vous intégrez une équipe chargée de moderniser un module de gestion de commandes au sein d'un système e-commerce existant, nommé `CommerceLegacy`. Ce système a évolué au fil des années, avec de nombreuses fonctionnalités ajoutées sans une refonte architecturale majeure.

On vous a signalé que la classe `OrderProcessor` et la manière dont les produits sont gérés posent des problèmes récurrents :
*   Les modifications dans le processus de commande sont complexes et risquent d'introduire des bugs inattendus.
*   L'ajout de nouvelles logiques de validation ou de notification est fastidieux.
*   Le code est difficile à tester unitairement.

Voici une description simplifiée des responsabilités actuelles de certaines classes clés :

**1. La classe `OrderProcessor` :**

Cette classe est responsable de la gestion complète du cycle de vie d'une commande. Elle contient des méthodes comme :
*   `processOrder(Order order)` : Méthode principale qui orchestre toutes les étapes.
*   `validateOrder(Order order)` : Contient des règles de validation complexes (disponibilité des produits, solde client, règles promotionnelles).
*   `saveOrder(Order order)` : Gère la persistance de la commande en base de données.
*   `sendConfirmationEmail(Order order)` : Envoie l'e-mail de confirmation au client.
*   `updateInventory(Order order)` : Décrémente les stocks des produits commandés.
*   `generateInvoice(Order order)` : Crée la facture au format PDF.
*   `notifyMarketingTeam(Order order)` : Envoie une notification à l'équipe marketing pour des analyses.

**2. La gestion des produits et des prix :**

La classe `Product` est une simple classe de données (POJO/DTO) qui contient des attributs comme `id`, `name`, `description`, `price`, `stockQuantity`.
Cependant, la logique de calcul des prix finaux (incluant taxes, promotions spécifiques au client, frais de port) est dispersée. On la retrouve souvent directement dans la classe `OrderProcessor` ou dans des utilitaires statiques comme `PriceCalculatorUtil.calculateFinalPrice(Product product, Customer customer, String region)`.

---

**Travail Demandé :**

Votre mission est d'analyser cette situation, d'identifier les mauvaises pratiques et de proposer des améliorations.

**Partie 1 : Analyse Critique et Identification des Anti-Patterns**

1.  **Identifiez les anti-patterns** présents dans la description de la classe `OrderProcessor` et dans la gestion des prix/produits.
2.  Pour chaque anti-pattern identifié, **justifiez votre choix** en expliquant pourquoi il s'agit d'un anti-pattern et quelles sont ses conséquences négatives sur le système (maintenabilité, évolutivité, testabilité, lisibilité, etc.).

**Partie 2 : Propositions de Refactoring et Alternatives**

1.  Pour chaque anti-pattern identifié en Partie 1, **proposez une ou plusieurs alternatives** de refactoring.
2.  **Décrivez les Design Patterns ou les principes de conception logicielle (SOLID, DRY, KISS)** que vous utiliseriez pour corriger ces mauvaises pratiques.
3.  **Illustrez vos propositions** avec des exemples de structure de classes ou de méthodes (pas besoin de code complet et exécutable, une description textuelle claire ou des diagrammes de classes simplifiés suffisent). Par exemple, si vous proposez de décomposer une classe, décrivez les nouvelles classes et leurs responsabilités.

**Partie 3 : Impact et Justification**

1.  **Expliquez les bénéfices attendus** de vos propositions de refactoring pour le système `CommerceLegacy`.
2.  **Discutez des compromis** potentiels ou des défis que pourrait représenter l'implémentation de vos solutions (par exemple, coût de la refonte, impact sur l'équipe, complexité initiale accrue pour une meilleure flexibilité future).

---

**Rendu :**

Un document clair et structuré (format PDF, Markdown ou Word) présentant votre analyse, vos propositions et vos justifications.

Bon courage et amusez-vous à démêler ce code !
