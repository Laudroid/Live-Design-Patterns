Voici une proposition de solution pour le TP sur le Design Pattern Template Method.

---

# Solution du TP : Traitement de Données Hétérogènes avec Template Method

## Partie 1 : Le Squelette de l'Algorithme (La Classe Abstraite)

La classe abstraite `ProcesseurDeDonnees` définit la structure fixe de l'algorithme et les méthodes abstraites pour les étapes variables. Elle gère également l'état des données.





```java
// Fichier: ProcesseurDeDonnees.java
import java.util.List;
import java.util.Map;

public abstract class ProcesseurDeDonnees {
    // État interne pour passer les données entre les étapes
    protected Object donneesBrutes;
    protected List<Map<String, String>> donneesTransformees;
    protected Map<String, Object> resultatsAnalyse;

    // La Template Method : définit l'ordre fixe des étapes
    public final void executerTraitement() {
        notifierDebutTraitement(); // Appel de la méthode hook

        System.out.println("Début du chargement des données...");
        chargerDonnees();
        System.out.println("Chargement terminé.");

        System.out.println("Début de la transformation des données...");
        transformerDonnees();
        System.out.println("Transformation terminée.");

        System.out.println("Début de l'analyse des données...");
        analyserDonnees();
        System.out.println("Analyse terminée.");

        System.out.println("Début de la sauvegarde des résultats...");
        sauvegarderResultats();
        System.out.println("Sauvegarde terminée.");

        notifierFinTraitement(); // Un autre hook optionnel
    }

    // Méthodes abstraites : implémentées par les sous-classes
    protected abstract void chargerDonnees();
    protected abstract void transformerDonnees();
    protected abstract void analyserDonnees();
    protected abstract void sauvegarderResultats();

    // Méthode "Hook" : implémentation par défaut que les sous-classes peuvent surcharger
    protected void notifierDebutTraitement() {
        System.out.println("--- Démarrage du traitement de données générique ---");
    }

    // Autre méthode hook pour la fin du traitement
    protected void notifierFinTraitement() {
        System.out.println("--- Traitement de données terminé ---");
    }
}
```





## Partie 2 : Les Implémentations Concrètes (Les Variations)

### `ProcesseurCSV`





```java
// Fichier: ProcesseurCSV.java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class ProcesseurCSV extends ProcesseurDeDonnees {

    @Override
    protected void notifierDebutTraitement() {
        System.out.println("--- Démarrage du traitement de données CSV ---");
    }

    @Override
    protected void chargerDonnees() {
        System.out.println("[CSV] Simulation de chargement de données depuis un fichier CSV.");
        // Simulation de données CSV brutes
        this.donneesBrutes = "nom,age,ville\nAlice,30,Paris\nBob,24,Lyon\nCharlie,35,Marseille";
        System.out.println("Données brutes chargées: \n" + donneesBrutes);
    }

    @Override
    protected void transformerDonnees() {
        System.out.println("[CSV] Transformation des données CSV en format utilisable.");
        String csvContent = (String) this.donneesBrutes;
        String[] lines = csvContent.split("\n");
        String[] headers = lines[0].split(",");

        this.donneesTransformees = new ArrayList<>();
        for (int i = 1; i < lines.length; i++) {
            String[] values = lines[i].split(",");
            Map<String, String> row = new HashMap<>();
            for (int j = 0; j < headers.length; j++) {
                row.put(headers[j], values[j]);
            }
            this.donneesTransformees.add(row);
        }
        System.out.println("Données transformées: " + donneesTransformees);
    }

    @Override
    protected void analyserDonnees() {
        System.out.println("[CSV] Analyse simple des données transformées (nombre de lignes, âge moyen).");
        this.resultatsAnalyse = new HashMap<>();
        this.resultatsAnalyse.put("nombre_lignes", donneesTransformees.size());

        double totalAge = 0;
        for (Map<String, String> row : donneesTransformees) {
            totalAge += Integer.parseInt(row.get("age"));
        }
        this.resultatsAnalyse.put("age_moyen", totalAge / donneesTransformees.size());
        System.out.println("Résultats de l'analyse: " + resultatsAnalyse);
    }

    @Override
    protected void sauvegarderResultats() {
        System.out.println("[CSV] Simulation de sauvegarde des résultats de l'analyse (affichage console).");
        System.out.println("Rapport CSV final:");
        System.out.println("Nombre de lignes traitées: " + resultatsAnalyse.get("nombre_lignes"));
        System.out.println("Âge moyen des personnes: " + String.format("%.2f", resultatsAnalyse.get("age_moyen")));
    }
}
```





### `ProcesseurBaseDeDonnees`





```java
// Fichier: ProcesseurBaseDeDonnees.java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class ProcesseurBaseDeDonnees extends ProcesseurDeDonnees {

    @Override
    protected void notifierDebutTraitement() {
        System.out.println("--- Démarrage du traitement de données de Base de Données ---");
    }

    @Override
    protected void chargerDonnees() {
        System.out.println("[DB] Simulation de chargement de données depuis une base de données.");
        // Simulation de données brutes (ex: résultats d'une requête SQL)
        List<Map<String, Object>> dbRows = new ArrayList<>();
        dbRows.add(Map.of("id", 1, "product_name", "Laptop", "price", 1200.00, "category", "Electronics"));
        dbRows.add(Map.of("id", 2, "product_name", "Mouse", "price", 25.00, "category", "Electronics"));
        dbRows.add(Map.of("id", 3, "product_name", "Keyboard", "price", 75.00, "category", "Electronics"));
        dbRows.add(Map.of("id", 4, "product_name", "Desk Chair", "price", 300.00, "category", "Furniture"));
        this.donneesBrutes = dbRows;
        System.out.println("Données brutes chargées: " + donneesBrutes);
    }

    @Override
    protected void transformerDonnees() {
        System.out.println("[DB] Transformation des données DB (filtrage par catégorie 'Electronics').");
        List<Map<String, Object>> rawData = (List<Map<String, Object>>) this.donneesBrutes;
        this.donneesTransformees = new ArrayList<>();
        for (Map<String, Object> row : rawData) {
            if ("Electronics".equals(row.get("category"))) {
                Map<String, String> transformedRow = new HashMap<>();
                transformedRow.put("ID", String.valueOf(row.get("id")));
                transformedRow.put("NomProduit", (String) row.get("product_name"));
                transformedRow.put("Prix", String.valueOf(row.get("price")));
                this.donneesTransformees.add(transformedRow);
            }
        }
        System.out.println("Données transformées: " + donneesTransformees);
    }

    @Override
    protected void analyserDonnees() {
        System.out.println("[DB] Analyse des données transformées (calcul du prix moyen des produits électroniques).");
        this.resultatsAnalyse = new HashMap<>();
        double totalPrix = 0;
        for (Map<String, String> row : donneesTransformees) {
            totalPrix += Double.parseDouble(row.get("Prix"));
        }
        this.resultatsAnalyse.put("prix_moyen_electronique", totalPrix / donneesTransformees.size());
        System.out.println("Résultats de l'analyse: " + resultatsAnalyse);
    }

    @Override
    protected void sauvegarderResultats() {
        System.out.println("[DB] Simulation de sauvegarde des résultats (affichage formaté pour DB).");
        System.out.println("SQL pour insertion des résultats:");
        System.out.println($"INSERT INTO analytics (metric_name, value) VALUES ('prix_moyen_electronique', {resultatsAnalyse.get("prix_moyen_electronique")});");
    }
}
```





## Partie 3 : Test et Validation

La classe `Main` exécute les différents processeurs.





```java
// Fichier: Main.java
public class Main {
    public static void main(String[] args) {
        System.out.println("--- Traitement avec ProcesseurCSV ---");
        ProcesseurDeDonnees csvProcessor = new ProcesseurCSV();
        csvProcessor.executerTraitement();

        System.out.println("\n--- Traitement avec ProcesseurBaseDeDonnees ---");
        ProcesseurDeDonnees dbProcessor = new ProcesseurBaseDeDonnees();
        dbProcessor.executerTraitement();
    }
}
```





### Sortie console du programme





```
--- Traitement avec ProcesseurCSV ---
--- Démarrage du traitement de données CSV ---
Début du chargement des données...
[CSV] Simulation de chargement de données depuis un fichier CSV.
Données brutes chargées: 
nom,age,ville
Alice,30,Paris
Bob,24,Lyon
Charlie,35,Marseille
Chargement terminé.
Début de la transformation des données...
[CSV] Transformation des données CSV en format utilisable.
Données transformées: [{nom=Alice, age=30, ville=Paris}, {nom=Bob, age=24, ville=Lyon}, {nom=Charlie, age=35, ville=Marseille}]
Transformation terminée.
Début de l'analyse des données...
[CSV] Analyse simple des données transformées (nombre de lignes, âge moyen).
Résultats de l'analyse: {nombre_lignes=3, age_moyen=29.666666666666668}
Analyse terminée.
Début de la sauvegarde des résultats...
[CSV] Simulation de sauvegarde des résultats de l'analyse (affichage console).
Rapport CSV final:
Nombre de lignes traitées: 3
Âge moyen des personnes: 29,67
Sauvegarde terminée.
--- Traitement de données terminé ---

--- Traitement avec ProcesseurBaseDeDonnees ---
--- Démarrage du traitement de données de Base de Données ---
Début du chargement des données...
[DB] Simulation de chargement de données depuis une base de données.
Données brutes chargées: [{product_name=Laptop, id=1, price=1200.0, category=Electronics}, {product_name=Mouse, id=2, price=25.0, category=Electronics}, {product_name=Keyboard, id=3, price=75.0, category=Electronics}, {product_name=Desk Chair, id=4, price=300.0, category=Furniture}]
Chargement terminé.
Début de la transformation des données...
[DB] Transformation des données DB (filtrage par catégorie 'Electronics').
Données transformées: [{NomProduit=Laptop, Prix=1200.0, ID=1}, {NomProduit=Mouse, Prix=25.0, ID=2}, {NomProduit=Keyboard, Prix=75.0, ID=3}]
Transformation terminée.
Début de l'analyse des données...
[DB] Analyse des données transformées (calcul du prix moyen des produits électroniques).
Résultats de l'analyse: {prix_moyen_electronique=433.3333333333333}
Analyse terminée.
Début de la sauvegarde des résultats...
[DB] Simulation de sauvegarde des résultats (affichage formaté pour DB).
SQL pour insertion des résultats:
INSERT INTO analytics (metric_name, value) VALUES ('prix_moyen_electronique', 433.3333333333333);
Sauvegarde terminée.
--- Traitement de données terminé ---
```





## Questions de Réflexion

1.  **Pourquoi le Template Method est-il adapté à ce scénario ?**
    Le Template Method est parfaitement adapté car il permet de définir une structure d'algorithme fixe (`charger -> transformer -> analyser -> sauvegarder`) tout en laissant les détails d'implémentation de chaque étape aux sous-classes. Cela garantit que tous les processeurs de données suivent la même séquence logique, tout en étant capables de gérer des sources et des traitements spécifiques (CSV, Base de Données, etc.). Par rapport à une approche où chaque processeur implémenterait l'intégralité de son algorithme de A à Z, le Template Method évite la duplication de code pour la structure de l'algorithme et assure une cohérence globale.

2.  **Comment ce pattern garantit-il la cohérence de l'algorithme ?**
    La cohérence est garantie par la "template method" (`executerTraitement()`) qui est une méthode `final` (ou non surchargable) dans la classe abstraite `ProcesseurDeDonnees`. Cette méthode définit l'ordre immuable des appels aux étapes de l'algorithme. Les sous-classes ne peuvent pas modifier cet ordre, elles ne peuvent qu'implémenter les détails des étapes individuelles.

3.  **Quelle est la différence fondamentale entre une méthode abstraite et une méthode "hook" dans le contexte du Template Method ?**
    *   **Méthode abstraite :** C'est une étape *obligatoire* de l'algorithme que les sous-classes *doivent* implémenter. Elle n'a pas d'implémentation par défaut dans la classe abstraite. Son rôle est de définir une étape qui varie nécessairement entre les implémentations concrètes.
    *   **Méthode "Hook" :** C'est une étape *optionnelle* de l'algorithme. Elle a une implémentation par défaut (souvent vide ou très simple) dans la classe abstraite. Les sous-classes *peuvent* la surcharger si elles ont besoin d'un comportement spécifique à ce point de l'algorithme, mais elles n'y sont pas obligées. Les hooks offrent des points d'extension sans forcer une implémentation.

4.  **Si vous deviez ajouter un nouveau type de processeur (par exemple, `ProcesseurAPI`), quelles seraient les étapes à suivre ?**
    1.  **Créer une nouvelle classe `ProcesseurAPI` :** Cette classe hériterait de `ProcesseurDeDonnees`.
    2.  **Implémenter les méthodes abstraites :**
        *   `chargerDonnees()`: Simulerait un appel à une API pour récupérer les données.
        *   `transformerDonnees()`: Adapterait le format JSON/XML de l'API en un format utilisable.
        *   `analyserDonnees()`: Effectuerait une analyse spécifique aux données de l'API.
        *   `sauvegarderResultats()`: Simulerait la sauvegarde des résultats (ex: envoi à un autre endpoint API ou affichage).
    3.  **(Optionnel) Surcharger les méthodes "hook" :** Par exemple, `notifierDebutTraitement()` pour afficher un message spécifique au traitement API.
    4.  **Utiliser dans `Main` :** Instancier `ProcesseurAPI` et appeler `executerTraitement()`.
    L'avantage est que cela ne nécessiterait aucune modification des classes `ProcesseurDeDonnees`, `ProcesseurCSV` ou `ProcesseurBaseDeDonnees`, respectant ainsi le principe Ouvert/Fermé.

5.  **En quoi le Template Method diffère-t-il du Design Pattern Strategy ?**
    Bien que les deux patterns utilisent l'héritage et le polymorphisme pour varier des comportements, leur intention et leur structure diffèrent :
    *   **Template Method :**
        *   **Intention :** Définit le *squelette d'un algorithme* dans une classe abstraite, en laissant les sous-classes redéfinir certaines étapes.
        *   **Relation :** Utilise l'héritage (relation "est un"). Les variations sont des sous-classes de la classe abstraite.
        *   **Contrôle :** La classe abstraite (le "template") contrôle l'ordre des étapes. Le comportement est fixé au moment de la compilation (ou du choix de la sous-classe).
    *   **Strategy :**
        *   **Intention :** Définit une *famille d'algorithmes*, les encapsule et les rend interchangeables. Permet au client de choisir un algorithme au moment de l'exécution.
        *   **Relation :** Utilise la composition (relation "a un"). Le contexte contient une référence à une interface de stratégie.
        *   **Contrôle :** Le client (ou le contexte) choisit et injecte la stratégie à utiliser. Le comportement peut être changé dynamiquement à l'exécution.

    En résumé, le Template Method se concentre sur la *structure* d'un algorithme et ses étapes fixes, tandis que Strategy se concentre sur l'*interchangeabilité* d'algorithmes complets.