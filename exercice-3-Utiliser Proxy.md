Bonjour à toutes et à tous,

Ce TP a pour objectif de vous faire explorer et implémenter le pattern de conception **Proxy**. Vous allez découvrir comment ce pattern permet d'ajouter une couche de contrôle ou de gestion autour d'un objet réel, sans modifier cet objet.

---

### TP : Contrôler l'accès à un service avec un Proxy

**Objectif du TP :** Mettre en œuvre le pattern Proxy pour contrôler l'accès à un service existant.

**Contexte :**
Imaginez que vous développez une application qui interagit avec un service de génération de rapports. Ce service, bien que fonctionnel, présente plusieurs contraintes :
1.  La génération d'un rapport est une opération coûteuse en temps et en ressources.
2.  Certains rapports sont sensibles et ne devraient être accessibles qu'à des utilisateurs ayant des droits spécifiques.
3.  Il serait utile de savoir qui demande quel rapport.

Plutôt que de modifier directement le service de rapports (ce qui pourrait être complexe, risqué, ou impossible si c'est un service tiers), vous décidez d'interposer un **Proxy** pour gérer ces préoccupations.

---

### Énoncé Détaillé

Votre tâche consiste à implémenter un service de rapports et son proxy associé, en respectant le pattern Proxy.

#### Partie 1 : Le Service de Rapports Réel (Subject et RealSubject)

1.  **Définir l'interface `IReportingService` :**
    *   Cette interface doit déclarer une méthode `generateReport(reportId: string, userId: string): string`.
    *   Cette méthode prend en paramètre l'identifiant du rapport à générer et l'identifiant de l'utilisateur demandeur.
    *   Elle retourne une chaîne de caractères représentant le rapport généré.

2.  **Implémenter la classe `RealReportingService` :**
    *   Cette classe doit implémenter l'interface `IReportingService`.
    *   Dans la méthode `generateReport`, simulez une opération longue (par exemple, avec un `Thread.sleep()` ou équivalent dans votre langage de prédilection, d'une durée de 2 à 3 secondes).
    *   Le rapport généré peut être une simple chaîne de caractères du type : `"Rapport [reportId] généré par le service réel pour l'utilisateur [userId]."`.
    *   Ajoutez un message de console clair indiquant que le service réel est en train de générer le rapport.

3.  **Test initial :**
    *   Créez une instance de `RealReportingService` et appelez sa méthode `generateReport` directement.
    *   Vérifiez que le rapport est généré et que le délai est respecté.

#### Partie 2 : Le Proxy de Contrôle (Proxy)

1.  **Implémenter la classe `ReportingServiceProxy` :**
    *   Cette classe doit également implémenter l'interface `IReportingService`. C'est crucial pour que le proxy puisse être utilisé de manière interchangeable avec le service réel.
    *   Le `ReportingServiceProxy` doit contenir une référence à une instance de `IReportingService` (qui sera le `RealReportingService`). Cette référence sera généralement injectée via le constructeur.

2.  **Mettre en œuvre la logique de contrôle dans `ReportingServiceProxy.generateReport` :**
    *   **Journalisation (Logging) :** Avant d'appeler le service réel, affichez un message de console indiquant que le proxy intercepte la demande pour le rapport `reportId` de l'utilisateur `userId`.
    *   **Contrôle d'accès (Protection Proxy) :**
        *   Définissez une règle simple : Seuls les utilisateurs avec l'ID `"admin"` ou `"manager"` peuvent accéder au rapport `"101"`.
        *   Si un utilisateur non autorisé tente d'accéder au rapport `"101"`, le proxy doit lever une exception (ou retourner un message d'erreur) sans appeler le service réel.
        *   Pour les autres rapports, l'accès est libre.
    *   **Mise en cache (Virtual Proxy / Caching Proxy - *Optionnel mais recommandé pour approfondir*) :**
        *   Si un rapport a déjà été généré récemment pour un `reportId` donné, le proxy devrait le retourner depuis un cache interne (par exemple, une `Map` ou un `Dictionary`) sans appeler à nouveau le service réel.
        *   Pour simplifier, vous pouvez considérer que le cache est valide indéfiniment pour la durée de vie du proxy.
        *   Ajoutez un message de console clair lorsque le rapport est servi depuis le cache.

#### Partie 3 : Intégration et Tests

1.  **Utilisation du Proxy :**
    *   Dans votre programme principal, créez une instance de `RealReportingService`.
    *   Puis, créez une instance de `ReportingServiceProxy` en lui passant l'instance du service réel.
    *   Toutes les interactions futures devront se faire *uniquement* via l'instance du `ReportingServiceProxy`.

2.  **Scénarios de test :**
    *   **Accès autorisé :** Appelez `generateReport` avec un utilisateur autorisé et un rapport non restreint (ex: `reportId="102"`, `userId="user1"`).
    *   **Accès refusé :** Appelez `generateReport` avec un utilisateur non autorisé tentant d'accéder au rapport restreint (ex: `reportId="101"`, `userId="user2"`). Vérifiez que l'exception est levée ou le message d'erreur retourné.
    *   **Accès autorisé au rapport restreint :** Appelez `generateReport` avec un utilisateur autorisé et le rapport restreint (ex: `reportId="101"`, `userId="admin"`).
    *   **Test de cache :** Appelez deux fois de suite `generateReport` avec les mêmes paramètres (ex: `reportId="102"`, `userId="user1"`). Vérifiez que le service réel n'est appelé qu'une seule fois et que la deuxième fois, le rapport est servi instantanément depuis le cache.

---

### Consignes Générales et Utilisation de l'IA

*   **Compréhension avant tout :** L'objectif est de comprendre *pourquoi* et *comment* le pattern Proxy fonctionne. N'hésitez pas à utiliser l'IA pour la syntaxe, les exemples de code dans votre langage, ou pour explorer des implémentations alternatives.
*   **Explication de votre code :** Si vous utilisez l'IA pour générer des parties de code, assurez-vous de pouvoir expliquer chaque ligne. Commentez votre code pour montrer votre compréhension.
*   **Design Pattern :** Concentrez-vous sur la structure du pattern Proxy : l'interface commune, le sujet réel et le proxy.
*   **Testez rigoureusement :** Les tests sont essentiels pour valider que votre proxy se comporte comme attendu dans tous les scénarios.
*   **Amusez-vous bien !** C'est une excellente occasion de renforcer vos compétences en conception logicielle.

---

N'hésitez pas à poser des questions si vous rencontrez des difficultés ou si certains points ne sont pas clairs. Bon courage !