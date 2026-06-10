Bonjour !

Ce TP vous guidera dans l'implémentation du Design Pattern **Adapter**. L'objectif est de comprendre comment ce pattern permet de faire collaborer des interfaces incompatibles, une situation très fréquente dans le développement logiciel, notamment lors de l'intégration de bibliothèques tierces ou de systèmes existants.

---

### **TP : Implémenter un Adapter pour une source de données externe**

**Objectif du TP :** Implémenter le pattern Adapter pour rendre une interface de service externe compatible avec une interface de référentiel de données interne à notre application.

**Contexte :**
Votre application gère des utilisateurs et attend une interface `IUserRepository` pour récupérer ces données. Cependant, une nouvelle source de données externe est disponible via un `ExternalCustomerService` qui expose des méthodes et des objets différents. Plutôt que de modifier toute votre application pour s'adapter à ce nouveau service, vous allez créer un Adapter.

**Prérequis :**
*   Connaissances de base en programmation orientée objet (classes, interfaces, héritage, composition).
*   Maîtrise d'un langage de programmation orienté objet (C#, Java, Python, TypeScript, etc.). Les exemples seront donnés dans un style générique, facilement transposable.

---

### **Énoncé de l'exercice**

**Partie 1 : Définition des interfaces et modèles existants**

Imaginez que votre application utilise les modèles et interfaces suivants :

```
// Modèle de données attendu par votre application
class User {
    int Id;
    string Nom;
    string Email;
}

// Interface de référentiel attendue par votre application
interface IUserRepository {
    User GetUserById(int id);
    List<User> GetAllUsers();
}
```

Et voici le service externe que vous devez intégrer :

```
// Modèle de données fourni par le service externe
class Customer {
    string CustomerId; // Notez que c'est un string ici
    string FullName;
    string ContactEmail;
}

// Service externe (l'Adaptee)
class ExternalCustomerService {
    // Simule une base de données ou un appel API
    private List<Customer> _customers = new List<Customer> {
        new Customer { CustomerId = "cust001", FullName = "Alice Dupont", ContactEmail = "alice.d@example.com" },
        new Customer { CustomerId = "cust002", FullName = "Bob Martin", ContactEmail = "bob.m@example.com" },
        new Customer { CustomerId = "cust003", FullName = "Charlie Leblanc", ContactEmail = "charlie.l@example.com" }
    };

    public Customer FetchCustomerData(string customerId) {
        Console.WriteLine($"[ExternalService] Fetching customer with ID: {customerId}");
        return _customers.FirstOrDefault(c => c.CustomerId == customerId);
    }

    public List<Customer> RetrieveAllCustomers() {
        Console.WriteLine("[ExternalService] Retrieving all customers.");
        return new List<Customer>(_customers); // Retourne une copie
    }
}
```

**Votre mission :** Créer un `ExternalCustomerServiceAdapter` qui implémente `IUserRepository` et utilise `ExternalCustomerService` en interne.

---

**Partie 2 : Implémentation de l'Adapter**

1.  **Créez la classe `ExternalCustomerServiceAdapter`.**
    *   Cette classe doit implémenter l'interface `IUserRepository`.
    *   Elle doit contenir une instance de `ExternalCustomerService` (par composition). Injectez cette instance via le constructeur de l'adapter.

2.  **Implémentez la méthode `GetUserById(int id)` :**
    *   Dans cette méthode, vous devrez appeler `FetchCustomerData` du `ExternalCustomerService`.
    *   **Attention :** Le `ExternalCustomerService` attend un `string` pour l'ID, tandis que `GetUserById` reçoit un `int`. Vous devrez gérer cette conversion (par exemple, `id.ToString()`).
    *   Une fois le `Customer` récupéré, convertissez-le en un objet `User` et retournez-le. Si aucun `Customer` n'est trouvé, retournez `null` ou levez une exception appropriée.

3.  **Implémentez la méthode `GetAllUsers()` :**
    *   Dans cette méthode, vous devrez appeler `RetrieveAllCustomers()` du `ExternalCustomerService`.
    *   Parcourez la liste des `Customer`s retournés et convertissez chacun d'eux en un objet `User`.
    *   Retournez une liste de `User`s.

---

**Partie 3 : Test et validation**

Créez une méthode `Main` (ou un script de test) pour valider votre Adapter :

1.  Instanciez `ExternalCustomerService`.
2.  Instanciez votre `ExternalCustomerServiceAdapter` en lui passant l'instance du service externe.
3.  Utilisez l'adapter comme si c'était un `IUserRepository` :
    *   Appelez `GetUserById` avec un ID valide (ex: 1 pour "cust001") et un ID non valide. Affichez les résultats.
    *   Appelez `GetAllUsers` et affichez la liste complète des utilisateurs.

**Exemple d'utilisation attendue :**

```
// Dans votre méthode Main ou votre client
IUserRepository userRepository = new ExternalCustomerServiceAdapter(new ExternalCustomerService());

// Test de GetUserById
User user1 = userRepository.GetUserById(1); // Devrait correspondre à cust001
if (user1 != null) {
    Console.WriteLine($"Utilisateur trouvé: ID={user1.Id}, Nom={user1.Nom}, Email={user1.Email}");
} else {
    Console.WriteLine("Utilisateur non trouvé.");
}

User userInvalid = userRepository.GetUserById(99);
if (userInvalid == null) {
    Console.WriteLine("Utilisateur 99 non trouvé (comportement attendu).");
}

// Test de GetAllUsers
List<User> allUsers = userRepository.GetAllUsers();
Console.WriteLine("\nTous les utilisateurs:");
foreach (var user in allUsers) {
    Console.WriteLine($"  ID={user.Id}, Nom={user.Nom}, Email={user.Email}");
}
```

---

**Partie 4 : Réflexion et utilisation de l'IA**

L'IA est un outil puissant pour générer du code. N'hésitez pas à l'utiliser pour vous aider dans ce TP. Cependant, le but n'est pas de copier-coller sans comprendre.

1.  **Génération initiale :** Vous pouvez demander à une IA de générer la structure de l'Adapter ou même une première ébauche des méthodes de conversion.
2.  **Analyse critique :**
    *   Une fois le code généré, prenez le temps de l'analyser. L'IA a-t-elle fait les bons choix pour la conversion des IDs (`int` vers `string`) ?
    *   Comment l'IA a-t-elle géré les cas où un utilisateur n'est pas trouvé ? Est-ce le comportement que vous attendez ?
    *   Y a-t-il des améliorations à apporter au code généré (lisibilité, robustesse, gestion des erreurs) ?
3.  **Explication :** Expliquez avec vos propres mots pourquoi le pattern Adapter est pertinent dans ce scénario. Quels sont les avantages de cette approche par rapport à une modification directe de l'interface `IUserRepository` ou du `ExternalCustomerService` ?
4.  **Autres cas d'usage :** Citez au moins deux autres scénarios réels où le pattern Adapter serait utile.

---

**Bonus (pour les plus rapides ou curieux) :**

*   **Gestion des erreurs :** Améliorez l'Adapter pour qu'il gère des exceptions spécifiques si le service externe retourne une erreur ou si la conversion échoue.
*   **Mappage plus complexe :** Imaginez que le `Customer` ait des champs supplémentaires non présents dans `User` (ex: `Address`, `PhoneNumber`). Comment géreriez-vous cela dans l'Adapter ?
*   **Adapter générique :** Pourrait-on créer un Adapter plus générique capable de convertir n'importe quel `SourceType` en `TargetType` en utilisant des fonctions de mappage ? (Ceci est un défi plus avancé, mais intéressant pour la réflexion).

---

Bon courage pour ce TP ! C'est une excellente occasion de pratiquer un pattern fondamental qui simplifie grandement l'intégration et la maintenance des systèmes.