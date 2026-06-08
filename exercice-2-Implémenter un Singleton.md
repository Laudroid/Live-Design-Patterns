Bonjour à toutes et à tous,

Ce TP a pour objectif de vous faire explorer un des Design Patterns les plus connus et utilisés : le **Singleton**. Vous allez non seulement l'implémenter, mais aussi comprendre et résoudre les défis qu'il pose dans un environnement multi-threadé.

---

### TP : Implémentation du Design Pattern Singleton avec Gestion des Threads

**Objectif du TP :**
Maîtriser l'implémentation du pattern Singleton, en particulier sa version thread-safe, et comprendre les compromis liés aux différentes approches de gestion de la concurrence.

**Contexte :**
Le pattern Singleton est fondamental pour les scénarios où une seule instance d'une classe est nécessaire et doit être accessible globalement. Pensez à un gestionnaire de configuration, un pool de connexions à une base de données, ou un logger. Cependant, sa mise en œuvre correcte, surtout dans des applications concurrentes (multi-threads), demande une attention particulière pour éviter la création de multiples instances.

**Langage au choix :**
Vous êtes libre de choisir le langage de programmation que vous maîtrisez le mieux (Java, C#, Python, C++, JavaScript/TypeScript, etc.).

---

**Exercice :**

Votre mission est d'implémenter un Singleton robuste et d'en analyser les différentes facettes.

**Étape 1 : Le Singleton Basique (Non Thread-Safe)**

1.  **Implémentez un Singleton simple.**
    *   Assurez-vous que le constructeur est privé.
    *   Fournissez une méthode statique publique pour obtenir l'instance unique.
    *   Utilisez une variable statique pour stocker cette instance.
2.  **Expliquez** en quelques lignes pourquoi cette implémentation garantit une instance unique dans un environnement mono-threadé.

**Étape 2 : Identification du Problème de Concurrence**

1.  **Créez un scénario de test multi-threadé.**
    *   Lancez plusieurs threads (par exemple, 5 à 10 threads).
    *   Chaque thread doit tenter d'obtenir l'instance du Singleton via la méthode statique que vous avez créée à l'Étape 1.
    *   Dans l'implémentation de votre Singleton, ajoutez un identifiant unique (par exemple, un `hashCode()` de l'instance, ou un ID généré au moment de la création) et un message de log à la création de l'instance pour visualiser quand une nouvelle instance est créée.
2.  **Démontrez le problème.**
    *   Exécutez votre test et observez la sortie.
    *   **Expliquez** la "race condition" (condition de concurrence) qui peut survenir et pourquoi votre Singleton basique échoue à garantir une instance unique dans ce contexte. Fournissez la sortie de votre programme qui illustre ce problème.

**Étape 3 : Implémentation Thread-Safe (Approche 1 - Simple)**

1.  **Modifiez votre Singleton** pour le rendre thread-safe en utilisant une technique de synchronisation simple (par exemple, en synchronisant la méthode `getInstance()` ou un bloc de code critique).
2.  **Testez** cette nouvelle implémentation avec le scénario multi-threadé de l'Étape 2. Vérifiez qu'une seule instance est désormais créée.
3.  **Discutez** des avantages et inconvénients de cette approche en termes de performance et de simplicité.

**Étape 4 : Exploration d'une Approche Thread-Safe Avancée**

1.  **Recherchez et implémentez une autre technique de Singleton thread-safe** qui vise à améliorer les performances par rapport à l'approche de l'Étape 3, tout en garantissant la sécurité des threads.
    *   Quelques pistes : "Double-Checked Locking" (avec l'utilisation de `volatile` si votre langage le supporte et le requiert), "Initialization-on-demand holder idiom" (spécifique à Java), ou l'utilisation d'un `Enum` (spécifique à Java). Si votre langage propose des mécanismes spécifiques (ex: `std::call_once` en C++), n'hésitez pas à l'explorer.
2.  **Implémentez** cette approche.
3.  **Expliquez** comment cette nouvelle approche fonctionne et en quoi elle diffère de la précédente. Mettez en évidence les subtilités (par exemple, l'importance de `volatile` pour DCL).

**Étape 5 : Réflexion et Comparaison**

1.  **Comparez** les différentes implémentations thread-safe que vous avez explorées (celle de l'Étape 3 et celle de l'Étape 4).
2.  **Quand choisiriez-vous** l'une plutôt que l'autre ? Y a-t-il des cas où la simplicité prime sur la performance, ou inversement ?
3.  **Quels sont les avantages et les inconvénients généraux** du pattern Singleton lui-même (au-delà de la gestion des threads) ?

---

**Rendu :**

Vous devrez fournir :

1.  **Le code source** de toutes vos implémentations (Singleton basique, thread-safe simple, thread-safe avancé) ainsi que le code de test multi-threadé.
2.  **Un document synthétisant vos choix, vos explications et les compromis de chaque approche.** Ce document doit inclure :
    *   Les réponses aux questions posées à chaque étape.
    *   Les sorties de console ou captures d'écran prouvant le problème de concurrence (Étape 2) et la résolution (Étape 3 et 4).
    *   Votre analyse comparative et vos réflexions de l'Étape 5.

---

**Conseils pour l'utilisation de l'IA :**

L'IA est un outil puissant pour l'apprentissage et le développement. N'hésitez pas à l'utiliser pour :

*   **Explorer des solutions :** Demandez-lui différentes manières d'implémenter un Singleton thread-safe dans votre langage.
*   **Comprendre des concepts complexes :** Si un terme comme "race condition", "volatile", ou "memory barrier" vous est obscur, interrogez l'IA pour des explications claires et des exemples.
*   **Générer du code boilerplate :** Laissez-la vous aider à écrire le code de base pour les threads ou les tests.
*   **Débugger :** Si votre code ne se comporte pas comme prévu, demandez à l'IA d'analyser votre code et de suggérer des pistes.

**Votre valeur ajoutée réside dans votre capacité à comprendre, analyser, justifier et adapter le code.** Ne vous contentez pas de copier-coller. Utilisez l'IA comme un assistant intelligent qui vous aide à approfondir votre compréhension et à accélérer votre apprentissage. N'hésitez pas à lui poser des questions sur les "pourquoi" et les "comment" derrière les solutions qu'elle propose.

---

**Critères d'évaluation :**

*   **Fonctionnalité :** Le code implémente correctement les différentes versions du Singleton et les tests démontrent les comportements attendus.
*   **Compréhension :** Les explications sont claires, précises et démontrent une bonne compréhension des concepts (Singleton, thread-safety, race conditions, compromis).
*   **Qualité du code :** Le code est lisible, bien structuré et respecte les bonnes pratiques du langage choisi.
*   **Justification des choix :** Les arguments pour ou contre chaque approche sont pertinents et bien étayés.
*   **Clarté des explications :** Le document de synthèse est bien organisé et facile à lire.

Bon courage pour ce TP !