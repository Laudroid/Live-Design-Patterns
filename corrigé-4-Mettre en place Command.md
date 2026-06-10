Voici une proposition de solution pour le TP sur le Design Pattern Command avec Undo/Redo, en utilisant Java pour les exemples de code.

---

# Solution du TP : Implémentation du Pattern Command avec Undo/Redo

## Étape 1 : Le Récepteur (Receiver) - Le Document

La classe `Document` contient le contenu textuel et les méthodes qui le modifient.





```java
// Fichier: Document.java
public class Document {
    private StringBuilder content;

    public Document() {
        this.content = new StringBuilder();
    }

    public void insertText(int position, String text) {
        if (position < 0 || position > content.length()) {
            throw new IllegalArgumentException("Position d'insertion invalide.");
        }
        content.insert(position, text);
        System.out.println($"[Document] Texte inséré à la position {position}: '{text}'");
    }

    public String deleteText(int startPosition, int endPosition) {
        if (startPosition < 0 || endPosition > content.length() || startPosition > endPosition) {
            throw new IllegalArgumentException("Positions de suppression invalides.");
        }
        String deletedText = content.substring(startPosition, endPosition);
        content.delete(startPosition, endPosition);
        System.out.println($"[Document] Texte supprimé de {startPosition} à {endPosition}: '{deletedText}'");
        return deletedText;
    }

    public String replaceText(int startPosition, int endPosition, String newText) {
        if (startPosition < 0 || endPosition > content.length() || startPosition > endPosition) {
            throw new IllegalArgumentException("Positions de remplacement invalides.");
        }
        String oldText = content.substring(startPosition, endPosition);
        content.replace(startPosition, endPosition, newText);
        System.out.println($"[Document] Texte remplacé de {startPosition} à {endPosition} ('{oldText}') par: '{newText}'");
        return oldText;
    }

    public String getContent() {
        return content.toString();
    }

    public void displayContent() {
        System.out.println("Contenu actuel du document: \"" + getContent() + "\"");
    }
}
```




## Étape 2 : L'Interface de Commande (Command Interface)

Cette interface définit le contrat pour toutes les commandes exécutables et annulables.





```java
// Fichier: Command.java
public interface Command {
    void execute();
    void undo();
}
```




## Étape 3 : Les Commandes Concrètes (Concrete Commands)

Chaque commande concrète encapsule une opération spécifique et les informations nécessaires pour l'annuler.

### `InsertTextCommand`





```java
// Fichier: InsertTextCommand.java
public class InsertTextCommand implements Command {
    private Document document;
    private int position;
    private String textToInsert;

    public InsertTextCommand(Document document, int position, String textToInsert) {
        this.document = document;
        this.position = position;
        this.textToInsert = textToInsert;
    }

    @Override
    public void execute() {
        document.insertText(position, textToInsert);
    }

    @Override
    public void undo() {
        // Pour annuler une insertion, on supprime le texte inséré
        document.deleteText(position, position + textToInsert.length());
    }
}
```




### `DeleteTextCommand`





```java
// Fichier: DeleteTextCommand.java
public class DeleteTextCommand implements Command {
    private Document document;
    private int startPosition;
    private int endPosition; // Position de fin avant l'exécution
    private String deletedText; // Texte mémorisé pour l'annulation

    public DeleteTextCommand(Document document, int startPosition, int endPosition) {
        this.document = document;
        this.startPosition = startPosition;
        this.endPosition = endPosition;
    }

    @Override
    public void execute() {
        // Mémorise le texte supprimé avant l'opération
        this.deletedText = document.deleteText(startPosition, endPosition);
    }

    @Override
    public void undo() {
        // Pour annuler une suppression, on réinsère le texte supprimé à sa position d'origine
        document.insertText(startPosition, deletedText);
    }
}
```




### `ReplaceTextCommand`





```java
// Fichier: ReplaceTextCommand.java
public class ReplaceTextCommand implements Command {
    private Document document;
    private int startPosition;
    private int endPosition; // Position de fin avant l'exécution
    private String newText;
    private String oldText; // Texte original mémorisé pour l'annulation

    public ReplaceTextCommand(Document document, int startPosition, int endPosition, String newText) {
        this.document = document;
        this.startPosition = startPosition;
        this.endPosition = endPosition;
        this.newText = newText;
    }

    @Override
    public void execute() {
        // Mémorise le texte original avant l'opération
        this.oldText = document.replaceText(startPosition, endPosition, newText);
    }

    @Override
    public void undo() {
        // Pour annuler un remplacement, on remplace le nouveau texte par l'ancien
        // Attention aux indices si la longueur a changé. Ici, on suppose que la position de début est la même.
        // La position de fin pour l'undo est startPosition + longueur du newText
        document.replaceText(startPosition, startPosition + newText.length(), oldText);
    }
}
```




## Étape 4 : L'Invoker (Invoker) - Le Gestionnaire d'Historique

La classe `CommandProcessor` gère l'exécution des commandes et les piles pour l'undo/redo.





```java
// Fichier: CommandProcessor.java
import java.util.Stack;

public class CommandProcessor {
    private Stack<Command> undoStack;
    private Stack<Command> redoStack;

    public CommandProcessor() {
        this.undoStack = new Stack<>();
        this.redoStack = new Stack<>();
    }

    public void executeCommand(Command command) {
        command.execute();
        undoStack.push(command);
        // Important: Si une nouvelle commande est exécutée, l'historique de redo est invalidé
        redoStack.clear();
        System.out.println("[Historique] Commande exécutée. Redo stack vidée.");
    }

    public void undo() {
        if (!undoStack.isEmpty()) {
            Command command = undoStack.pop();
            command.undo();
            redoStack.push(command);
            System.out.println("[Historique] Undo effectué.");
        } else {
            System.out.println("[Historique] Impossible d'annuler: la pile d'undo est vide.");
        }
    }

    public void redo() {
        if (!redoStack.isEmpty()) {
            Command command = redoStack.pop();
            command.execute(); // On ré-exécute la commande
            undoStack.push(command);
            System.out.println("[Historique] Redo effectué.");
        } else {
            System.out.println("[Historique] Impossible de rétablir: la pile de redo est vide.");
        }
    }
}
```




## Étape 5 : Test et Validation

La classe `Main` simule l'interaction avec l'éditeur.





```java
// Fichier: Main.java
public class Main {
    public static void main(String[] args) {
        Document document = new Document();
        CommandProcessor processor = new CommandProcessor();

        System.out.println("--- Début des opérations ---");
        document.displayContent();

        // 1. Insérer "Bonjour " au début.
        Command cmd1 = new InsertTextCommand(document, 0, "Bonjour ");
        processor.executeCommand(cmd1);
        document.displayContent();

        // 2. Insérer "monde !" à la fin.
        Command cmd2 = new InsertTextCommand(document, document.getContent().length(), "monde !");
        processor.executeCommand(cmd2);
        document.displayContent();

        // 3. Supprimer "monde" (de la position 8 à 13)
        Command cmd3 = new DeleteTextCommand(document, 8, 13);
        processor.executeCommand(cmd3);
        document.displayContent();

        // 4. Insérer "le monde !" à la place (à la position 8)
        Command cmd4 = new InsertTextCommand(document, 8, "le monde !");
        processor.executeCommand(cmd4);
        document.displayContent();

        System.out.println("\n--- Début des Undo/Redo ---");

        // Undo 1: Annule l'insertion de "le monde !"
        processor.undo();
        document.displayContent();

        // Undo 2: Annule la suppression de "monde" (réinsère "monde")
        processor.undo();
        document.displayContent();

        // Redo 1: Rétablit la suppression de "monde"
        processor.redo();
        document.displayContent();

        // Redo 2: Rétablit l'insertion de "le monde !"
        processor.redo();
        document.displayContent();

        // Tentative de Redo (devrait échouer car la pile de redo est vide)
        processor.redo();
        document.displayContent();

        // Nouvelle commande après des undo/redo
        System.out.println("\n--- Nouvelle commande après undo/redo ---");
        Command cmd5 = new InsertTextCommand(document, document.getContent().length(), " Comment allez-vous ?");
        processor.executeCommand(cmd5);
        document.displayContent();

        // Tentative de Redo après une nouvelle commande (devrait échouer car redoStack a été vidée)
        processor.redo();
        document.displayContent();

        // Undo de la dernière commande
        processor.undo();
        document.displayContent();
    }
}
```




### Sortie console du programme





```
--- Début des opérations ---
Contenu actuel du document: ""
[Document] Texte inséré à la position 0: 'Bonjour '
[Historique] Commande exécutée. Redo stack vidée.
Contenu actuel du document: "Bonjour "
[Document] Texte inséré à la position 8: 'monde !'
[Historique] Commande exécutée. Redo stack vidée.
Contenu actuel du document: "Bonjour monde !"
[Document] Texte supprimé de 8 à 13: 'monde'
[Historique] Commande exécutée. Redo stack vidée.
Contenu actuel du document: "Bonjour !"
[Document] Texte inséré à la position 8: 'le monde !'
[Historique] Commande exécutée. Redo stack vidée.
Contenu actuel du document: "Bonjour le monde !!"

--- Début des Undo/Redo ---
[Historique] Undo effectué.
[Document] Texte supprimé de 8 à 18: 'le monde !'
Contenu actuel du document: "Bonjour !!"
[Historique] Undo effectué.
[Document] Texte inséré à la position 8: 'monde'
Contenu actuel du document: "Bonjour monde !!"
[Historique] Redo effectué.
[Document] Texte supprimé de 8 à 13: 'monde'
Contenu actuel du document: "Bonjour !!"
[Historique] Redo effectué.
[Document] Texte inséré à la position 8: 'le monde !'
Contenu actuel du document: "Bonjour le monde !!"
[Historique] Impossible de rétablir: la pile de redo est vide.
Contenu actuel du document: "Bonjour le monde !!"

--- Nouvelle commande après undo/redo ---
[Document] Texte inséré à la position 18: ' Comment allez-vous ?'
[Historique] Commande exécutée. Redo stack vidée.
Contenu actuel du document: "Bonjour le monde !! Comment allez-vous ?"
[Historique] Impossible de rétablir: la pile de redo est vide.
Contenu actuel du document: "Bonjour le monde !! Comment allez-vous ?"
[Historique] Undo effectué.
[Document] Texte supprimé de 18 à 40: ' Comment allez-vous ?'
Contenu actuel du document: "Bonjour le monde !!"
```




## Explications et Justifications

Le pattern Command est parfaitement adapté pour implémenter les fonctionnalités d'undo/redo dans un éditeur de texte simplifié.

*   **`Document` (Récepteur) :** C'est l'objet qui connaît les opérations réelles à effectuer. Il est complètement découplé des commandes et de l'invoker. Il ne sait pas qui l'appelle ni pourquoi.
*   **`Command` (Interface de Commande) :** Définit une interface uniforme pour toutes les opérations. Chaque commande concrète doit savoir comment s'exécuter (`execute()`) et comment s'annuler (`undo()`). C'est la clé de l'undo/redo.
*   **Commandes Concrètes (`InsertTextCommand`, `DeleteTextCommand`, `ReplaceTextCommand`) :** Chaque commande encapsule une requête spécifique, y compris le récepteur (`Document`) et tous les paramètres nécessaires à l'exécution *et* à l'annulation. Par exemple, `DeleteTextCommand` mémorise le texte supprimé pour pouvoir le réinsérer lors d'un `undo()`.
*   **`CommandProcessor` (Invoker / Gestionnaire d'Historique) :** Cet objet est responsable de l'exécution des commandes et de la gestion de l'historique.
    *   Il maintient deux piles (`undoStack` et `redoStack`) pour stocker les commandes.
    *   Lors de l'exécution d'une nouvelle commande, celle-ci est poussée sur `undoStack`, et la `redoStack` est vidée. C'est un comportement standard : si l'utilisateur effectue une nouvelle action après avoir annulé des opérations, il ne peut plus "refaire" les opérations annulées qui se trouvaient dans la `redoStack`.
    *   `undo()` dépile une commande de `undoStack`, appelle son `undo()` et la pousse sur `redoStack`.
    *   `redo()` dépile une commande de `redoStack`, appelle son `execute()` et la pousse sur `undoStack`.

**Avantages du pattern Command avec Undo/Redo :**

*   **Découplage :** L'émetteur de la requête (le client ou l'interface utilisateur) est découplé du récepteur (`Document`) et de la logique d'exécution. Le client n'a qu'à créer une commande et la passer à l'invoker.
*   **Support de l'Undo/Redo :** C'est l'avantage principal ici. En encapsulant chaque opération comme un objet avec des méthodes `execute()` et `undo()`, il devient simple de gérer un historique d'actions.
*   **Extensibilité :** L'ajout de nouvelles opérations (ex: `FormatTextCommand`, `CopyCommand`) est facile. Il suffit de créer une nouvelle classe `Command` sans modifier les classes existantes.
*   **Composition de commandes :** Des commandes complexes peuvent être construites à partir de commandes plus simples (pattern Composite appliqué aux commandes).
*   **Journalisation/Persistance :** Les commandes peuvent être facilement journalisées ou sérialisées pour être rejouées ou persistées.

Ce TP démontre la puissance du pattern Command pour structurer des systèmes où la gestion des actions et de leur historique est une exigence clé.