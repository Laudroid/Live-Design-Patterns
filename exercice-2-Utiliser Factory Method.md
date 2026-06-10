Bonjour !

Ce TP vous permettra de mettre en pratique le design pattern **Factory Method**. L'objectif est de comprendre comment déléguer l'instanciation d'objets à des sous-classes, rendant votre code plus flexible et découplé.

---

## TP : Implémentation du Design Pattern Factory Method

### Objectif du TP

Appliquer le pattern Factory Method pour déléguer l'instanciation d'objets à des sous-classes, en respectant le principe d'ouverture/fermeture (Open/Closed Principle).

### Contexte

Vous développez une suite d'applications de bureautique (traitement de texte, tableur, présentation). Chaque application doit pouvoir créer son propre type de document sans que le code de l'application principale ne soit directement couplé aux classes concrètes des documents.

### Énoncé

Votre tâche est de concevoir et d'implémenter une structure de classes utilisant le pattern Factory Method pour gérer la création de différents types de documents par leurs applications respectives.

#### Partie 1 : Les Produits (Documents)

1.  **Définir l'interface/classe abstraite `Document`** :
    *   Cette interface ou classe abstraite représentera le concept générique d'un document.
    *   Elle devra déclarer au moins une méthode abstraite, par exemple `ouvrir()` ou `afficherContenu()`, qui sera implémentée par les documents concrets.

2.  **Implémenter des classes de documents concrets** :
    *   Créez au moins trois classes concrètes qui implémentent ou étendent `Document` :
        *   `TextDocument` (pour un traitement de texte)
        *   `SpreadsheetDocument` (pour un tableur)
        *   `PresentationDocument` (pour une application de présentation)
    *   Chaque classe concrète doit implémenter la méthode `ouvrir()` (ou `afficherContenu()`) de manière spécifique, par exemple en affichant un message simple indiquant le type de document et son action.

#### Partie 2 : Les Créateurs (Applications)

1.  **Définir la classe abstraite `Application`** :
    *   Cette classe représentera le concept générique d'une application de bureautique.
    *   Elle doit déclarer une **Factory Method** abstraite, par exemple `creerDocument()`, qui retournera un objet de type `Document`. C'est cette méthode que les sous-classes devront implémenter.
    *   Elle doit également contenir une méthode concrète (non abstraite), par exemple `nouveauDocument()`, qui utilise la Factory Method pour créer un document, puis appelle la méthode `ouvrir()` de ce document. Cette méthode `nouveauDocument()` ne doit pas connaître les classes concrètes des documents.

2.  **Implémenter des classes d'applications concrètes** :
    *   Créez au moins trois classes concrètes qui étendent `Application` :
        *   `TextEditorApp`
        *   `SpreadsheetApp`
        *   `PresentationApp`
    *   Chaque application concrète doit implémenter la Factory Method `creerDocument()` pour retourner l'instance du type de document qui lui correspond (par exemple, `TextEditorApp` doit retourner un `TextDocument`).

#### Partie 3 : Utilisation et Test

1.  **Créer une classe principale (par exemple `Main` ou `Client`)** :
    *   Dans cette classe, instanciez chaque type d'application concrète (`TextEditorApp`, `SpreadsheetApp`, `PresentationApp`).
    *   Pour chaque instance d'application, appelez la méthode `nouveauDocument()`.
    *   Vérifiez que chaque application crée et ouvre correctement le type de document attendu, sans que le code client n'ait eu à instancier directement les documents.

### Consignes Générales

*   Choisissez un langage de programmation de votre choix (Java, C#, Python, TypeScript, etc.).
*   Structurez votre code de manière claire et lisible.
*   Ajoutez des commentaires pertinents là où c'est nécessaire pour expliquer vos choix ou des parties complexes.

### Pour aller plus loin (Optionnel)

*   Ajoutez une méthode `enregistrer()` à l'interface `Document` et implémentez-la dans les classes concrètes.
*   Modifiez la méthode `nouveauDocument()` de la classe `Application` pour qu'elle appelle également `enregistrer()` après `ouvrir()`.
*   Explorez comment vous pourriez ajouter un nouveau type de document (par exemple, `ImageDocument`) et son application (`ImageEditorApp`) sans modifier les classes `Application` ou `Main` existantes (c'est le principe d'ouverture/fermeture en action !).

---

Bon courage pour ce TP ! C'est une excellente occasion de solidifier votre compréhension des patterns de création.