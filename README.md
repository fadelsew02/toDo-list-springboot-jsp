## ToDoController
## @GetMapping({"/", "viewToDoList"})
### - Affiche la liste complète des todos
### - Récupère la liste depuis le service et l'ajoute au modèle
### - Affichage avec la vue "ViewToDoList"  
### - Permet aussi d'afficher un message en paramètre

## @GetMapping("/updateToDoStatus/{id}")
### - Met à jour le statut d'un todo
### - Appelle la méthode updateStatus() du service 
### - Redirige vers la liste avec un message de confirmation

## @GetMapping("/addToDoItem") 
### - Affiche le formulaire de création d'un nouveau todo
### - Initialise un nouvel objet todo et l'ajoute au modèle
### - Utilise la vue "AddToDoItem"

## @PostMapping("/saveToDoItem")
### - Enregistre ou met à jour un todo
### - Appelle le service pour sauvegarder 
### - Redirige vers la liste ou le formulaire selon le résultat

## @GetMapping("/editToDoItem/{id}")
### - Affiche un formulaire pour éditer un todo existant
### - Récupère le todo correspondant à updater  

## @PostMapping("/editSaveToDoItem") 
### - Enregistre les modifications d'un todo
### - Appelle le service pour mettre à jour
### - Redirige vers liste ou formulaire d'édition  

## @GetMapping("/deleteToDoItem/{id}")
### - Supprime un todo
### - Appelle le service pour supprimer
### - Redirige vers la liste

## ToDo.java (model)

## Modèle Todo

La classe `ToDo` représente l'entité principale de l'application de gestion de tâches. Elle modélise le concept d'une tâche à réaliser ("to-do item").

Voici la description des principaux éléments :

### Attributs

- `id` : clé primaire de type `Long`, générée automatiquement en base de données (auto-increment)  
- `title` : titre de la tâche de type `String`, non nul 
- `date` : date de la tâche au format `java.util.Date`, non nulle
- `status` : statut de la tâche comme "A Faire", "En cours", etc. Stocké dans un `String`, non nul
 
### Annotations

- `@Entity` : indique que c'est une entité JPA persistante
- `@Table` : relie la classe à une table de base de données nommée `todo` 
- `@Id` : désigne l'attribut `id` comme clé primaire
- `@GeneratedValue` : indique que la valeur de l'id est générée automatiquement   
- `@Column` : lie chaque attribut à une colonne en base de données, avec contrainte de non nullité
- `@DateTimeFormat` : format d'affichage de la date sous la forme `yyyy-MM-dd`

### Méthodes

- Accesseurs (getters) et mutateurs (setters) sur tous les attributs
- Un constructeur vide par défaut


## IToDoRepo.java

## Repository Todo

`IToDoRepo` est l'interface définissant le repository pour accéder en base de données aux objets `ToDo`.

### Spécificités

- Hérite des méthodes de `JpaRepository` pour le CRUD
- Type d'entité géré : `ToDo`  
- Type de la clé primaire : `Long`

### Annotations 

- `@Repository` : permet l'injection de l'implémentation concrète de cette interface dans les autres composants 
- `JpaRepository` apporte déjà les annotations JPA pour la persistance

### Méthodes disponibles

Hérite de tous les méthodes de base de `JpaRepository` :

- `save()` : Persiste ou met à jour une entité
- `findById()` : Récupère une entité par son id
- `findAll()` : Récupère toutes les entités
- `count()` : Compte les enregistrements
- `delete()` : Supprime une entité
- etc.

Aucune méthode personnalisée n'a été ajoutée dans cette interface `IToDoRepo`.

Les méthodes héritées suffisent aux besoins basiques de l'application côté persistance des `ToDo`.

La couche service contiendra la logique applicative plus complexe si nécessaire.



## ToDoService.java
Voici une analyse du fichier de code Java pour une application ToDo en format Markdown:

## Analyse du code ToDoApplication

### Aperçu

Il s'agit d'une classe de service Java nommée `ToDoService` pour une application ToDo. 

Elle utilise le framework Spring et interagit avec un repository pour gérer les opérations de CRUD sur les objets `ToDo`.

### Dépendances

- `org.springframework` - Pour l'injection de dépendances et les annotations Spring 
- `com.example.ToDoApp` - Les packages de l'application contenant les modèles et le repository
- `java.util` - Pour les ArrayList et List

#### Fonctionnalités

Les méthodes de la classe permettent de:

- Récupérer tous les ToDos
- Récupérer un ToDo par son ID
- Mettre à jour le statut d'un ToDo
- Sauvegarder/mettre à jour un ToDo
- Supprimer un ToDo

### Implémentation

- `@Service` - Annote la classe comme un Spring Service
- `@Autowired` - Injecte automatiquement le repository
- Utilise l'interface `IToDoRepo` pour accéder aux données 
- Implémente la logique métier pour chaque cas d'utilisation
- Retourne des boolean pour indiquer le succès ou l'échec des opérations de CRUD


## Analyse du fichier WebMvcConfig

### Aperçu

Fichier de configuration Spring pour configurer le moteur de vues JSP.


### Rôle 

Ce fichier définit la configuration nécessaire pour que Spring puisse résoudre et rendre les vues JSP.

### Détails

- Annoté avec `@Configuration` pour indiquer que c'est une classe de configuration Spring

- Définit un bean `ViewResolver` pour résoudre les vues

    - Utilise `InternalResourceViewResolver` pour les JSP
    
    - Le préfixe définit le chemin du dossier des vues JSP 
    
    - Le suffixe définit l'extension `.jsp`

    - La vue utilise la classe `JstlView` pour le support JSTL

### Utilisation

Ce fichier de configuration est chargé par Spring au démarrage.

Permet de rendre des vues JSP storées dans `/WEB-INF/jsp/` automatiquement.

Les contrôleurs peuvent simplement renvoyer le nom logique de la vue sans extension ou chemin.

## Analyse du fichier application.properties

### Vue d'ensemble

Fichier de configuration Spring Boot pour l'application ToDoApp. 

Définit les propriétés de configuration pour:

- Le serveur Web
- Le moteur de templates
- La base de données
- JPA / Hibernate

### Détails

#### Serveur Web

- `server.port` : le port sur lequel démarrer le serveur embarqué (8001)

#### Vues 

- `spring.mvc.view.prefix` : chemin de base des fichiers de vues JSP
- `spring.mvc.view.suffix` : extension des fichiers de vues (.jsp)

#### Base de données

- `spring.datasource.*` : configuration de la connexion à la base PostgreSQL
    - URL, nom d'utilisateur, mot de passe

#### JPA / Hibernate

- `spring.jpa.hibernate.ddl-auto` : stratégie de migration de la BDD à appliquer au démarrage  

### Utilisation

Chargé automatiquement par Spring Boot au démarrage pour configurer l'application.

Permet d'externaliser les configurations sans toucher au code.



Voici une analyse du fichier JSP addToDo.jsp en format Markdown :

## Analyse du fichier addToDo.jsp

### Vue d'ensemble

Page JSP pour l'ajout d'un item ToDo dans l'application ToDo.

Utilise Spring MVC avec JSTL.

### Fonctionnalités

- Formulaire pour la saisie des détails d'un item ToDo:
    - Titre
    - Date
    - Statut
- Envoi du formulaire vers le contrôleur `/saveToDoItem`
- Notifications avec Toastr


#### Formulaire

- `<form:form>` lie le model `todo` pour le binding
- Inputs avec `path` pour lier les champs du model

#### Notifications

- Interpole le paramètre `message` pour vérifier les erreurs
- Configure et affiche les notifications avec Toastr


## Analyse du fichier editToDo.jsp

### Vue d'ensemble

Page JSP pour l'édition d'un item ToDo existant dans l'application ToDo. 

Similaire à la page addToDo.jsp.

Utilise Spring MVC Form Binding avec JSTL.

### Fonctionnalités

- Formulaire pré-rempli pour éditer les détails d'un item ToDo
- Binding vers l'objet `todo` existant 
- Soumission du formulaire vers le contrôleur `/editSaveToDo`
- Notifications d'erreurs avec Toastr


### Points importants

- `<form:input>` pré-remplie avec les valores de l'objet `todo`
- Champ `id` caché pour identifier l'item à mettre à jour


## Voici une analyse détaillée du fichier viewToDoItem.jsp :

## Vue d'ensemble

- Page JSP pour afficher la liste des éléments Todo
- Utilise Spring MVC JSTL pour générer le code côté serveur
- Affiche la liste dans un tableau Bootstrap
- Permet de modifier l'état et effectuer des actions sur les éléments
- Notifications avec Toastr

## Fonctionnalités

## Affichage des données

- `<c:forEach>` boucle sur la liste `${list}` 
- Affiche les propriétés de chaque `todo` dans le tableau

### Boutons d'actions

Permet de :

- Marquer un élément comme complet 
- Éditer un élément
- Supprimer un élément

Avec des liens vers les contrôleurs correspondants.

### Notifications

- Affiche des notifications en fonction des paramètres reçus
- Configuré via le script Toastr global

### Bouton d'ajout 

- Bouton pour accéder à l'écran d'ajout d'un nouvel élément

