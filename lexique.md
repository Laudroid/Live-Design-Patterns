**Design Patterns** : Solutions éprouvées à des problèmes de conception récurrents en génie logiciel, visant la réutilisabilité et la maintenance.

**Patterns de création** : Catégorie de patterns qui gèrent l'instanciation des objets.

**Patterns de structure** : Catégorie de patterns qui concernent la composition des classes et objets pour améliorer la modularité et la flexibilité.

**Patterns de comportement** : Catégorie de patterns qui gèrent la communication et la logique entre objets.

**Principes SOLID** : Ensemble de cinq principes de conception orientée objet (Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion) servant de base aux design patterns.

**Single Responsibility Principle (SRP)** : Principe SOLID stipulant qu'une classe ne devrait avoir qu'une seule raison de changer.

**Open/Closed Principle (OCP)** : Principe SOLID stipulant qu'une entité logicielle doit être ouverte à l'extension, mais fermée à la modification.

**Liskov Substitution Principle (LSP)** : Principe SOLID stipulant que les objets d'un programme doivent pouvoir être remplacés par des instances de leurs sous-types sans altérer la justesse du programme.

**Interface Segregation Principle (ISP)** : Principe SOLID stipulant qu'une interface ne devrait pas forcer les classes qui l'implémentent à dépendre de méthodes qu'elles n'utilisent pas.

**Dependency Inversion Principle (DIP)** : Principe SOLID stipulant que les modules de haut niveau ne doivent pas dépendre des modules de bas niveau, mais tous deux doivent dépendre d'abstractions. Les abstractions ne doivent pas dépendre des détails, mais les détails doivent dépendre des abstractions.

**Anti-patterns** : Mauvaises pratiques courantes en génie logiciel, à reconnaître et à éviter.

**Gang of Four (GoF)** : Groupe de quatre auteurs (Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides) dont le livre a popularisé les design patterns en génie logiciel.

**Réutilisabilité** : Capacité d'un composant logiciel à être utilisé dans différentes applications ou contextes.

**Maintenance** : Activités visant à modifier un logiciel après sa livraison pour corriger des défauts, améliorer les performances ou adapter le produit à un environnement modifié.

**Instanciation des objets** : Processus de création d'une instance (un objet) d'une classe.

**Composition des classes et objets** : Méthode d'assemblage de classes et d'objets pour former des structures plus complexes.

**Communication entre objets** : Interaction et échange d'informations entre différents objets d'un système.

**Singleton** : Pattern de création qui garantit qu'une classe n'a qu'une seule instance et fournit un point d'accès global à celle-ci.

**Factory Method** : Pattern de création qui définit une interface pour créer un objet, mais laisse les sous-classes décider quelle classe instancier.

**Abstract Factory** : Pattern de création qui fournit une interface pour créer des familles d'objets apparentés ou dépendants sans spécifier leurs classes concrètes.

**Découplage de l'instanciation** : Séparation de la logique de création d'objets de leur utilisation, réduisant les dépendances directes.

**Builder** : Pattern de création qui permet la construction pas à pas d'objets complexes, en séparant la construction de sa représentation.

**Prototype** : Pattern de création qui spécifie les types d'objets à créer en utilisant une instance prototype et en créant de nouveaux objets par clonage de ce prototype.

**Clonage d'objets** : Processus de création d'une copie exacte d'un objet existant.

**Modularité** : Degré auquel les composants d'un système peuvent être séparés et combinés de manière flexible.

**Flexibilité** : Capacité d'un système à s'adapter facilement aux changements ou aux nouvelles exigences.

**Adapter** : Pattern de structure qui permet à des interfaces incompatibles de collaborer, en convertissant l'interface d'une classe en une autre interface attendue par les clients.

**Interopérabilité** : Capacité de différents systèmes ou composants à travailler ensemble.

**Decorator** : Pattern de structure qui permet d'ajouter dynamiquement des comportements supplémentaires à un objet sans modifier sa structure, en utilisant la composition plutôt que l'héritage.

**Héritage** : Mécanisme par lequel une classe peut acquérir les propriétés et les comportements d'une autre classe.

**Facade** : Pattern de structure qui fournit une interface unifiée et simplifiée à un ensemble d'interfaces complexes dans un sous-système.

**Composite** : Pattern de structure qui permet de composer des objets en structures arborescentes et de traiter les objets individuels et les compositions d'objets de manière uniforme.

**Arborescences et structures récursives** : Structures de données où chaque élément peut avoir un ou plusieurs 'enfants' formant une hiérarchie.

**Proxy** : Pattern de structure qui fournit un substitut ou un mandataire pour un autre objet, contrôlant l'accès à cet objet.

**Contrôle d'accès** : Mécanisme permettant de gérer qui ou quoi peut accéder à une ressource ou à une fonctionnalité.

**Lazy loading** : Stratégie de chargement différé, où un objet est chargé ou créé uniquement lorsqu'il est nécessaire.

**Logging transversal** : Enregistrement d'informations (logs) concernant l'exécution d'un programme, appliqué de manière à traverser différentes parties du système.

**Observer / Event Bus** : Pattern de comportement où un objet (sujet) notifie un ensemble d'observateurs de tout changement d'état. Event Bus est une implémentation courante de ce principe pour la gestion d'événements.

**Gestion des événements** : Mécanismes pour capturer, distribuer et réagir aux événements se produisant dans une application.

**Réactivité des applications** : Capacité d'une application à répondre rapidement aux interactions utilisateur ou aux changements de données.

**Strategy** : Pattern de comportement qui permet de définir une famille d'algorithmes, d'encapsuler chacun d'eux et de les rendre interchangeables.

**Encapsulation d'algorithmes interchangeables** : Regroupement de différentes implémentations d'un même algorithme de manière à pouvoir les substituer facilement.

**Command** : Pattern de comportement qui encapsule une requête en tant qu'objet, permettant de paramétrer des clients avec différentes requêtes, de mettre en file d'attente, de journaliser ou de supporter les opérations d'annulation (undo/redo).

**Undo/Redo** : Fonctionnalités permettant d'annuler une ou plusieurs actions précédentes, et de les rétablir.

**Template Method** : Pattern de comportement qui définit le squelette d'un algorithme dans une opération, en laissant les sous-classes redéfinir certaines étapes sans changer la structure de l'algorithme.

**Spécialisation des étapes de l'algorithme** : Modification ou adaptation de parties spécifiques d'un algorithme générique par des sous-classes.

**Iterator** : Pattern de comportement qui fournit un moyen d'accéder séquentiellement aux éléments d'une collection sans exposer sa représentation sous-jacente.

**Chain of Responsibility** : Pattern de comportement qui délègue la gestion d'une requête le long d'une chaîne d'objets, jusqu'à ce qu'un objet la traite.

**Délégation de responsabilité** : Mécanisme par lequel un objet transmet une tâche ou une requête à un autre objet pour traitement.

**Patterns architecturaux** : Solutions générales et réutilisables pour des problèmes récurrents dans l'architecture logicielle d'un système, définissant la structure et l'organisation globale.

**Frameworks modernes** : Cadres de travail logiciels offrant des bibliothèques, des conventions et des outils pour faciliter le développement d'applications, intégrant souvent des design patterns.

**Injection de dépendances (Dependency Injection)** : Pattern de conception qui permet de découpler les composants en injectant leurs dépendances plutôt que de les laisser les créer elles-mêmes.

**Patterns réactifs** : Ensemble de patterns liés à la programmation réactive, gérant les flux de données asynchrones et les propagations de changements.

**Repository Pattern** : Pattern architectural qui abstrait le mécanisme de stockage et d'accès aux données, agissant comme un intermédiaire entre le domaine de l'application et la couche de persistance des données.

**Unit of Work** : Pattern architectural qui gère un ensemble d'opérations sur la base de données comme une seule transaction atomique, garantissant que toutes les opérations réussissent ou échouent ensemble.

**Domain Driven Design (DDD)** : Approche de développement logiciel qui se concentre sur le domaine métier et la logique du domaine, en s'appuyant sur un modèle de domaine.

**Reactor Pattern** : Pattern réactif qui gère de manière non bloquante plusieurs requêtes de service simultanément, en les démultiplexant à partir d'une source unique et en les distribuant aux gestionnaires d'événements appropriés.

**Observable Pattern** : Pattern réactif qui représente un flux de données ou d'événements pouvant être observés et sur lesquels on peut réagir.

