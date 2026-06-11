Voici une proposition de solution pour le TP sur les Design Patterns Iterator et Chain of Responsibility.

---

# Solution du TP : Gestion de Requêtes de Support Client avec Iterator et Chain of Responsibility

## Étape 1 : La Requête (Request/Ticket)

Nous allons définir des énumérations pour les types et priorités de requêtes afin de rendre le code plus lisible et robuste.

### Énumération `RequestType`



```java
// Fichier: RequestType.java
public enum RequestType {
    TECHNIQUE,
    FACTURATION,
    GENERALE
}
```



### Énumération `Priority`



```java
// Fichier: Priority.java
public enum Priority {
    BASSE,
    MOYENNE,
    HAUTE
}
```



### Classe `Request`



```java
// Fichier: Request.java
public class Request {
    private int id;
    private String description;
    private RequestType type;
    private Priority priority;

    public Request(int id, String description, RequestType type, Priority priority) {
        this.id = id;
        this.description = description;
        this.type = type;
        this.priority = priority;
    }

    public int getId() {
        return id;
    }

    public String getDescription() {
        return description;
    }

    public RequestType getType() {
        return type;
    }

    public Priority getPriority() {
        return priority;
    }

    @Override
    public String toString() {
        return "Request #" + id + " [" + type + ", " + priority + "]: '" + description + "'";
    }
}
```



## Étape 2 : L'Itérateur de Requêtes

### Interface `RequestIterator`



```java
// Fichier: RequestIterator.java
public interface RequestIterator {
    boolean hasNext();
    Request next();
}
```



### Classe `RequestCollection` (l'agrégat)



```java
// Fichier: RequestCollection.java
import java.util.ArrayList;
import java.util.List;

public class RequestCollection {
    private List<Request> requests = new ArrayList<>();

    public void addRequest(Request request) {
        requests.add(request);
    }

    public RequestIterator createIterator() {
        return new ConcreteRequestIterator(this);
    }

    // Méthode pour que l'itérateur puisse accéder à la liste interne
    public List<Request> getRequests() {
        return requests;
    }
}
```



### Classe `ConcreteRequestIterator`



```java
// Fichier: ConcreteRequestIterator.java
import java.util.List;

public class ConcreteRequestIterator implements RequestIterator {
    private RequestCollection collection;
    private int position = 0;

    public ConcreteRequestIterator(RequestCollection collection) {
        this.collection = collection;
    }

    @Override
    public boolean hasNext() {
        return position < collection.getRequests().size();
    }

    @Override
    public Request next() {
        if (hasNext()) {
            return collection.getRequests().get(position++);
        }
        return null; // Ou lever une NoSuchElementException
    }
}
```



## Étape 3 : La Chaîne de Responsabilité des Gestionnaires

### Classe abstraite `SupportHandler`



```java
// Fichier: SupportHandler.java
public abstract class SupportHandler {
    protected SupportHandler nextHandler;

    public void setNextHandler(SupportHandler nextHandler) {
        this.nextHandler = nextHandler;
    }

    public abstract void handleRequest(Request request);
}
```



### Implémentations concrètes des gestionnaires

#### `GeneralSupportHandler`



```java
// Fichier: GeneralSupportHandler.java
public class GeneralSupportHandler extends SupportHandler {
    @Override
    public void handleRequest(Request request) {
        if (request.getType() == RequestType.GENERALE &&
            (request.getPriority() == Priority.BASSE || request.getPriority() == Priority.MOYENNE)) {
            System.out.println($"[GeneralSupportHandler] Traite la requête #{request.getId()}: {request.getDescription()}");
        } else if (nextHandler != null) {
            System.out.println($"[GeneralSupportHandler] Ne peut pas traiter la requête #{request.getId()}. Transmet à {nextHandler.getClass().getSimpleName()}.");
            nextHandler.handleRequest(request);
        } else {
            // Cas où il n'y a pas de gestionnaire suivant (devrait être géré par UnresolvedRequestLogger)
            System.out.println($"[GeneralSupportHandler] Ne peut pas traiter la requête #{request.getId()} et pas de gestionnaire suivant.");
        }
    }
}
```



#### `TechnicalSupportHandler`



```java
// Fichier: TechnicalSupportHandler.java
public class TechnicalSupportHandler extends SupportHandler {
    @Override
    public void handleRequest(Request request) {
        if (request.getType() == RequestType.TECHNIQUE &&
            (request.getPriority() == Priority.MOYENNE || request.getPriority() == Priority.HAUTE)) {
            System.out.println($"[TechnicalSupportHandler] Traite la requête #{request.getId()}: {request.getDescription()}");
        } else if (nextHandler != null) {
            System.out.println($"[TechnicalSupportHandler] Ne peut pas traiter la requête #{request.getId()}. Transmet à {nextHandler.getClass().getSimpleName()}.");
            nextHandler.handleRequest(request);
        } else {
            System.out.println($"[TechnicalSupportHandler] Ne peut pas traiter la requête #{request.getId()} et pas de gestionnaire suivant.");
        }
    }
}
```



#### `BillingSupportHandler`



```java
// Fichier: BillingSupportHandler.java
public class BillingSupportHandler extends SupportHandler {
    @Override
    public void handleRequest(Request request) {
        if (request.getType() == RequestType.FACTURATION &&
            (request.getPriority() == Priority.MOYENNE || request.getPriority() == Priority.HAUTE)) {
            System.out.println($"[BillingSupportHandler] Traite la requête #{request.getId()}: {request.getDescription()}");
        } else if (nextHandler != null) {
            System.out.println($"[BillingSupportHandler] Ne peut pas traiter la requête #{request.getId()}. Transmet à {nextHandler.getClass().getSimpleName()}.");
            nextHandler.handleRequest(request);
        } else {
            System.out.println($"[BillingSupportHandler] Ne peut pas traiter la requête #{request.getId()} et pas de gestionnaire suivant.");
        }
    }
}
```



#### `UnresolvedRequestLogger`



```java
// Fichier: UnresolvedRequestLogger.java
public class UnresolvedRequestLogger extends SupportHandler {
    @Override
    public void handleRequest(Request request) {
        System.out.println($"[UnresolvedRequestLogger] La requête #{request.getId()} n'a pas pu être traitée par la chaîne: {request.getDescription()}");
        // Ici, on pourrait logger dans un fichier, envoyer un email à un administrateur, etc.
    }
}
```



## Étape 4 : Intégration et Simulation

### Classe `Main`



```java
// Fichier: Main.java
public class Main {
    public static void main(String[] args) {
        // 1. Création de la collection de requêtes
        RequestCollection requestCollection = new RequestCollection();

        // 2. Ajout de requêtes variées
        requestCollection.addRequest(new Request(101, "Problème de connexion internet", RequestType.TECHNIQUE, Priority.HAUTE));
        requestCollection.addRequest(new Request(102, "Demande de remboursement", RequestType.FACTURATION, Priority.MOYENNE));
        requestCollection.addRequest(new Request(103, "Question sur les horaires d'ouverture", RequestType.GENERALE, Priority.BASSE));
        requestCollection.addRequest(new Request(104, "Bug dans l'application mobile", RequestType.TECHNIQUE, Priority.MOYENNE));
        requestCollection.addRequest(new Request(105, "Facture incorrecte", RequestType.FACTURATION, Priority.HAUTE));
        requestCollection.addRequest(new Request(106, "Suggestion d'amélioration", RequestType.GENERALE, Priority.HAUTE)); // Non traitée par General
        requestCollection.addRequest(new Request(107, "Problème de paiement", RequestType.FACTURATION, Priority.BASSE)); // Non traitée par Billing

        // 3. Création des instances des gestionnaires
        GeneralSupportHandler generalHandler = new GeneralSupportHandler();
        TechnicalSupportHandler technicalHandler = new TechnicalSupportHandler();
        BillingSupportHandler billingHandler = new BillingSupportHandler();
        UnresolvedRequestLogger unresolvedLogger = new UnresolvedRequestLogger();

        // 4. Construction de la chaîne de responsabilité
        generalHandler.setNextHandler(technicalHandler);
        technicalHandler.setNextHandler(billingHandler);
        billingHandler.setNextHandler(unresolvedLogger); // Le dernier maillon logue les requêtes non résolues

        System.out.println("--- Démarrage du traitement des requêtes ---");

        // 5. Parcours des requêtes avec l'itérateur et passage à la chaîne
        RequestIterator iterator = requestCollection.createIterator();
        while (iterator.hasNext()) {
            Request request = iterator.next();
            System.out.println($"\nTraitement de: {request}");
            generalHandler.handleRequest(request); // On passe la requête au premier gestionnaire de la chaîne
        }

        System.out.println("\n--- Fin du traitement des requêtes ---");
    }
}
```



### Sortie console de l'exécution du programme



```
--- Démarrage du traitement des requêtes ---

Traitement de: Request #101 [TECHNIQUE, HAUTE]: 'Problème de connexion internet'
[GeneralSupportHandler] Ne peut pas traiter la requête #101. Transmet à TechnicalSupportHandler.
[TechnicalSupportHandler] Traite la requête #101: Problème de connexion internet

Traitement de: Request #102 [FACTURATION, MOYENNE]: 'Demande de remboursement'
[GeneralSupportHandler] Ne peut pas traiter la requête #102. Transmet à TechnicalSupportHandler.
[TechnicalSupportHandler] Ne peut pas traiter la requête #102. Transmet à BillingSupportHandler.
[BillingSupportHandler] Traite la requête #102: Demande de remboursement

Traitement de: Request #103 [GENERALE, BASSE]: 'Question sur les horaires d'ouverture'
[GeneralSupportHandler] Traite la requête #103: Question sur les horaires d'ouverture

Traitement de: Request #104 [TECHNIQUE, MOYENNE]: 'Bug dans l'application mobile'
[GeneralSupportHandler] Ne peut pas traiter la requête #104. Transmet à TechnicalSupportHandler.
[TechnicalSupportHandler] Traite la requête #104: Bug dans l'application mobile

Traitement de: Request #105 [FACTURATION, HAUTE]: 'Facture incorrecte'
[GeneralSupportHandler] Ne peut pas traiter la requête #105. Transmet à TechnicalSupportHandler.
[TechnicalSupportHandler] Ne peut pas traiter la requête #105. Transmet à BillingSupportHandler.
[BillingSupportHandler] Traite la requête #105: Facture incorrecte

Traitement de: Request #106 [GENERALE, HAUTE]: 'Suggestion d'amélioration'
[GeneralSupportHandler] Ne peut pas traiter la requête #106. Transmet à TechnicalSupportHandler.
[TechnicalSupportHandler] Ne peut pas traiter la requête #106. Transmet à BillingSupportHandler.
[BillingSupportHandler] Ne peut pas traiter la requête #106. Transmet à UnresolvedRequestLogger.
[UnresolvedRequestLogger] La requête #106 n'a pas pu être traitée par la chaîne: Suggestion d'amélioration

Traitement de: Request #107 [FACTURATION, BASSE]: 'Problème de paiement'
[GeneralSupportHandler] Ne peut pas traiter la requête #107. Transmet à TechnicalSupportHandler.
[TechnicalSupportHandler] Ne peut pas traiter la requête #107. Transmet à BillingSupportHandler.
[BillingSupportHandler] Ne peut pas traiter la requête #107. Transmet à UnresolvedRequestLogger.
[UnresolvedRequestLogger] La requête #107 n'a pas pu être traitée par la chaîne: Problème de paiement

--- Fin du traitement des requêtes ---
```