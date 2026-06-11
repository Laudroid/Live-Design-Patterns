Bonjour !

Ce TP est conçu pour vous permettre d'implémenter le Design Pattern **Template Method**. L'objectif est de structurer un algorithme dont les étapes principales sont fixes, mais dont certaines implémentations peuvent varier.

---

### **TP : Traitement de Données Hétérogènes avec Template Method**

**Objectif du TP :**
Implémenter le Design Pattern Template Method pour définir le squelette d'un algorithme de traitement de données, en laissant les détails d'implémentation de certaines étapes aux sous-classes.

**Contexte :**
Dans de nombreuses applications, le processus de traitement de données suit une séquence logique et immuable : charger les données, les transformer, les analyser, puis sauvegarder les résultats. Cependant, la *manière* de charger, transformer, analyser ou sauvegarder peut varier considérablement selon la source des données (CSV, base de données, API) ou le type d'analyse requis. Le Template Method est idéal pour gérer cette situation en garantissant la cohérence de l'algorithme global tout en offrant la flexibilité nécessaire pour les étapes spécifiques.

**Énoncé du Problème :**
Vous devez concevoir un système capable de traiter différents types de données. L'algorithme de traitement est toujours le même :
1.  **Charger les données brutes.**
2.  **Transformer les données brutes** en un format utilisable.
3.  **Analyser les données transformées.**
4.  **Sauvegarder les résultats de l'analyse.**

Cependant, les spécificités de chaque étape (comment charger, comment transformer, comment analyser, comment sauvegarder) dépendront du contexte (par exemple, traitement de fichiers CSV vs. traitement de données issues d'une base de données).

---

**Consignes Générales :**

*   Le langage de programmation est libre (Java, C#, Python, PHP, etc.), choisissez celui avec lequel vous êtes le plus à l'aise.
*   Structurez votre code de manière claire et commentez les parties importantes.

---

**Étapes du TP :**

**Partie 1 : Le Squelette de l'Algorithme (La Classe Abstraite)**

1.  **Créez une classe abstraite** nommée `ProcesseurDeDonnees` (ou `DataProcessor` si vous préférez l'anglais).
2.  **Définissez la "Template Method" :** Dans cette classe abstraite, implémentez une méthode concrète publique appelée `executerTraitement()` (ou `processData()`). Cette méthode sera la "template method" et définira l'ordre fixe des étapes de l'algorithme. Elle devra appeler les méthodes des étapes suivantes dans l'ordre :
    *   `chargerDonnees()`
    *   `transformerDonnees()`
    *   `analyserDonnees()`
    *   `sauvegarderResultats()`
3.  **Déclarez les étapes variables comme méthodes abstraites :** Les méthodes `chargerDonnees()`, `transformerDonnees()`, `analyserDonnees()`, et `sauvegarderResultats()` devront être déclarées comme abstraites dans `ProcesseurDeDonnees`. Elles ne contiendront aucune implémentation dans la classe abstraite.
4.  **Ajoutez une méthode "Hook" (optionnel) :** Pour illustrer la flexibilité, ajoutez une méthode "hook" (méthode avec une implémentation par défaut vide ou simple, que les sous-classes peuvent surcharger) appelée `notifierDebutTraitement()` (ou `onProcessingStart()`). Cette méthode sera appelée au début de `executerTraitement()`. Son implémentation par défaut pourrait simplement afficher un message générique ou ne rien faire.
5.  **Gérez l'état :** La classe abstraite `ProcesseurDeDonnees` devrait maintenir un état interne (par exemple, une variable pour les données brutes, une autre pour les données transformées, et une pour les résultats de l'analyse) afin que les données puissent être passées d'une étape à l'autre.

**Partie 2 : Les Implémentations Concrètes (Les Variations)**

1.  **Créez une première sous-classe concrète :** Nommez-la `ProcesseurCSV` (ou `CSVProcessor`).
    *   Cette classe devra hériter de `ProcesseurDeDonnees`.
    *   Implémentez toutes les méthodes abstraites de manière à simuler le traitement de données issues d'un fichier CSV (vous pouvez utiliser des chaînes de caractères ou des listes/tableaux pour simuler les données, pas besoin de lire un vrai fichier).
        *   `chargerDonnees()`: Simule le chargement de données CSV (ex: une chaîne de caractères représentant des lignes CSV).
        *   `transformerDonnees()`: Simule la transformation (ex: découper les lignes en colonnes, filtrer quelques données).
        *   `analyserDonnees()`: Simule une analyse simple (ex: compter le nombre de lignes, trouver une valeur max/min).
        *   `sauvegarderResultats()`: Simule la sauvegarde (ex: afficher les résultats dans la console).
    *   (Optionnel) Surchargez la méthode `notifierDebutTraitement()` pour afficher un message spécifique au traitement CSV.

2.  **Créez une deuxième sous-classe concrète :** Nommez-la `ProcesseurBaseDeDonnees` (ou `DatabaseProcessor`).
    *   Cette classe devra hériter de `ProcesseurDeDonnees`.
    *   Implémentez toutes les méthodes abstraites de manière à simuler le traitement de données issues d'une base de données (là encore, simulez les données et les opérations).
        *   `chargerDonnees()`: Simule le chargement de données depuis une DB (ex: une liste d'objets ou de dictionnaires).
        *   `transformerDonnees()`: Simule une transformation différente (ex: joindre des données, agréger).
        *   `analyserDonnees()`: Simule une autre analyse (ex: calculer une moyenne, regrouper par catégorie).
        *   `sauvegarderResultats()`: Simule la sauvegarde (ex: afficher les résultats formatés pour une insertion DB).
    *   (Optionnel) Surchargez la méthode `notifierDebutTraitement()` pour afficher un message spécifique au traitement de base de données.

**Partie 3 : Test et Validation**

1.  **Dans votre programme principal (main) :**
    *   Créez une instance de `ProcesseurCSV`.
    *   Appelez sa méthode `executerTraitement()`.
    *   Créez une instance de `ProcesseurBaseDeDonnees`.
    *   Appelez sa méthode `executerTraitement()`.
2.  **Vérifiez les sorties :** Assurez-vous que chaque processeur exécute bien son propre ensemble d'étapes spécifiques tout en respectant l'ordre défini par la "template method".

---

**Questions de Réflexion (à discuter ou à noter) :**

1.  **Pourquoi le Template Method est-il adapté à ce scénario ?** Quels sont les avantages par rapport à une approche où chaque type de processeur implémenterait l'intégralité de son algorithme de A à Z ?
2.  **Comment ce pattern garantit-il la cohérence de l'algorithme ?**
3.  **Quelle est la différence fondamentale entre une méthode abstraite et une méthode "hook" dans le contexte du Template Method ?**
4.  **Si vous deviez ajouter un nouveau type de processeur (par exemple, `ProcesseurAPI`), quelles seraient les étapes à suivre ?**
5.  **En quoi le Template Method diffère-t-il du Design Pattern Strategy ?** (C'est une question classique qui aide à bien distinguer les deux).

---

Bon courage pour ce TP ! N'hésitez pas à explorer et à expérimenter.