Bonjour à toutes et à tous,

Ce TP a pour objectif de vous familiariser avec le Design Pattern **Facade**. Vous allez apprendre à simplifier l'accès à un sous-système complexe en créant une interface unifiée et plus simple.

---

### **TP : Implémentation du Design Pattern Facade**

**Objectif du TP :**
Créer une Facade pour simplifier l'interaction avec un sous-système complexe, en réduisant la dépendance du client vis-à-vis des multiples composants internes.

**Contexte :**
Imaginez que vous développez un système de gestion pour une maison connectée. Cette maison est équipée de divers appareils (lumières, système audio, thermostat, système de sécurité) qui sont contrôlés par des interfaces distinctes et parfois verbeuses. Pour un utilisateur, ou pour d'autres parties de l'application, interagir directement avec chaque appareil pour accomplir une tâche simple (comme "démarrer une soirée film") devient rapidement fastidieux et source d'erreurs.

**Énoncé du Problème :**
Sans une Facade, un client souhaitant "démarrer une soirée film" devrait :
1.  Allumer le système audio.
2.  Régler le volume du système audio.
3.  Allumer les lumières.
4.  Régler l'intensité des lumières.
5.  Régler la température du thermostat.
6.  Désactiver le système de sécurité (si actif).

Cela implique de connaître et d'interagir avec plusieurs objets et leurs méthodes spécifiques.

---

**Votre Mission :**

Vous allez implémenter ce sous-système complexe et ensuite créer une Facade pour offrir des opérations simplifiées.

**1. Implémentation du Sous-système (Les Composants de la Maison Connectée)**

Créez les classes suivantes, chacune représentant un composant de la maison connectée. Pour ce TP, leurs méthodes peuvent simplement afficher un message sur la console (`System.out.println`) pour simuler l'action.

*   **`Lumiere`**
    *   `allumer()`
    *   `eteindre()`
    *   `reglerIntensite(int niveau)`
*   **`SystemeAudio`**
    *   `allumer()`
    *   `eteindre()`
    *   `jouerMusique(String titre)`
    *   `reglerVolume(int volume)`
*   **`Thermostat`**
    *   `setTemperature(int temperature)`
    *   `getTemperature()`
    *   `activerChauffage()`
    *   `activerClimatisation()`
*   **`SystemeSecurite`**
    *   `activer()`
    *   `desactiver()`
    *   `verifierCapteurs()`


**2. Création de la Façade**

Créez une classe nommée `MaisonConnecteeFacade`. Cette classe sera la Facade.

*   Elle devra contenir des références (instances) de chaque composant du sous-système (`Lumiere`, `SystemeAudio`, `Thermostat`, `SystemeSecurite`). Ces instances peuvent être initialisées dans le constructeur de la Facade.
*   Elle devra exposer les méthodes simplifiées suivantes :
    *   `demarrerSoireeFilm(String titreMusique, int volume, int intensiteLumiere, int temperature)` : Cette méthode orchestrera les appels aux méthodes des composants pour configurer la maison pour une soirée film.
        *   Exemple : désactiver la sécurité, allumer l'audio et jouer la musique, régler le volume, allumer les lumières et régler l'intensité, régler le thermostat.
    *   `preparerNuit()` : Cette méthode éteindra tout et activera le système de sécurité.
        *   Exemple : éteindre les lumières, éteindre l'audio, régler le thermostat à une température de nuit, activer la sécurité.
    *   `reveilMatin(int temperatureSouhaitee)` : Configure la maison pour le matin.
        *   Exemple : désactiver la sécurité, allumer les lumières à une faible intensité, allumer l'audio (par exemple, jouer "radio matin"), régler le thermostat.


**3. Test Client**

Dans une classe `Application` (ou `Main`), créez une instance de votre `MaisonConnecteeFacade`.

*   Appelez les méthodes de la Facade (`demarrerSoireeFilm`, `preparerNuit`, `reveilMatin`) pour observer le comportement simplifié.
*   Comparez la simplicité du code client utilisant la Facade par rapport à ce qu'il aurait été si vous aviez dû interagir directement avec chaque composant.

**4. Réflexion (Optionnel mais recommandé)**

*   Comment la Facade a-t-elle amélioré la lisibilité et la maintenabilité du code client ?
*   Quels sont les avantages en termes de découplage ?
*   Imaginez que vous deviez ajouter un nouveau composant (ex: `Stores`) ou une nouvelle fonctionnalité complexe (ex: `ModeVacances`). Comment cela affecterait-il votre Facade et vos composants existants ?

---

**Critères de Réussite :**

*   Les classes du sous-système sont bien définies et indépendantes.
*   La classe `MaisonConnecteeFacade` est implémentée correctement, orchestrant les appels aux composants.
*   Le code client utilise uniquement la Facade pour interagir avec le sous-système, démontrant la simplification.
*   Le principe du Design Pattern Facade est respecté : une interface simple pour un sous-système complexe.

**Quelques pistes pour vous aider :**

*   Commencez par les classes les plus simples (les composants).
*   Pensez à l'ordre des opérations au sein de chaque méthode de la Facade.
*   L'IA est un outil puissant ; utilisez-la pour accélérer les tâches répétitives et vous concentrer sur la conception de la Facade et la compréhension de son rôle. Si vous bloquez, essayez de reformuler votre question à l'IA ou de décomposer le problème en étapes plus petites.

Bon courage pour ce TP !