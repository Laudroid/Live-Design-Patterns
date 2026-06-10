Bonjour à toutes et à tous,

Ce TP vise à vous immerger dans l'implémentation du Design Pattern **Command**, en mettant l'accent sur une fonctionnalité essentielle : le support de l'annulation (undo) et du rétablissement (redo) des opérations.

---

### **TP : Implémentation du Pattern Command avec Undo/Redo**

**Objectif du TP :**
Mettre en œuvre le Design Pattern Command pour encapsuler des opérations, et étendre cette implémentation pour gérer un historique d'actions permettant l'annulation et le rétablissement.

**Contexte :**
Dans de nombreuses applications interactives (éditeurs de texte, logiciels de dessin, jeux vidéo), la capacité d'annuler une action ou de la rétablir est fondamentale pour l'expérience utilisateur. Le pattern Command est une solution élégante pour structurer ce type de fonctionnalité, en découplant l'émetteur d'une requête de son récepteur et en permettant la gestion d'un historique d'opérations.

**Mise en situation :**
Nous allons simuler un éditeur de texte très simplifié. Cet éditeur permettra d'effectuer des opérations de base sur une chaîne de caractères (le "document"). Chaque modification devra être une commande qui peut être exécutée, annulée et rétablie.

**Prérequis :**
*   Connaissance des principes de la Programmation Orientée Objet (POO).
*   Compréhension des interfaces et classes abstraites.
*   Maîtrise des structures de données de type pile (Stack).

---

#### **Étape 1 : Le Récepteur (Receiver) - Le Document**

Créez une classe `Document` (ou `TextDocument`). C'est l'objet sur lequel les commandes vont opérer.

*   Elle doit contenir une propriété pour stocker le contenu textuel (par exemple, un `StringBuilder` ou `String` en Java/C#, ou une simple `string` en Python/JS).
*   Implémentez les méthodes suivantes qui modifieront *réellement* le contenu du document :
    *   `insertText(int position, String text)` : Insère du texte à une position donnée.
    *   `deleteText(int startPosition, int endPosition)` : Supprime le texte entre deux positions.
    *   `replaceText(int startPosition, int endPosition, String newText)` : Remplace une portion de texte par une nouvelle.
*   Ajoutez une méthode `getContent()` pour récupérer le texte actuel du document et `displayContent()` pour l'afficher.

#### **Étape 2 : L'Interface de Commande (Command Interface)**

Définissez une interface `Command` (ou une classe abstraite `AbstractCommand`).

*   Elle doit déclarer deux méthodes :
    *   `void execute()` : Exécute l'opération.
    *   `void undo()` : Annule l'opération précédemment exécutée.

#### **Étape 3 : Les Commandes Concrètes (Concrete Commands)**

Implémentez les commandes concrètes pour chaque opération de votre `Document`. Chaque commande doit :

*   Prendre une instance du `Document` (le récepteur) dans son constructeur.
*   Stocker les informations nécessaires pour exécuter *et* annuler l'opération.

1.  **`InsertTextCommand`**
    *   **`execute()`** : Appelle `document.insertText(...)`.
    *   **`undo()`** : Supprime le texte qui a été inséré. Pour cela, la commande devra avoir mémorisé la position et la longueur du texte inséré.

2.  **`DeleteTextCommand`**
    *   **`execute()`** : Appelle `document.deleteText(...)`.
    *   **`undo()`** : Réinsère le texte qui a été supprimé. La commande devra donc avoir mémorisé le texte supprimé et sa position d'origine.

3.  **`ReplaceTextCommand`**
    *   **`execute()`** : Appelle `document.replaceText(...)`.
    *   **`undo()`** : Remplace le nouveau texte par le texte original. La commande devra mémoriser le texte original et sa position.

#### **Étape 4 : L'Invoker (Invoker) - Le Gestionnaire d'Historique**

Créez une classe `CommandProcessor` (ou `HistoryManager`). C'est l'objet qui va gérer l'exécution des commandes et l'historique pour l'undo/redo.

*   Elle doit contenir deux piles (stacks) :
    *   `undoStack` : Pour stocker les commandes qui ont été exécutées et peuvent être annulées.
    *   `redoStack` : Pour stocker les commandes qui ont été annulées et peuvent être rétablies.
*   Implémentez les méthodes suivantes :
    *   `void executeCommand(Command command)` :
        *   Appelle `command.execute()`.
        *   Ajoute la commande à `undoStack`.
        *   **Important :** Si une nouvelle commande est exécutée, la `redoStack` doit être vidée, car l'historique des "redo" n'est plus valide.
    *   `void undo()` :
        *   Si `undoStack` n'est pas vide :
            *   Dépile la dernière commande.
            *   Appelle `command.undo()`.
            *   Empile cette commande sur `redoStack`.
    *   `void redo()` :
        *   Si `redoStack` n'est pas vide :
            *   Dépile la dernière commande.
            *   Appelle `command.execute()` (oui, `execute()` pour le redo, car on refait l'action).
            *   Empile cette commande sur `undoStack`.

#### **Étape 5 : Test et Validation**

Créez une classe `Main` (ou un script de test) pour simuler l'utilisation de votre éditeur.

1.  Instanciez un `Document` et un `CommandProcessor`.
2.  Exécutez une séquence d'opérations :
    *   Insérer "Bonjour " au début.
    *   Insérer "monde !" à la fin.
    *   Supprimer "monde".
    *   Insérer "le monde !" à la place.
3.  Affichez le contenu du document après chaque étape significative.
4.  Effectuez plusieurs `undo()` et `redo()` pour vérifier le bon fonctionnement.
    *   Exemple : `undo()`, `undo()`, `redo()`, `redo()`, puis une nouvelle commande (`insertText`), puis tentez un `redo()` (qui ne devrait rien faire).

---

**Consignes Générales :**

*   **Langage :** Choisissez votre langage de prédilection (Java, C#, Python, TypeScript, etc.). Les concepts restent les mêmes.
*   **Clarté du code :** Veillez à la lisibilité, aux noms de variables et de méthodes explicites.
*   **Gestion des erreurs :** Pour ce TP, une gestion basique des erreurs (ex: index out of bounds) est suffisante, mais vous pouvez l'améliorer si vous le souhaitez.
*   **Utilisation de l'IA :** N'hésitez pas à utiliser des outils d'IA pour générer des ébauches de code, des explications sur des concepts ou pour débugger. Cependant, l'objectif est que *vous* compreniez et maîtrisiez le pattern. Utilisez l'IA comme un assistant, pas comme un substitut à votre réflexion. Posez-vous la question : "Pourquoi l'IA a-t-elle fait ce choix ? Est-ce la meilleure approche ? Comment fonctionne réellement cette partie du code ?" Concentrez-vous particulièrement sur la logique d'undo/redo et la gestion des piles, c'est là que la compréhension humaine est cruciale.

Bon courage pour ce TP !