# TP 1 : La Todo List ! ☑️

## Objectifs 🎯

Votre objectif est de réaliser une Todo List en Java EE.
Votre todo list devra permettre de :
- Créer une tâche
- Afficher la liste des tâches
- Supprimer une tâche
- Marquer une tâche comme terminée
- Modifier une tâche

> Les tâches terminées doivent être barrées.

### Exemple de liste : 

#### A faire

- [x] ~~Suivre le cours de Java EE~~ 🖊️ ❌
- [ ] Terminer l'exercice 🖊️ ❌


## Rendu 📥

- Vous devrez créer un répo GitHub pour votre projet
- Vous devrez créer un fichier README.md
    - Présentation du projet
    - Présentation des fonctionnalités
    - Lancer le projet

Vous serez évalué sur les critères suivants :
- Le projet est terminé et fonctionnel
- La qualité du code
- La qualité du README.md
- La réalisation ou non de la partie bonus

## User Stories 📝

### User Story 1 : Créer une tâche

En tant qu'utilisateur,
je souhaite pouvoir créer une tâche,
afin de pouvoir l'ajouter à ma todo list.

```gherkin
Feature: Todo List

    Scenario: Créer une tâche
        Given je saisi le titre "Acheter du pain"
        When je valide la création de la tâche
        Then la tâche "Acheter du pain" est ajoutée à ma todo list
```

### User Story 2 : Afficher la liste des tâches

En tant qu'utilisateur,
je souhaite pouvoir afficher la liste des tâches,
afin de pouvoir consulter ma todo list.

```gherkin
Feature: Todo List

    Scenario: Afficher la liste des tâches
        Given j'ai les tâches "Acheter du pain", "Acheter du lait"
        When je consulte ma todo list
        Then je vois les tâches "Acheter du pain", "Acheter du lait"
```

### User Story 3 : Supprimer une tâche

En tant qu'utilisateur,
je souhaite pouvoir supprimer une tâche,
afin de pouvoir la retirer de ma todo list.

```gherkin
Feature: Todo List

    Scenario: Supprimer une tâche
        Given j'ai les tâches "Acheter du pain", "Acheter du lait"
        When je supprime la tâche "Acheter du pain"
        Then je vois les tâches "Acheter du lait"
```

### User Story 4 : Marquer une tâche comme terminée

En tant qu'utilisateur,
je souhaite pouvoir marquer une tâche comme terminée,
afin de savoir qu'elle est terminée dans ma todo list.

```gherkin
Feature: Tâches terminées

    Scenario: Marquer une tâche comme terminée
        Given j'ai les tâches "Acheter du pain", "Acheter du lait"
        When je marque la tâche "Acheter du pain" comme terminée
        Then je vois les tâches "Acheter du lait" affichée normalement
        And je vois la tâche "Acheter du pain" barrée
```

### User Story 5 : Modifier une tâche

En tant qu'utilisateur,
je souhaite pouvoir modifier une tâche,
afin de pouvoir corriger une faute de frappe ou une erreur.

```gherkin
Feature: Todo List

    Scenario: Modifier une tâche
        Given j'ai les tâches "Acheter du pin", "Acheter du lait"
        When je modifie la tâche "Acheter du pin" en "Acheter du pain"
        Then je vois les tâches "Acheter du pain", "Acheter du lait"
```

## Bonus 🎁

- Ajouter une date de création à la tâche
- Ajouter une date de modification à la tâche
- Ajouter une date de terminaison à la tâche (qui remplace la date de modification)
- Ajouter un message si la todo list est vide (ex : "Vous n'avez pas de tâches à faire !")
- Ajouter un message si toutes les tâches sont terminées (ex : "Vous avez terminé toutes vos tâches !") 
- Ajouter un bouton pour vider la liste

## Indices du poulet 🐥

- Vous devrez créer une classe `Task` avec les attributs suivants :
    - `id` : `int`
    - `title` : `String`
    - `creationDate` : `LocalDateTime` (optionnel)
    - `modificationDate` : `LocalDateTime` (optionnel)
    - `completionDate` : `LocalDateTime` (optionnel)
    - `completed` : `boolean`
- Vous devrez créer un servlet `TaskServlet` qui gérera les requêtes HTTP
    - le servlet devra avoir un attribut `tasks` qui sera une `List<Task>`
    - la méthode `doGet` devra afficher la liste des tâches (vide au début)
    - la méthode `doPost` devra ajouter une tâche à la liste
- Vous devrez créer un JSP `todo.jsp` qui affichera la todo list
    - Il doit être stocké dans le dossier `WEB-INF`
    - Il contiendra un formulaire pour ajouter une tâche
    - Une liste des tâches à faire
        - Il parcourra la liste des tâches et affichera le titre de chaque tâche
        - Il affichera un bouton pour supprimer la tâche
        - Il affichera une checkbox pour marquer la tâche comme terminée
        - La tâche sera barrée si elle est terminée
