Voici une proposition de solution pour le TP sur le Design Pattern Proxy, en utilisant Java pour les exemples de code.

---

# Solution du TP : Contrôler l'accès à un service avec un Proxy

## Partie 1 : Le Service de Rapports Réel (Subject et RealSubject)

### 1.1. Interface `IReportingService` (Subject)

Cette interface définit le contrat que le service réel et le proxy doivent respecter.




```java
// Fichier: IReportingService.java
public interface IReportingService {
    String generateReport(String reportId, String userId);
}
```



### 1.2. Implémentation de la classe `RealReportingService` (RealSubject)

Cette classe simule le service coûteux en temps et en ressources.




```java
// Fichier: RealReportingService.java
public class RealReportingService implements IReportingService {
    @Override
    public String generateReport(String reportId, String userId) {
        System.out.println($"[RealReportingService] Démarrage de la génération du rapport '{reportId}' pour l'utilisateur '{userId}'...");
        try {
            // Simulation d'une opération longue
            Thread.sleep(2500); // 2.5 secondes
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            System.err.println("La génération du rapport a été interrompue.");
        }
        String reportContent = $"Rapport '{reportId}' généré par le service réel pour l'utilisateur '{userId}'.";
        System.out.println($"[RealReportingService] Rapport '{reportId}' généré avec succès.");
        return reportContent;
    }
}
```



### 1.3. Test initial

Un test simple pour vérifier le fonctionnement du service réel.




```java
// Fichier: Main.java (partie test initial)
public class Main {
    public static void main(String[] args) {
        System.out.println("--- Test initial du RealReportingService ---");
        IReportingService realService = new RealReportingService();
        String report = realService.generateReport("100", "testUser");
        System.out.println("Rapport obtenu: " + report);
        System.out.println("-------------------------------------------\n");

        // Les autres tests seront ajoutés ici
    }
}
```



**Sortie attendue du test initial :**



```
--- Test initial du RealReportingService ---
[RealReportingService] Démarrage de la génération du rapport '100' pour l'utilisateur 'testUser'...
(pause de 2.5 secondes)
[RealReportingService] Rapport '100' généré avec succès.
Rapport obtenu: Rapport '100' généré par le service réel pour l'utilisateur 'testUser'.
-------------------------------------------
```



## Partie 2 : Le Proxy de Contrôle (Proxy)

### 2.1. Implémentation de la classe `ReportingServiceProxy`

Cette classe implémente `IReportingService` et contient la logique de contrôle.




```java
// Fichier: ReportingServiceProxy.java
import java.util.HashMap;
import java.util.Map;
import java.util.Set;
import java.util.HashSet;
import java.util.Arrays;

public class ReportingServiceProxy implements IReportingService {
    private IReportingService realService; // Référence au service réel (Adaptee)
    private Map<String, String> cache; // Cache pour les rapports
    private Set<String> authorizedUsersForReport101; // Utilisateurs autorisés pour le rapport 101

    public ReportingServiceProxy(IReportingService realService) {
        this.realService = realService;
        this.cache = new HashMap<>();
        this.authorizedUsersForReport101 = new HashSet<>(Arrays.asList("admin", "manager"));
    }

    @Override
    public String generateReport(String reportId, String userId) {
        System.out.println($"[ReportingServiceProxy] Interception de la demande pour le rapport '{reportId}' par l'utilisateur '{userId}'.");

        // 1. Contrôle d'accès (Protection Proxy)
        if ("101".equals(reportId) && !authorizedUsersForReport101.contains(userId)) {
            System.out.println($"[ReportingServiceProxy] ACCÈS REFUSÉ: L'utilisateur '{userId}' n'est pas autorisé à accéder au rapport '{reportId}'.");
            throw new SecurityException($"Accès non autorisé au rapport '{reportId}'.");
            // Ou retourner un message d'erreur spécifique, selon la politique
            // return "Erreur: Accès non autorisé.";
        }

        // 2. Mise en cache (Virtual Proxy / Caching Proxy)
        if (cache.containsKey(reportId)) {
            System.out.println($"[ReportingServiceProxy] Rapport '{reportId}' servi depuis le cache.");
            return cache.get(reportId);
        }

        // Si pas en cache et accès autorisé, appeler le service réel
        String reportContent = realService.generateReport(reportId, userId);

        // Mettre le rapport en cache après génération
        cache.put(reportId, reportContent);
        System.out.println($"[ReportingServiceProxy] Rapport '{reportId}' mis en cache.");

        return reportContent;
    }
}
```



## Partie 3 : Intégration et Tests

### 3.1. Utilisation du Proxy

Le client interagit uniquement avec le proxy.

### 3.2. Scénarios de test

**Modification de la classe `Main.java`**




```java
// Fichier: Main.java (version complète)
public class Main {
    public static void main(String[] args) {
        System.out.println("--- Test initial du RealReportingService ---");
        IReportingService realService = new RealReportingService();
        String report = realService.generateReport("100", "testUser");
        System.out.println("Rapport obtenu: " + report);
        System.out.println("-------------------------------------------\n");

        System.out.println("--- Utilisation du ReportingServiceProxy ---");
        IReportingService proxyService = new ReportingServiceProxy(new RealReportingService());

        // Scénario 1: Accès autorisé à un rapport non restreint
        System.out.println("\n--- Scénario 1: Accès autorisé (rapport 102, user1) ---");
        String report102_user1 = proxyService.generateReport("102", "user1");
        System.out.println("Rapport obtenu: " + report102_user1);

        // Scénario 2: Accès refusé à un rapport restreint
        System.out.println("\n--- Scénario 2: Accès refusé (rapport 101, user2) ---");
        try {
            proxyService.generateReport("101", "user2");
        } catch (SecurityException e) {
            System.out.println("Erreur d'accès: " + e.getMessage());
        }

        // Scénario 3: Accès autorisé à un rapport restreint
        System.out.println("\n--- Scénario 3: Accès autorisé (rapport 101, admin) ---");
        String report101_admin = proxyService.generateReport("101", "admin");
        System.out.println("Rapport obtenu: " + report101_admin);

        // Scénario 4: Test de cache (deuxième appel pour rapport 102, user1)
        System.out.println("\n--- Scénario 4: Test de cache (rapport 102, user1 - deuxième appel) ---");
        String report102_user1_cached = proxyService.generateReport("102", "user1");
        System.out.println("Rapport obtenu: " + report102_user1_cached);

        // Scénario 5: Test de cache (deuxième appel pour rapport 101, admin)
        System.out.println("\n--- Scénario 5: Test de cache (rapport 101, admin - deuxième appel) ---");
        String report101_admin_cached = proxyService.generateReport("101", "admin");
        System.out.println("Rapport obtenu: " + report101_admin_cached);
    }
}
```



### Sortie console complète (avec les pauses)



```
--- Test initial du RealReportingService ---
[RealReportingService] Démarrage de la génération du rapport '100' pour l'utilisateur 'testUser'...
(pause 2.5s)
[RealReportingService] Rapport '100' généré avec succès.
Rapport obtenu: Rapport '100' généré par le service réel pour l'utilisateur 'testUser'.
-------------------------------------------

--- Utilisation du ReportingServiceProxy ---

--- Scénario 1: Accès autorisé (rapport 102, user1) ---
[ReportingServiceProxy] Interception de la demande pour le rapport '102' par l'utilisateur 'user1'.
[RealReportingService] Démarrage de la génération du rapport '102' pour l'utilisateur 'user1'...
(pause 2.5s)
[RealReportingService] Rapport '102' généré avec succès.
[ReportingServiceProxy] Rapport '102' mis en cache.
Rapport obtenu: Rapport '102' généré par le service réel pour l'utilisateur 'user1'.

--- Scénario 2: Accès refusé (rapport 101, user2) ---
[ReportingServiceProxy] Interception de la demande pour le rapport '101' par l'utilisateur 'user2'.
[ReportingServiceProxy] ACCÈS REFUSÉ: L'utilisateur 'user2' n'est pas autorisé à accéder au rapport '101'.
Erreur d'accès: Accès non autorisé au rapport '101'.

--- Scénario 3: Accès autorisé (rapport 101, admin) ---
[ReportingServiceProxy] Interception de la demande pour le rapport '101' par l'utilisateur 'admin'.
[RealReportingService] Démarrage de la génération du rapport '101' pour l'utilisateur 'admin'...
(pause 2.5s)
[RealReportingService] Rapport '101' généré avec succès.
[ReportingServiceProxy] Rapport '101' mis en cache.
Rapport obtenu: Rapport '101' généré par le service réel pour l'utilisateur 'admin'.

--- Scénario 4: Test de cache (rapport 102, user1 - deuxième appel) ---
[ReportingServiceProxy] Interception de la demande pour le rapport '102' par l'utilisateur 'user1'.
[ReportingServiceProxy] Rapport '102' servi depuis le cache.
Rapport obtenu: Rapport '102' généré par le service réel pour l'utilisateur 'user1'.

--- Scénario 5: Test de cache (rapport 101, admin - deuxième appel) ---
[ReportingServiceProxy] Interception de la demande pour le rapport '101' par l'utilisateur 'admin'.
[ReportingServiceProxy] Rapport '101' servi depuis le cache.
Rapport obtenu: Rapport '101' généré par le service réel pour l'utilisateur 'admin'.
```



## Explications et Justifications

Le pattern Proxy a été implémenté avec succès pour ajouter une couche de contrôle autour du `RealReportingService`.

*   **`IReportingService` (Subject) :** C'est l'interface commune qui permet au `RealReportingService` et au `ReportingServiceProxy` d'être interchangeables. Le client interagit avec cette interface, sans savoir s'il parle au service réel ou à son proxy.
*   **`RealReportingService` (RealSubject) :** C'est l'objet coûteux et sensible que nous voulons protéger et optimiser. Il contient la logique métier de génération de rapports.
*   **`ReportingServiceProxy` (Proxy) :** C'est le cœur du pattern. Il implémente la même interface que le service réel, ce qui lui permet de se substituer à lui. Il contient une référence au `RealReportingService` et délègue les appels à ce dernier après avoir appliqué sa propre logique :
    *   **Journalisation :** Chaque demande est loguée, ce qui permet de tracer les accès.
    *   **Contrôle d'accès (Protection Proxy) :** Le proxy vérifie les droits de l'utilisateur avant de permettre l'accès à certains rapports. Si l'utilisateur n'est pas autorisé, le service réel n'est jamais appelé, économisant des ressources et protégeant les données.
    *   **Mise en cache (Virtual Proxy / Caching Proxy) :** Le proxy stocke les rapports déjà générés. Si une demande pour un rapport déjà en cache est reçue, le proxy le retourne immédiatement sans appeler le service réel, ce qui améliore considérablement les performances pour les requêtes répétées.

**Avantages du Proxy dans ce scénario :**

*   **Découplage :** Le client est découplé des préoccupations transversales (sécurité, cache, journalisation). Il se contente de demander un rapport.
*   **Protection :** Le proxy protège le service réel des accès non autorisés.
*   **Optimisation :** Le cache du proxy réduit la charge sur le service réel et améliore la réactivité de l'application.
*   **Transparence :** Pour le client, l'utilisation du proxy est transparente ; il appelle la même interface.
*   **Flexibilité :** Les préoccupations de sécurité, de cache ou de journalisation peuvent être modifiées ou ajoutées sans toucher au service réel.

Ce TP démontre comment le pattern Proxy permet d'ajouter des fonctionnalités de manière non intrusive, en enveloppant un objet existant pour en contrôler l'accès ou en améliorer le comportement.