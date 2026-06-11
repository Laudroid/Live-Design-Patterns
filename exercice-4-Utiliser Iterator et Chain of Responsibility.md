Bonjour !

Ce TP a pour objectif de vous faire manipuler deux Design Patterns fondamentaux : **Iterator** et **Chain of Responsibility**. Vous allez les utiliser conjointement pour résoudre une problématique courante de traitement de données et de délégation de tâches.

---

### TP : Gestion de Requêtes de Support Client avec Iterator et Chain of Responsibility

**Objectif Pédagogique :**
Mettre en œuvre les Design Patterns Iterator et Chain of Responsibility pour la gestion et le traitement délégué d'une série de requêtes.

**Contexte & Problématique :**
Une entreprise reçoit quotidiennement un volume important de requêtes de support client. Ces requêtes sont de natures diverses (technique, facturation, générale) et nécessitent des niveaux d'expertise différents. L'objectif est de concevoir un système permettant de parcourir efficacement ces requêtes et de les acheminer automatiquement vers le service ou l'expert approprié.

**Architecture Générale :**
Le pattern **Iterator** sera utilisé pour parcourir la collection de requêtes. Chaque requête sera ensuite soumise à une **chaîne de gestionnaires (Chain of Responsibility)** qui décideront si elles peuvent la traiter ou si elles doivent la transmettre au maillon suivant.

---

### Détail des Étapes :

**Étape 1 : La Requête (Request/Ticket)**

Créez une classe `Request` (ou `Ticket`) qui représentera une requête de support client.
Elle devra contenir les propriétés suivantes :
*   `id` (identifiant unique)
*   `description` (texte libre de la requête)
*   `type` (un type prédéfini, par exemple : `TECHNIQUE`, `FACTURATION`, `GENERALE`)
*   `priority` (un niveau de priorité, par exemple : `BASSE`, `MOYENNE`, `HAUTE`)

Prévoyez des méthodes pour accéder à ces propriétés (`getType()`, `getPriority()`, etc.).

**Étape 2 : L'Itérateur de Requêtes**

Implémentez le pattern Iterator pour parcourir une collection de `Request`.
*   Créez une interface `RequestIterator` (ou `Iterator<Request>`) avec les méthodes `hasNext()` et `next()`.
*   Créez une classe `RequestCollection` (l'agrégat) qui contiendra une liste de `Request`. Cette classe devra avoir :
    *   Une méthode `addRequest(Request request)` pour ajouter une requête à la collection.
    *   Une méthode `createIterator()` qui retourne une instance de votre `RequestIterator` capable de parcourir les requêtes de la collection.
*   Implémentez une classe concrète `ConcreteRequestIterator` qui implémente `RequestIterator` et qui sait comment parcourir la `RequestCollection`.

**Étape 3 : La Chaîne de Responsabilité des Gestionnaires**

Implémentez le pattern Chain of Responsibility pour le traitement des requêtes.
*   Créez une interface ou une classe abstraite `SupportHandler` (le gestionnaire abstrait) avec :
    *   Une méthode `setNextHandler(SupportHandler handler)` pour définir le maillon suivant dans la chaîne.
    *   Une méthode abstraite `handleRequest(Request request)` que les gestionnaires concrets devront implémenter.
*   Créez plusieurs implémentations concrètes de `SupportHandler` :
    *   `GeneralSupportHandler` : Gère les requêtes de type `GENERALE` avec une priorité `BASSE` ou `MOYENNE`.
    *   `TechnicalSupportHandler` : Gère les requêtes de type `TECHNIQUE` avec une priorité `MOYENNE` ou `HAUTE`.
    *   `BillingSupportHandler` : Gère les requêtes de type `FACTURATION` avec une priorité `MOYENNE` ou `HAUTE`.
    *   `UnresolvedRequestLogger` : Ce gestionnaire sera le dernier de la chaîne. Il ne traite aucune requête mais logue simplement celles qui lui parviennent, indiquant qu'elles n'ont pas pu être traitées par les gestionnaires précédents.

Chaque gestionnaire concret, dans sa méthode `handleRequest()`, devra :
1.  Vérifier s'il peut traiter la requête en fonction de son type et de sa priorité.
2.  Si oui, afficher un message indiquant qu'il a traité la requête (ex: "GeneralSupportHandler a traité la requête #123").
3.  Si non, passer la requête au gestionnaire suivant dans la chaîne (s'il existe).

**Étape 4 : Intégration et Simulation**

Dans votre programme principal (`main` ou équivalent) :
1.  Créez une instance de `RequestCollection`.
2.  Ajoutez-y plusieurs requêtes variées (différents types et priorités) pour tester tous les cas.
3.  Créez les instances de vos gestionnaires concrets (`GeneralSupportHandler`, `TechnicalSupportHandler`, `BillingSupportHandler`, `UnresolvedRequestLogger`).
4.  Construisez la chaîne de responsabilité en liant les gestionnaires (par exemple : `General` -> `Technical` -> `Billing` -> `Unresolved`).
5.  Obtenez un itérateur de la `RequestCollection`.
6.  Parcourez toutes les requêtes à l'aide de l'itérateur. Pour chaque requête :
    *   Passez-la au premier gestionnaire de votre chaîne (`GeneralSupportHandler` dans l'exemple ci-dessus).
    *   Observez les messages affichés pour vérifier que les requêtes sont bien traitées ou transmises correctement.

---

### Consignes Générales :

*   Utilisez le langage de programmation de votre choix (Java, C#, Python, PHP, etc.).
*   Structurez votre code de manière claire et lisible, en respectant les principes SOLID autant que possible.
*   N'hésitez pas à utiliser l'IA comme un assistant pour la génération de code boilerplate ou la clarification de concepts, mais assurez-vous de comprendre chaque ligne et chaque choix architectural. L'objectif est votre compréhension et votre capacité à justifier vos implémentations.
*   Ajoutez des commentaires pertinents là où la logique n'est pas immédiatement évidente.
*   Prévoyez un petit programme principal (`main` ou équivalent) pour démontrer le fonctionnement de votre système.

---

### Critères d'Évaluation :

*   Correcte implémentation des patterns Iterator et Chain of Responsibility.
*   Clarté et organisation du code (respect des conventions, nommage pertinent).
*   Gestion appropriée des différents types et priorités de requêtes par la chaîne.
*   Démonstration fonctionnelle du système via le programme principal.

---

### Pour Aller Plus Loin (Optionnel) :

*   Ajouter un gestionnaire de requêtes de "haute priorité" qui pourrait court-circuiter une partie de la chaîne ou être placé en début de chaîne pour un traitement immédiat.
*   Implémenter un filtre sur l'itérateur pour ne traiter que certains types de requêtes à la fois (par exemple, un `PriorityFilterIterator`).
*   Gérer le cas où une requête est traitée par plusieurs gestionnaires (si la logique le permet, par exemple, une requête technique pourrait aussi nécessiter une étape de facturation).

Bon courage pour ce TP !