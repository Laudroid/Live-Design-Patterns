Voici une proposition de solution pour le TP sur le Design Pattern Adapter, en utilisant Java pour les exemples de code.

---

# Solution du TP : Implémenter un Adapter pour une source de données externe

## Partie 1 : Définition des interfaces et modèles existants

### Modèles et interfaces de l'application



```java
// Fichier: User.java
public class User {
    private int id;
    private String nom;
    private String email;

    public User(int id, String nom, String email) {
        this.id = id;
        this.nom = nom;
        this.email = email;
    }

    // Getters
    public int getId() { return id; }
    public String getNom() { return nom; }
    public String getEmail() { return email; }

    @Override
    public String toString() {
        return "User{id=" + id + ", nom='" + nom + "', email='" + email + "'}";
    }
}
```





```java
// Fichier: IUserRepository.java
import java.util.List;

public interface IUserRepository {
    User getUserById(int id);
    List<User> getAllUsers();
}
```



### Service externe (l'Adaptee)



```java
// Fichier: Customer.java
public class Customer {
    private String customerId; // Notez que c'est un string ici
    private String fullName;
    private String contactEmail;

    public Customer(String customerId, String fullName, String contactEmail) {
        this.customerId = customerId;
        this.fullName = fullName;
        this.contactEmail = contactEmail;
    }

    // Getters
    public String getCustomerId() { return customerId; }
    public String getFullName() { return fullName; }
    public String getContactEmail() { return contactEmail; }

    @Override
    public String toString() {
        return "Customer{customerId='" + customerId + "', fullName='" + fullName + "', contactEmail='" + contactEmail + "'}";
    }
}
```





```java
// Fichier: ExternalCustomerService.java
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

public class ExternalCustomerService {
    // Simule une base de données ou un appel API
    private List<Customer> customers = new ArrayList<>();

    public ExternalCustomerService() {
        customers.add(new Customer("cust001", "Alice Dupont", "alice.d@example.com"));
        customers.add(new Customer("cust002", "Bob Martin", "bob.m@example.com"));
        customers.add(new Customer("cust003", "Charlie Leblanc", "charlie.l@example.com"));
    }

    public Customer fetchCustomerData(String customerId) {
        System.out.println($"[ExternalService] Fetching customer with ID: {customerId}");
        Optional<Customer> foundCustomer = customers.stream()
                                                    .filter(c -> c.getCustomerId().equals(customerId))
                                                    .findFirst();
        return foundCustomer.orElse(null);
    }

    public List<Customer> retrieveAllCustomers() {
        System.out.println("[ExternalService] Retrieving all customers.");
        return new ArrayList<>(customers); // Retourne une copie pour éviter les modifications externes
    }
}
```



## Partie 2 : Implémentation de l'Adapter

### Classe `ExternalCustomerServiceAdapter`



```java
// Fichier: ExternalCustomerServiceAdapter.java
import java.util.List;
import java.util.stream.Collectors;

public class ExternalCustomerServiceAdapter implements IUserRepository {
    private ExternalCustomerService externalService;

    // Injection de l'Adaptee via le constructeur
    public ExternalCustomerServiceAdapter(ExternalCustomerService externalService) {
        this.externalService = externalService;
    }

    // Méthode utilitaire pour convertir un Customer en User
    private User convertCustomerToUser(Customer customer) {
        if (customer == null) {
            return null;
        }
        // Conversion de l'ID string en int.
        // On suppose ici que "cust001" -> 1, "cust002" -> 2, etc.
        // Une logique plus robuste serait nécessaire pour des IDs arbitraires.
        int id = Integer.parseInt(customer.getCustomerId().replace("cust", ""));
        return new User(id, customer.getFullName(), customer.getContactEmail());
    }

    @Override
    public User getUserById(int id) {
        // Conversion de l'ID int en string pour le service externe
        String customerId = "cust" + String.format("%03d", id); // ex: 1 -> "cust001"
        Customer customer = externalService.fetchCustomerData(customerId);
        return convertCustomerToUser(customer);
    }

    @Override
    public List<User> getAllUsers() {
        List<Customer> customers = externalService.retrieveAllCustomers();
        // Conversion de chaque Customer en User
        return customers.stream()
                        .map(this::convertCustomerToUser)
                        .collect(Collectors.toList());
    }
}
```



## Partie 3 : Test et validation

### Classe `Main` pour les tests



```java
// Fichier: Main.java
import java.util.List;

public class Main {
    public static void main(String[] args) {
        // 1. Instancier ExternalCustomerService
        ExternalCustomerService externalService = new ExternalCustomerService();

        // 2. Instancier l'Adapter en lui passant le service externe
        IUserRepository userRepository = new ExternalCustomerServiceAdapter(externalService);

        System.out.println("--- Test de GetUserById ---");
        // Test de GetUserById avec un ID valide (ex: 1 pour "cust001")
        User user1 = userRepository.getUserById(1);
        if (user1 != null) {
            System.out.println($"Utilisateur trouvé: ID={user1.getId()}, Nom={user1.getNom()}, Email={user1.getEmail()}");
        } else {
            System.out.println("Utilisateur 1 non trouvé.");
        }

        // Test de GetUserById avec un ID non valide
        User userInvalid = userRepository.getUserById(99);
        if (userInvalid == null) {
            System.out.println("Utilisateur 99 non trouvé (comportement attendu).");
        } else {
            System.out.println($"Utilisateur trouvé par erreur: ID={userInvalid.getId()}, Nom={userInvalid.getNom()}, Email={userInvalid.getEmail()}");
        }

        System.out.println("\n--- Test de GetAllUsers ---");
        // Test de GetAllUsers
        List<User> allUsers = userRepository.getAllUsers();
        System.out.println("Tous les utilisateurs:");
        for (User user : allUsers) {
            System.out.println($"  ID={user.getId()}, Nom={user.getNom()}, Email={user.getEmail()}");
        }
    }
}
```



### Sortie console attendue



```
--- Test de GetUserById ---
[ExternalService] Fetching customer with ID: cust001
Utilisateur trouvé: ID=1, Nom=Alice Dupont, Email=alice.d@example.com
[ExternalService] Fetching customer with ID: cust099
Utilisateur 99 non trouvé (comportement attendu).

--- Test de GetAllUsers ---
[ExternalService] Retrieving all customers.
Tous les utilisateurs:
  ID=1, Nom=Alice Dupont, Email=alice.d@example.com
  ID=2, Nom=Bob Martin, Email=bob.m@example.com
  ID=3, Nom=Charlie Leblanc, Email=charlie.l@example.com
```



## Partie 4 : Réflexion et utilisation de l'IA

### 4.1. Explication de la pertinence du pattern Adapter

Le pattern Adapter est pertinent dans ce scénario car il permet de faire collaborer deux interfaces incompatibles (`IUserRepository` et `ExternalCustomerService`) sans modifier aucune des deux.

*   **Découplage :** L'application cliente continue d'interagir avec l'interface `IUserRepository` qu'elle connaît, sans être consciente des spécificités de `ExternalCustomerService` ou de son modèle `Customer`.
*   **Réutilisabilité :** `ExternalCustomerService` peut être réutilisé par d'autres parties du système qui pourraient avoir besoin de son interface native, et `IUserRepository` peut être implémentée par d'autres sources de données (ex: une base de données interne, un autre service externe via un autre adapter).
*   **Maintenabilité :** Les changements dans l'interface de `ExternalCustomerService` n'affecteraient que l'`ExternalCustomerServiceAdapter`, et non l'ensemble de l'application.
*   **Flexibilité :** Il est facile de "brancher" différentes sources de données en créant de nouveaux adapters pour chaque service externe ou base de données, tant qu'ils implémentent `IUserRepository`.

Sans l'Adapter, il faudrait soit :
1.  Modifier `IUserRepository` et toutes les classes qui l'utilisent pour s'adapter à `ExternalCustomerService` (coûteux, risque de régression).
2.  Modifier `ExternalCustomerService` pour qu'il implémente `IUserRepository` (souvent impossible si c'est une bibliothèque tierce ou un service externe que l'on ne contrôle pas).

L'Adapter agit comme un "traducteur" ou un "enveloppeur" qui convertit les appels et les données d'une interface à l'autre.

### 4.2. Autres cas d'usage du pattern Adapter

1.  **Intégration de bibliothèques graphiques :** Une application existante utilise une ancienne bibliothèque de rendu graphique avec une interface spécifique. Une nouvelle bibliothèque plus performante est disponible, mais avec une interface différente. Un Adapter peut être créé pour que l'application continue d'appeler l'ancienne interface, tandis que l'Adapter traduit ces appels vers la nouvelle bibliothèque.
2.  **Connexion à différentes bases de données :** Une application a une interface `IDatabaseConnector` générique. Pour se connecter à PostgreSQL, MySQL ou Oracle, des Adapters spécifiques (`PostgreSQLAdapter`, `MySQLAdapter`, `OracleAdapter`) peuvent être créés. Chacun implémente `IDatabaseConnector` et utilise les pilotes et APIs spécifiques à sa base de données respective.
