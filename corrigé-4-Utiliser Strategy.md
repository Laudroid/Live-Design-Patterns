Voici une proposition de solution pour le TP sur le Design Pattern Strategy.

---

# Solution du TP : Maîtrise du Pattern Strategy - Gestion des Coûts de Livraison

## Étape 0 : Préparation (Classe `Commande`)

La classe `Commande` contient les informations nécessaires au calcul des frais de livraison.





```java
// Fichier: Commande.java
public class Commande {
    private String idCommande;
    private double poidsTotal; // en kg
    private double montantTotal; // hors frais de livraison

    public Commande(String idCommande, double poidsTotal, double montantTotal) {
        this.idCommande = idCommande;
        this.poidsTotal = poidsTotal;
        this.montantTotal = montantTotal;
    }

    public String getIdCommande() {
        return idCommande;
    }

    public double getPoidsTotal() {
        return poidsTotal;
    }

    public double getMontantTotal() {
        return montantTotal;
    }

    @Override
    public String toString() {
        return "Commande{" +
               "idCommande='" + idCommande + '\'' +
               ", poidsTotal=" + poidsTotal + "kg" +
               ", montantTotal=" + String.format("%.2f", montantTotal) + "€" +
               '}';
    }
}
```




## Étape 1 : Définition de l'Interface de Stratégie

L'interface `ICalculateurFraisLivraison` est le contrat pour toutes les stratégies de calcul.





```java
// Fichier: ICalculateurFraisLivraison.java
public interface ICalculateurFraisLivraison {
    double calculerFrais(Commande commande);
    String getNomStrategie(); // Ajout pour faciliter l'affichage dans les tests
}
```




## Étape 2 : Implémentation des Stratégies Concrètes

### `LivraisonStandard` (Forfaitaire)





```java
// Fichier: LivraisonStandard.java
public class LivraisonStandard implements ICalculateurFraisLivraison {
    private static final double COUT_FIXE = 5.00;

    @Override
    public double calculerFrais(Commande commande) {
        return COUT_FIXE;
    }

    @Override
    public String getNomStrategie() {
        return "Livraison Standard (Forfaitaire)";
    }
}
```




### `LivraisonParPoids`





```java
// Fichier: LivraisonParPoids.java
public class LivraisonParPoids implements ICalculateurFraisLivraison {
    private static final double COUT_PAR_KG = 2.00;

    @Override
    public double calculerFrais(Commande commande) {
        return commande.getPoidsTotal() * COUT_PAR_KG;
    }

    @Override
    public String getNomStrategie() {
        return "Livraison par Poids";
    }
}
```




## Étape 3 : Le Contexte (La Classe Utilisatrice)

`ProcesseurCommande` utilise la stratégie configurée pour calculer les frais.





```java
// Fichier: ProcesseurCommande.java
public class ProcesseurCommande {
    private ICalculateurFraisLivraison strategieLivraison;

    public ProcesseurCommande(ICalculateurFraisLivraison strategieInitiale) {
        this.strategieLivraison = strategieInitiale;
    }

    public void setStrategieLivraison(ICalculateurFraisLivraison strategie) {
        System.out.println($"Changement de stratégie de livraison vers: {strategie.getNomStrategie()}");
        this.strategieLivraison = strategie;
    }

    public void traiterCommande(Commande commande) {
        if (strategieLivraison == null) {
            System.out.println("Erreur: Aucune stratégie de livraison n'est définie.");
            return;
        }

        double fraisLivraison = strategieLivraison.calculerFrais(commande);
        double montantTotalFinal = commande.getMontantTotal() + fraisLivraison;

        System.out.println("\n--- Traitement de la commande " + commande.getIdCommande() + " ---");
        System.out.println("Détails de la commande: " + commande);
        System.out.println("Stratégie de livraison utilisée: " + strategieLivraison.getNomStrategie());
        System.out.println("Frais de livraison calculés: " + String.format("%.2f", fraisLivraison) + "€");
        System.out.println("Montant total final (articles + livraison): " + String.format("%.2f", montantTotalFinal) + "€");
        System.out.println("----------------------------------------");
    }
}
```




## Étape 4 : Mise en Pratique et Test

La classe `Main` démontre l'utilisation des différentes stratégies.





```java
// Fichier: Main.java
public class Main {
    public static void main(String[] args) {
        // Création de commandes
        Commande commande1 = new Commande("CMD001", 1.5, 50.00);
        Commande commande2 = new Commande("CMD002", 5.0, 120.00);
        Commande commande3 = new Commande("CMD003", 0.8, 25.50);

        // Instanciation du processeur de commande avec une stratégie initiale
        ProcesseurCommande processeur = new ProcesseurCommande(new LivraisonStandard());

        // Test avec LivraisonStandard
        processeur.traiterCommande(commande1);
        processeur.traiterCommande(commande2);

        // Changement de stratégie pour LivraisonParPoids
        processeur.setStrategieLivraison(new LivraisonParPoids());
        processeur.traiterCommande(commande1);
        processeur.traiterCommande(commande3);

        // Les tests pour LivraisonExpress seront ajoutés ici
    }
}
```




### Sortie console (avant `LivraisonExpress`)




```
Changement de stratégie de livraison vers: Livraison Standard (Forfaitaire)

--- Traitement de la commande CMD001 ---
Détails de la commande: Commande{idCommande='CMD001', poidsTotal=1.5kg, montantTotal=50,00€}
Stratégie de livraison utilisée: Livraison Standard (Forfaitaire)
Frais de livraison calculés: 5,00€
Montant total final (articles + livraison): 55,00€
----------------------------------------

--- Traitement de la commande CMD002 ---
Détails de la commande: Commande{idCommande='CMD002', poidsTotal=5.0kg, montantTotal=120,00€}
Stratégie de livraison utilisée: Livraison Standard (Forfaitaire)
Frais de livraison calculés: 5,00€
Montant total final (articles + livraison): 125,00€
----------------------------------------
Changement de stratégie de livraison vers: Livraison par Poids

--- Traitement de la commande CMD001 ---
Détails de la commande: Commande{idCommande='CMD001', poidsTotal=1.5kg, montantTotal=50,00€}
Stratégie de livraison utilisée: Livraison par Poids
Frais de livraison calculés: 3,00€
Montant total final (articles + livraison): 53,00€
----------------------------------------

--- Traitement de la commande CMD003 ---
Détails de la commande: Commande{idCommande='CMD003', poidsTotal=0.8kg, montantTotal=25,50€}
Stratégie de livraison utilisée: Livraison par Poids
Frais de livraison calculés: 1,60€
Montant total final (articles + livraison): 27,10€
----------------------------------------
```




## Étape 5 : Ajout d'une Nouvelle Stratégie

### `LivraisonExpress`





```java
// Fichier: LivraisonExpress.java
public class LivraisonExpress implements ICalculateurFraisLivraison {
    private static final double COUT_FIXE_EXPRESS = 10.00;
    private static final double POURCENTAGE_MONTANT_TOTAL = 0.01; // 1%

    @Override
    public double calculerFrais(Commande commande) {
        return COUT_FIXE_EXPRESS + (commande.getMontantTotal() * POURCENTAGE_MONTANT_TOTAL);
    }

    @Override
    public String getNomStrategie() {
        return "Livraison Express";
    }
}
```




### Test de la nouvelle stratégie (mise à jour de `Main.java`)





```java
// Fichier: Main.java (mise à jour)
public class Main {
    public static void main(String[] args) {
        // ... (code précédent)

        // Changement de stratégie pour LivraisonExpress
        processeur.setStrategieLivraison(new LivraisonExpress());
        processeur.traiterCommande(commande1);
        processeur.traiterCommande(commande2);
        processeur.traiterCommande(commande3);
    }
}
```




### Sortie console complète (avec `LivraisonExpress`)




```
// ... (sortie précédente)

Changement de stratégie de livraison vers: Livraison Express

--- Traitement de la commande CMD001 ---
Détails de la commande: Commande{idCommande='CMD001', poidsTotal=1.5kg, montantTotal=50,00€}
Stratégie de livraison utilisée: Livraison Express
Frais de livraison calculés: 10,50€
Montant total final (articles + livraison): 60,50€
----------------------------------------

--- Traitement de la commande CMD002 ---
Détails de la commande: Commande{idCommande='CMD002', poidsTotal=5.0kg, montantTotal=120,00€}
Stratégie de livraison utilisée: Livraison Express
Frais de livraison calculés: 11,20€
Montant total final (articles + livraison): 131,20€
----------------------------------------

--- Traitement de la commande CMD003 ---
Détails de la commande: Commande{idCommande='CMD003', poidsTotal=0.8kg, montantTotal=25,50€}
Stratégie de livraison utilisée: Livraison Express
Frais de livraison calculés: 10,26€
Montant total final (articles + livraison): 35,76€
----------------------------------------
```




## Étape 6 : Réflexion et Discussion

1.  **Avantages :** Les principaux avantages du pattern Strategy dans ce scénario, par rapport à une approche `if/else if` ou `switch`, sont :
    *   **Flexibilité et Extensibilité :** L'ajout d'une nouvelle stratégie de livraison (`LivraisonExpress`) ne nécessite aucune modification des classes existantes (`ICalculateurFraisLivraison`, `LivraisonStandard`, `LivraisonParPoids`, `ProcesseurCommande`). Cela respecte le principe Ouvert/Fermé (Open/Closed Principle) de SOLID.
    *   **Découplage :** La classe `ProcesseurCommande` est découplée des détails d'implémentation des algorithmes de calcul de frais. Elle ne connaît que l'interface `ICalculateurFraisLivraison`.
    *   **Réduction de la complexité conditionnelle :** Élimine les blocs conditionnels complexes dans le `ProcesseurCommande` qui deviendraient ingérables avec de nombreuses stratégies.
    *   **Testabilité :** Chaque stratégie peut être testée isolément.
    *   **Changement de comportement à l'exécution :** La stratégie de livraison peut être changée dynamiquement pour une commande donnée.

2.  **Autres Contextes :** Le pattern Strategy est applicable dans de nombreux contextes où différents algorithmes peuvent être utilisés de manière interchangeable.
    *   **Exemple concret :** Dans un système de traitement d'images, différentes stratégies de filtres (noir et blanc, sépia, flou) pourraient être implémentées comme des stratégies, appliquées à une image par un `ImageProcessor`.
    *   Autres exemples : algorithmes de tri, stratégies de compression de fichiers, différentes méthodes de paiement, comportements d'IA pour des personnages de jeu.

3.  **Inconvénients :**
    *   **Augmentation du nombre de classes :** Chaque nouvelle stratégie nécessite une nouvelle classe, ce qui peut augmenter le nombre total de classes dans le projet. Pour un très petit nombre d'algorithmes stables, cela pourrait sembler excessif.
    *   **Le client doit connaître les stratégies :** Le client (`Main`) doit être conscient des différentes stratégies disponibles pour pouvoir choisir et instancier celle qui convient, ou un mécanisme de sélection (ex: Factory) doit être mis en place.
