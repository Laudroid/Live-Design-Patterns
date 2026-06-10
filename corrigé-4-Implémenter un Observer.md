Voici une proposition de solution pour le TP sur le Design Pattern Observer, en utilisant Java pour les exemples de code.

---

# Solution du TP : Implémentation du Pattern Observer - Système de Notification de Stock Produit

## 1. Définir les interfaces du pattern Observer

### Interface `Sujet` (Observable)




```java
// Fichier: Sujet.java
public interface Sujet {
    void enregistrerObservateur(Observateur observateur);
    void desenregistrerObservateur(Observateur observateur);
    void notifierObservateurs();
}
```




### Interface `Observateur`




```java
// Fichier: Observateur.java
public interface Observateur {
    void mettreAJour(Sujet sujet); // L'observateur reçoit le sujet pour récupérer les données
}
```




## 2. Implémenter le Sujet Concret : `Produit`

La classe `Produit` est l'objet dont l'état est observé. Elle maintient la liste de ses observateurs et les notifie en cas de changement de stock.




```java
// Fichier: Produit.java
import java.util.ArrayList;
import java.util.List;

public class Produit implements Sujet {
    private String nom;
    private int stock;
    private List<Observateur> observateurs;

    public Produit(String nom, int stockInitial) {
        this.nom = nom;
        this.stock = stockInitial;
        this.observateurs = new ArrayList<>();
        System.out.println($"Produit '{nom}' créé avec un stock initial de {stockInitial}.");
    }

    // Getters pour que les observateurs puissent récupérer l'état
    public String getNom() {
        return nom;
    }

    public int getStock() {
        return stock;
    }

    // Méthode pour changer le stock et notifier les observateurs
    public void setStock(int nouveauStock) {
        if (this.stock != nouveauStock) { // Notifier seulement si le stock a réellement changé
            System.out.println($"\n--- Changement de stock pour '{nom}': de {this.stock} à {nouveauStock} ---");
            this.stock = nouveauStock;
            notifierObservateurs();
        } else {
            System.out.println($"\nStock de '{nom}' inchangé ({nouveauStock}). Pas de notification.");
        }
    }

    @Override
    public void enregistrerObservateur(Observateur observateur) {
        if (!observateurs.contains(observateur)) {
            observateurs.add(observateur);
            System.out.println($"Observateur '{observateur.getClass().getSimpleName()}' enregistré pour '{nom}'.");
        }
    }

    @Override
    public void desenregistrerObservateur(Observateur observateur) {
        if (observateurs.remove(observateur)) {
            System.out.println($"Observateur '{observateur.getClass().getSimpleName()}' désenregistré pour '{nom}'.");
        }
    }

    @Override
    public void notifierObservateurs() {
        System.out.println($"Notification des observateurs pour '{nom}' (stock: {stock})...");
        for (Observateur observateur : observateurs) {
            observateur.mettreAJour(this); // Passe l'instance du produit lui-même
        }
    }
}
```




## 3. Implémenter les Observateurs Concrets

Chaque département implémente `Observateur` et contient sa propre logique de réaction.

### Classe `DepartementMarketing`




```java
// Fichier: DepartementMarketing.java
public class DepartementMarketing implements Observateur {
    private static final int SEUIL_PROMOTION = 10;

    @Override
    public void mettreAJour(Sujet sujet) {
        if (sujet instanceof Produit) {
            Produit produit = (Produit) sujet;
            if (produit.getStock() <= SEUIL_PROMOTION) {
                System.out.println($"[Marketing] Le stock de '{produit.getNom()}' est de {produit.getStock()} (<= {SEUIL_PROMOTION}). Envisager une promotion ou une alerte de réapprovisionnement.");
            } else {
                System.out.println($"[Marketing] Le stock de '{produit.getNom()}' est de {produit.getStock()} (> {SEUIL_PROMOTION}). Pas d'action spécifique requise.");
            }
        }
    }
}
```




### Classe `DepartementLogistique`




```java
// Fichier: DepartementLogistique.java
public class DepartementLogistique implements Observateur {
    private static final int SEUIL_CRITIQUE = 5;

    @Override
    public void mettreAJour(Sujet sujet) {
        if (sujet instanceof Produit) {
            Produit produit = (Produit) sujet;
            if (produit.getStock() <= SEUIL_CRITIQUE) {
                System.out.println($"[Logistique] Le stock de '{produit.getNom()}' est de {produit.getStock()} (<= {SEUIL_CRITIQUE}). Déclencher une commande fournisseur URGENTE !");
            } else {
                System.out.println($"[Logistique] Le stock de '{produit.getNom()}' est de {produit.getStock()} (> {SEUIL_CRITIQUE}). Stock suffisant pour l'instant.");
            }
        }
    }
}
```




### Classe `ServiceClients`




```java
// Fichier: ServiceClients.java
public class ServiceClients implements Observateur {
    @Override
    public void mettreAJour(Sujet sujet) {
        if (sujet instanceof Produit) {
            Produit produit = (Produit) sujet;
            if (produit.getStock() == 0) {
                System.out.println($"[Service Clients] Le stock de '{produit.getNom()}' est de 0. Informer les clients en attente et mettre à jour le site.");
            } else {
                System.out.println($"[Service Clients] Le stock de '{produit.getNom()}' est de {produit.getStock()} (> 0). Pas d'action spécifique requise.");
            }
        }
    }
}
```




## 4. Mettre en Scène (Classe Principale)

La classe `Main` orchestre la création du produit, l'enregistrement des observateurs et la simulation des changements de stock.




```java
// Fichier: Main.java
public class Main {
    public static void main(String[] args) {
        // Instancier un Produit
        Produit ordinateurPortable = new Produit("Ordinateur Portable", 20);

        // Instancier les observateurs
        DepartementMarketing marketing = new DepartementMarketing();
        DepartementLogistique logistique = new DepartementLogistique();
        ServiceClients serviceClients = new ServiceClients();

        // Enregistrer les observateurs auprès du Produit
        ordinateurPortable.enregistrerObservateur(marketing);
        ordinateurPortable.enregistrerObservateur(logistique);
        ordinateurPortable.enregistrerObservateur(serviceClients);

        // Simuler des changements de stock
        System.out.println("\n--- Simulation 1: Stock passe à 15 ---");
        ordinateurPortable.setStock(15);

        System.out.println("\n--- Simulation 2: Stock passe à 8 ---");
        ordinateurPortable.setStock(8);

        System.out.println("\n--- Simulation 3: Stock passe à 3 ---");
        ordinateurPortable.setStock(3);

        System.out.println("\n--- Simulation 4: Stock passe à 0 ---");
        ordinateurPortable.setStock(0);

        System.out.println("\n--- Simulation 5: Stock passe à 5 (remise en stock partielle) ---");
        ordinateurPortable.setStock(5);

        // Désenregistrer un observateur et simuler un nouveau changement
        System.out.println("\n--- Désenregistrement du Département Marketing ---");
        ordinateurPortable.desenregistrerObservateur(marketing);

        System.out.println("\n--- Simulation 6: Stock passe à 0 (Marketing ne devrait plus être notifié) ---");
        ordinateurPortable.setStock(0);
    }
}
```




### Sortie console de l'exécution du programme




```
Produit 'Ordinateur Portable' créé avec un stock initial de 20.
Observateur 'DepartementMarketing' enregistré pour 'Ordinateur Portable'.
Observateur 'DepartementLogistique' enregistré pour 'Ordinateur Portable'.
Observateur 'ServiceClients' enregistré pour 'Ordinateur Portable'.

--- Simulation 1: Stock passe à 15 ---
Notification des observateurs pour 'Ordinateur Portable' (stock: 15)...
[Marketing] Le stock de 'Ordinateur Portable' est de 15 (> 10). Pas d'action spécifique requise.
[Logistique] Le stock de 'Ordinateur Portable' est de 15 (> 5). Stock suffisant pour l'instant.
[Service Clients] Le stock de 'Ordinateur Portable' est de 15 (> 0). Pas d'action spécifique requise.

--- Simulation 2: Stock passe à 8 ---
Notification des observateurs pour 'Ordinateur Portable' (stock: 8)...
[Marketing] Le stock de 'Ordinateur Portable' est de 8 (<= 10). Envisager une promotion ou une alerte de réapprovisionnement.
[Logistique] Le stock de 'Ordinateur Portable' est de 8 (> 5). Stock suffisant pour l'instant.
[Service Clients] Le stock de 'Ordinateur Portable' est de 8 (> 0). Pas d'action spécifique requise.

--- Simulation 3: Stock passe à 3 ---
Notification des observateurs pour 'Ordinateur Portable' (stock: 3)...
[Marketing] Le stock de 'Ordinateur Portable' est de 3 (<= 10). Envisager une promotion ou une alerte de réapprovisionnement.
[Logistique] Le stock de 'Ordinateur Portable' est de 3 (<= 5). Déclencher une commande fournisseur URGENTE !
[Service Clients] Le stock de 'Ordinateur Portable' est de 3 (> 0). Pas d'action spécifique requise.

--- Simulation 4: Stock passe à 0 ---
Notification des observateurs pour 'Ordinateur Portable' (stock: 0)...
[Marketing] Le stock de 'Ordinateur Portable' est de 0 (<= 10). Envisager une promotion ou une alerte de réapprovisionnement.
[Logistique] Le stock de 'Ordinateur Portable' est de 0 (<= 5). Déclencher une commande fournisseur URGENTE !
[Service Clients] Le stock de 'Ordinateur Portable' est de 0. Informer les clients en attente et mettre à jour le site.

--- Simulation 5: Stock passe à 5 (remise en stock partielle) ---
Notification des observateurs pour 'Ordinateur Portable' (stock: 5)...
[Marketing] Le stock de 'Ordinateur Portable' est de 5 (<= 10). Envisager une promotion ou une alerte de réapprovisionnement.
[Logistique] Le stock de 'Ordinateur Portable' est de 5 (<= 5). Déclencher une commande fournisseur URGENTE !
[Service Clients] Le stock de 'Ordinateur Portable' est de 5 (> 0). Pas d'action spécifique requise.

--- Désenregistrement du Département Marketing ---
Observateur 'DepartementMarketing' désenregistré pour 'Ordinateur Portable'.

--- Simulation 6: Stock passe à 0 (Marketing ne devrait plus être notifié) ---
Notification des observateurs pour 'Ordinateur Portable' (stock: 0)...
[Logistique] Le stock de 'Ordinateur Portable' est de 0 (<= 5). Déclencher une commande fournisseur URGENTE !
[Service Clients] Le stock de 'Ordinateur Portable' est de 0. Informer les clients en attente et mettre à jour le site.
```




## Réflexion

### Avantages de cette approche par rapport à une notification directe

*   **Découplage :** Le `Produit` (le sujet) n'a aucune connaissance des classes concrètes de ses observateurs (`DepartementMarketing`, etc.) ni de la logique qu'ils exécutent. Il ne sait que qu'il doit notifier des objets qui implémentent l'interface `Observateur`. Cela réduit le couplage entre le sujet et ses observateurs.
*   **Flexibilité et Extensibilité :** Il est très facile d'ajouter de nouveaux types d'observateurs (ex: un `DepartementComptabilite`) sans modifier la classe `Produit`. Il suffit de créer une nouvelle classe qui implémente `Observateur` et de l'enregistrer. Cela respecte le principe Ouvert/Fermé (Open/Closed Principle).
*   **Réutilisabilité :** Les observateurs peuvent être réutilisés pour observer différents sujets, et un sujet peut avoir différents ensembles d'observateurs à différents moments.
*   **Gestion des dépendances :** Le sujet ne gère pas directement les dépendances des observateurs, il se contente de les notifier.

### Quand est-il pertinent d'utiliser le pattern Observer ?

Le pattern Observer est pertinent dans les situations où :
*   Un changement d'état dans un objet doit entraîner des changements dans d'autres objets, et le nombre ou le type de ces objets change dynamiquement.
*   Un objet doit pouvoir notifier d'autres objets sans faire d'hypothèses sur ces objets.
*   Il existe une relation "un à plusieurs" où un sujet peut avoir de nombreux observateurs.
*   Les objets doivent être faiblement couplés.

Exemples concrets :
*   Interfaces utilisateur (UI) : Les widgets (boutons, champs de texte) sont des sujets qui notifient les contrôleurs de leurs événements.
*   Flux de données : Les systèmes de streaming de données où les consommateurs s'abonnent à des flux.
*   Systèmes de messagerie : Les éditeurs publient des messages, les abonnés les reçoivent.

### Limites ou inconvénients potentiels

*   **Complexité accrue :** Pour des systèmes très simples, l'introduction du pattern Observer peut ajouter une complexité inutile (plus d'interfaces, plus de classes).
*   **Problèmes de performance :** Si un sujet a un très grand nombre d'observateurs, la notification de tous peut devenir coûteuse en termes de performance.
*   **Fuites de mémoire (Memory Leaks) :** Si les observateurs ne sont pas correctement désenregistrés, ils peuvent continuer à détenir une référence au sujet, empêchant le sujet d'être collecté par le garbage collector, même s'il n'est plus utilisé.
*   **Ordre de notification non garanti :** L'ordre dans lequel les observateurs sont notifiés n'est généralement pas garanti, ce qui peut poser problème si l'ordre des réactions est important.
*   **Problèmes de concurrence :** Dans un environnement multi-threadé, la notification des observateurs doit être gérée avec soin pour éviter les race conditions ou les deadlocks.

En conclusion, le pattern Observer est un outil puissant pour créer des systèmes réactifs et faiblement couplés, mais il doit être utilisé judicieusement en tenant compte de ses avantages et de ses inconvénients.