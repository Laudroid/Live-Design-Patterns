Voici une proposition de solution pour le TP sur le Design Pattern Facade.

---

# Solution du TP : Implémentation du Design Pattern Facade

## 1. Implémentation du Sous-système (Les Composants de la Maison Connectée)

Ces classes représentent les différents appareils de la maison connectée. Leurs méthodes affichent simplement des messages pour simuler leur fonctionnement.

### Classe `Lumiere`



```java
// Fichier: Lumiere.java
public class Lumiere {
    private String emplacement;

    public Lumiere(String emplacement) {
        this.emplacement = emplacement;
    }

    public void allumer() {
        System.out.println(emplacement + " Lumière: Allumée.");
    }

    public void eteindre() {
        System.out.println(emplacement + " Lumière: Éteinte.");
    }

    public void reglerIntensite(int niveau) {
        System.out.println(emplacement + " Lumière: Intensité réglée à " + niveau + "%.");
    }
}
```



### Classe `SystemeAudio`



```java
// Fichier: SystemeAudio.java
public class SystemeAudio {
    private String emplacement;

    public SystemeAudio(String emplacement) {
        this.emplacement = emplacement;
    }

    public void allumer() {
        System.out.println(emplacement + " Système Audio: Allumé.");
    }

    public void eteindre() {
        System.out.println(emplacement + " Système Audio: Éteint.");
    }

    public void jouerMusique(String titre) {
        System.out.println(emplacement + " Système Audio: Lecture de '" + titre + "'.");
    }

    public void reglerVolume(int volume) {
        System.out.println(emplacement + " Système Audio: Volume réglé à " + volume + ".");
    }
}
```



### Classe `Thermostat`



```java
// Fichier: Thermostat.java
public class Thermostat {
    private int temperatureActuelle;

    public Thermostat() {
        this.temperatureActuelle = 20; // Température par défaut
    }

    public void setTemperature(int temperature) {
        this.temperatureActuelle = temperature;
        System.out.println("Thermostat: Température réglée à " + temperature + "°C.");
    }

    public int getTemperature() {
        return temperatureActuelle;
    }

    public void activerChauffage() {
        System.out.println("Thermostat: Chauffage activé.");
    }

    public void activerClimatisation() {
        System.out.println("Thermostat: Climatisation activée.");
    }
}
```



### Classe `SystemeSecurite`



```java
// Fichier: SystemeSecurite.java
public class SystemeSecurite {
    private boolean actif;

    public SystemeSecurite() {
        this.actif = false; // Désactivé par défaut
    }

    public void activer() {
        this.actif = true;
        System.out.println("Système de Sécurité: Activé.");
    }

    public void desactiver() {
        this.actif = false;
        System.out.println("Système de Sécurité: Désactivé.");
    }

    public void verifierCapteurs() {
        System.out.println("Système de Sécurité: Vérification des capteurs...");
        System.out.println("Système de Sécurité: Capteurs OK.");
    }

    public boolean isActif() {
        return actif;
    }
}
```



## 2. Création de la Façade : `MaisonConnecteeFacade`

Cette classe simplifie l'interaction avec le sous-système en offrant des méthodes de haut niveau.



```java
// Fichier: MaisonConnecteeFacade.java
public class MaisonConnecteeFacade {
    private Lumiere salonLumiere;
    private SystemeAudio salonAudio;
    private Thermostat thermostat;
    private SystemeSecurite securite;

    // Le constructeur initialise tous les composants du sous-système
    public MaisonConnecteeFacade() {
        this.salonLumiere = new Lumiere("Salon");
        this.salonAudio = new SystemeAudio("Salon");
        this.thermostat = new Thermostat();
        this.securite = new SystemeSecurite();
    }

    public void demarrerSoireeFilm(String titreMusique, int volume, int intensiteLumiere, int temperature) {
        System.out.println("\n--- Démarrage de la Soirée Film ---");
        if (securite.isActif()) {
            securite.desactiver();
        }
        salonAudio.allumer();
        salonAudio.reglerVolume(volume);
        salonAudio.jouerMusique(titreMusique);
        salonLumiere.allumer();
        salonLumiere.reglerIntensite(intensiteLumiere);
        thermostat.setTemperature(temperature);
        thermostat.activerClimatisation(); // Supposons qu'on veuille rafraîchir pour le film
        System.out.println("--- La Soirée Film est prête ! ---");
    }

    public void preparerNuit() {
        System.out.println("\n--- Préparation pour la Nuit ---");
        salonLumiere.eteindre();
        salonAudio.eteindre();
        thermostat.setTemperature(18); // Température plus fraîche pour la nuit
        thermostat.activerChauffage(); // Ou climatisation selon la saison
        securite.activer();
        securite.verifierCapteurs();
        System.out.println("--- Bonne nuit ! ---");
    }

    public void reveilMatin(int temperatureSouhaitee) {
        System.out.println("\n--- Préparation pour le Réveil Matin ---");
        if (securite.isActif()) {
            securite.desactiver();
        }
        salonLumiere.allumer();
        salonLumiere.reglerIntensite(30); // Lumière douce
        salonAudio.allumer();
        salonAudio.reglerVolume(15);
        salonAudio.jouerMusique("Radio Matin");
        thermostat.setTemperature(temperatureSouhaitee);
        thermostat.activerChauffage(); // Pour réchauffer la maison
        System.out.println("--- Bonjour ! ---");
    }
}
```



## 3. Test Client

La classe `Application` (ou `Main`) utilise la Facade pour interagir avec le système.



```java
// Fichier: Application.java
public class Application {
    public static void main(String[] args) {
        // Création de l'instance de la Facade
        MaisonConnecteeFacade maMaison = new MaisonConnecteeFacade();

        // Utilisation des méthodes simplifiées de la Facade
        maMaison.demarrerSoireeFilm("Inception Soundtrack", 25, 40, 22);
        maMaison.preparerNuit();
        maMaison.reveilMatin(21);
    }
}
```



### Sortie console du programme



```
--- Démarrage de la Soirée Film ---
Salon Système de Sécurité: Désactivé.
Salon Système Audio: Allumé.
Salon Système Audio: Volume réglé à 25.
Salon Système Audio: Lecture de 'Inception Soundtrack'.
Salon Lumière: Allumée.
Salon Lumière: Intensité réglée à 40%.
Thermostat: Température réglée à 22°C.
Thermostat: Climatisation activée.
--- La Soirée Film est prête ! ---

--- Préparation pour la Nuit ---
Salon Lumière: Éteinte.
Salon Système Audio: Éteint.
Thermostat: Température réglée à 18°C.
Thermostat: Chauffage activé.
Système de Sécurité: Activé.
Système de Sécurité: Vérification des capteurs...
Système de Sécurité: Capteurs OK.
--- Bonne nuit ! ---

--- Préparation pour le Réveil Matin ---
Système de Sécurité: Désactivé.
Salon Lumière: Allumée.
Salon Lumière: Intensité réglée à 30%.
Salon Système Audio: Allumé.
Salon Système Audio: Volume réglé à 15.
Salon Système Audio: Lecture de 'Radio Matin'.
Thermostat: Température réglée à 21°C.
Thermostat: Chauffage activé.
--- Bonjour ! ---
```



## 4. Réflexion

### Comment la Facade a-t-elle amélioré la lisibilité et la maintenabilité du code client ?

*   **Lisibilité :** Le code client est beaucoup plus facile à lire et à comprendre. Au lieu de voir une série d'appels à des objets et méthodes disparates (`salonAudio.allumer()`, `salonLumiere.reglerIntensite()`, etc.), le client voit des opérations de haut niveau qui correspondent directement aux intentions de l'utilisateur (`demarrerSoireeFilm()`, `preparerNuit()`). Cela rend le code plus expressif et moins verbeux.
*   **Maintenabilité :** Si la logique de "démarrer une soirée film" change (par exemple, on ajoute un nouveau composant, ou l'ordre des opérations est modifié), seule la méthode `demarrerSoireeFilm` dans la Facade doit être mise à jour. Le code client n'a pas besoin d'être touché, ce qui réduit le risque d'erreurs et simplifie les évolutions.

### Quels sont les avantages en termes de découplage ?

*   **Découplage du client et du sous-système :** Le client est complètement découplé des classes concrètes du sous-système (`Lumiere`, `SystemeAudio`, etc.). Il ne connaît que la Facade. Cela signifie que les modifications internes au sous-système (ajout/suppression de classes, changement d'API des composants) n'affecteront pas le client tant que l'interface de la Facade reste stable.
*   **Réduction des dépendances :** Le client n'a qu'une seule dépendance (la Facade) au lieu de multiples dépendances envers chaque composant du sous-système. Cela simplifie la gestion des dépendances et la complexité globale du système.

### Imaginez que vous deviez ajouter un nouveau composant (ex: `Stores`) ou une nouvelle fonctionnalité complexe (ex: `ModeVacances`). Comment cela affecterait-il votre Facade et vos composants existants ?

*   **Ajout d'un nouveau composant (`Stores`) :**
    1.  **Créer la classe `Stores` :** Avec des méthodes comme `ouvrir()`, `fermer()`, `reglerPosition(int pourcentage)`.
    2.  **Modifier la `MaisonConnecteeFacade` :**
        *   Ajouter une instance de `Stores` comme attribut.
        *   Initialiser cette instance dans le constructeur de la Facade.
        *   Mettre à jour les méthodes existantes de la Facade (`demarrerSoireeFilm`, `preparerNuit`, `reveilMatin`) pour inclure les appels aux méthodes de `Stores` si nécessaire (ex: `demarrerSoireeFilm` pourrait fermer les stores, `reveilMatin` pourrait les ouvrir).
    *   **Impact :** Les composants existants (`Lumiere`, `SystemeAudio`, etc.) ne sont pas affectés. Le code client n'est pas affecté du tout, car il continue d'appeler les mêmes méthodes de la Facade. C'est un excellent exemple du principe Ouvert/Fermé (Open/Closed Principle).

*   **Ajout d'une nouvelle fonctionnalité complexe (`ModeVacances`) :**
    1.  **Ajouter une nouvelle méthode à la `MaisonConnecteeFacade` :** Par exemple, `activerModeVacances()`.
    2.  **Implémenter la logique dans cette nouvelle méthode :** Cette méthode orchestrerait les appels aux composants existants (ex: éteindre toutes les lumières, régler le thermostat à une température économique, activer la sécurité, et éventuellement simuler une présence en allumant/éteignant les lumières aléatoirement si les composants le permettent).
    *   **Impact :** Les composants existants ne sont pas affectés. Les méthodes existantes de la Facade ne sont pas affectées. Seule la Facade est étendue avec une nouvelle méthode, et le client peut choisir d'utiliser cette nouvelle fonctionnalité.

Dans les deux cas, la Facade agit comme un point d'extension centralisé pour les interactions de haut niveau, protégeant le client de la complexité sous-jacente et des changements dans le sous-système.