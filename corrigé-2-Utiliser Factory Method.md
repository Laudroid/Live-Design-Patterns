Voici une proposition de solution pour le TP sur le Design Pattern Factory Method.

---

# Solution du TP : Implémentation du Design Pattern Factory Method

## Partie 1 : Les Produits (Documents)

### 1.1. Interface `Document`


```java
// Fichier: Document.java
public interface Document {
    void ouvrir();
    void enregistrer(); // Ajout pour l'étape "Pour aller plus loin"
}
```


### 1.2. Classes de documents concrets


```java
// Fichier: TextDocument.java
public class TextDocument implements Document {
    @Override
    public void ouvrir() {
        System.out.println("Ouverture d'un document texte.");
    }

    @Override
    public void enregistrer() {
        System.out.println("Enregistrement du document texte.");
    }
}
```



```java
// Fichier: SpreadsheetDocument.java
public class SpreadsheetDocument implements Document {
    @Override
    public void ouvrir() {
        System.out.println("Ouverture d'un document tableur.");
    }

    @Override
    public void enregistrer() {
        System.out.println("Enregistrement du document tableur.");
    }
}
```



```java
// Fichier: PresentationDocument.java
public class PresentationDocument implements Document {
    @Override
    public void ouvrir() {
        System.out.println("Ouverture d'un document de présentation.");
    }

    @Override
    public void enregistrer() {
        System.out.println("Enregistrement du document de présentation.");
    }
}
```


## Partie 2 : Les Créateurs (Applications)

### 2.1. Classe abstraite `Application`


```java
// Fichier: Application.java
public abstract class Application {

    // La Factory Method abstraite
    // Les sous-classes implémenteront cette méthode pour créer le type de document approprié.
    protected abstract Document creerDocument();

    // Méthode concrète qui utilise la Factory Method
    // Cette méthode ne connaît pas les classes concrètes des documents.
    public void nouveauDocument() {
        System.out.println("--- Création d'un nouveau document ---");
        Document doc = creerDocument(); // Appel de la Factory Method
        doc.ouvrir();
        doc.enregistrer(); // Ajout pour l'étape "Pour aller plus loin"
        System.out.println("--- Document prêt ---\n");
    }
}
```


### 2.2. Classes d'applications concrètes


```java
// Fichier: TextEditorApp.java
public class TextEditorApp extends Application {
    @Override
    protected Document creerDocument() {
        System.out.println("TextEditorApp: Création d'un TextDocument.");
        return new TextDocument();
    }
}
```



```java
// Fichier: SpreadsheetApp.java
public class SpreadsheetApp extends Application {
    @Override
    protected Document creerDocument() {
        System.out.println("SpreadsheetApp: Création d'un SpreadsheetDocument.");
        return new SpreadsheetDocument();
    }
}
```



```java
// Fichier: PresentationApp.java
public class PresentationApp extends Application {
    @Override
    protected Document creerDocument() {
        System.out.println("PresentationApp: Création d'un PresentationDocument.");
        return new PresentationDocument();
    }
}
```


## Partie 3 : Utilisation et Test

### Classe principale `Main`


```java
// Fichier: Main.java
public class Main {
    public static void main(String[] args) {
        // Instanciation des applications concrètes
        Application textEditor = new TextEditorApp();
        Application spreadsheet = new SpreadsheetApp();
        Application presentation = new PresentationApp();

        // Chaque application crée et ouvre son document spécifique
        textEditor.nouveauDocument();
        spreadsheet.nouveauDocument();
        presentation.nouveauDocument();
    }
}
```


### Sortie attendue du programme


```
--- Création d'un nouveau document ---
TextEditorApp: Création d'un TextDocument.
Ouverture d'un document texte.
Enregistrement du document texte.
--- Document prêt ---

--- Création d'un nouveau document ---
SpreadsheetApp: Création d'un SpreadsheetDocument.
Ouverture d'un document tableur.
Enregistrement du document tableur.
--- Document prêt ---

--- Création d'un nouveau document ---
PresentationApp: Création d'un PresentationDocument.
Ouverture d'un document de présentation.
Enregistrement du document de présentation.
--- Document prêt ---
```


Le code client (`Main`) interagit uniquement avec l'abstraction `Application` et ne connaît pas les classes concrètes de documents (`TextDocument`, etc.). Chaque application concrète est responsable de l'instanciation de son document spécifique via la Factory Method `creerDocument()`.

## Pour aller plus loin (Optionnel)

### Ajout de la méthode `enregistrer()`

Les modifications ont déjà été intégrées dans les implémentations des classes `Document`, `TextDocument`, `SpreadsheetDocument`, `PresentationDocument` et `Application` ci-dessus.

### Ajout d'un nouveau type de document (ImageDocument)

L'avantage du Factory Method est sa capacité à étendre le système sans modifier le code existant (respect du principe d'ouverture/fermeture).

1.  **Créer la nouvelle classe de document concret :**

    
```java
    // Fichier: ImageDocument.java
    public class ImageDocument implements Document {
        @Override
        public void ouvrir() {
            System.out.println("Ouverture d'un document image.");
        }

        @Override
        public void enregistrer() {
            System.out.println("Enregistrement du document image.");
        }
    }
    ```


2.  **Créer la nouvelle classe d'application concrète :**

    
```java
    // Fichier: ImageEditorApp.java
    public class ImageEditorApp extends Application {
        @Override
        protected Document creerDocument() {
            System.out.println("ImageEditorApp: Création d'un ImageDocument.");
            return new ImageDocument();
        }
    }
    ```


3.  **Mettre à jour la classe `Main` (client) :**
    Seule la classe `Main` (le client) doit être modifiée pour instancier et utiliser la nouvelle application. Les classes `Document`, `Application`, `TextEditorApp`, `SpreadsheetApp`, `PresentationApp` n'ont pas besoin d'être touchées.

    
```java
    // Fichier: Main.java (version mise à jour)
    public class Main {
        public static void main(String[] args) {
            Application textEditor = new TextEditorApp();
            Application spreadsheet = new SpreadsheetApp();
            Application presentation = new PresentationApp();
            Application imageEditor = new ImageEditorApp(); // Nouvelle application

            textEditor.nouveauDocument();
            spreadsheet.nouveauDocument();
            presentation.nouveauDocument();
            imageEditor.nouveauDocument(); // Utilisation de la nouvelle application
        }
    }
    ```


Cette démonstration illustre parfaitement comment le Factory Method permet d'ajouter de nouvelles fonctionnalités (nouveaux types de documents et leurs applications) sans modifier les parties existantes du code, rendant le système plus robuste et facile à maintenir.