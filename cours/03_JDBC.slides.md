---
title: Template
theme: solarized
author: Alexandre Devos
company: Octocorn
contributors:
  - Alexandre Devos
sources:
  - 
---

# JDBC

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## JDBC

### Définition

- **J**ava **D**ata**B**ase **C**onnectivity
- API Java permettant d'interagir avec une base de données relationnelle
- Permet d'exécuter des requêtes SQL depuis Java

----

## JDBC

### Setup

Créez une BDD sur Docker à partir du fichier `docker-compose.yml` suivant :

```yaml
version: '3.9'

services:
  db:
    image: mysql:8.0
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: db
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - "3306:3306"
    volumes:
      - mysql-data:/var/lib/mysql
volumes:
  mysql-data:
```

> Perdu sur Docker ? Suivez ce [cours](https://github.com/Octocorn-Learning/Docker) !

----

## JDBC

### Setup

Connectez-vous sur votre BDD avec un client SQL (Workbench) et créez la BDD `users` avec la table `personnes` suivante :

```sql
CREATE
DATABASE users DEFAULT CHARACTER SET UTF8MB4;
USE
users;
CREATE TABLE `personnes`
(
    `id`     int          NOT NULL AUTO_INCREMENT,
    `nom`    varchar(255) NOT NULL,
    `prenom` varchar(255) NOT NULL,
    PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
```

----

## JDBC

### Setup

Insérez quelques utilisateurs dans la table `personnes` :

```sql
INSERT INTO `personnes` (`nom`, `prenom`)
VALUES ('Bowie', 'David'),
       ('Mercury', 'Freddie'),
       ('Jackson', 'Michael'),
       ('Cobain', 'Kurt'),
       ('Hendrix', 'Jimi');
```

----

## JDBC

### Setup

Pour pouvoir utiliser JDBC, il faut ajouter la dépendance suivante dans le fichier `pom.xml` :

```xml
 <!-- https://mvnrepository.com/artifact/com.mysql/mysql-connector-j -->
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <version>8.2.0</version>
</dependency>
```

----

## JDBC

### Setup

L'ajout de la dépendance nous donnera accès à la classe `DriverManager` :

```java
public class Main {
    public static void main(String[] args) {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

> Nous pourrons utiliser ce driver pour créer une connexion à notre BDD !

----

## JDBC

### Connexion

La méthode `getConnection` de la classe `DriverManager` nous permet de créer une connexion à notre BDD :

```java
DriverManager.getConnection(url, user, password);
```

- `url` : URL de la BDD
- `user` : utilisateur de la BDD
- `password` : mot de passe de la BDD

----

## JDBC

### Connexion

La connection est généralement stockée dans une méthode à part :

```java
private void loadDatabase() {
        // Chargement du driver
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
        }

        try {
            connexion = DriverManager.getConnection("jdbc:mysql://localhost:3306/users", "root", "root");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
```

> Il est aussi possible d'utiliser une classe statique, un singleton, etc.

---

# JDBC

## Requêtes

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## JDBC

### Requêtes

- JDBC permet d'exécuter des requêtes SQL depuis Java
- Il nous donne accès à la classe `Statement` qui nous permettra d'exécuter des requêtes
- Il existe trois types de `Statement` : `Statement`, `PreparedStatement` et `CallableStatement`

> CallableStatement ne sera pas abordé dans ce cours

----

## JDBC

### Statement

- `Statement` est la classe la plus simple
- Elle permet d'exécuter des requêtes SQL
- Elle ne permet pas de paramétrer les requêtes

----

## JDBC

### Statement

Pour exécuter une requête SQL avec un `Statement`, il faut utiliser la méthode `executeQuery` :

```java
// Création de la connexion
Statement statement = connexion.createStatement();
// Appel de la requête SQL et récupération du résultat
ResultSet resultSet = statement.executeQuery("SELECT * FROM personnes");
```

> `executeQuery` retourne un `ResultSet` qui contient les résultats de la requête

----


## JDBC

### PreparedStatement

- `PreparedStatement` permet d'exécuter des requêtes SQL
- Elle pourra prendre des paramètres
- Elle permet de précompiler les requêtes pour les réutiliser

----

## JDBC

### PreparedStatement

Pour ajouter un paramètre à une requête SQL, il faut utiliser la méthode `setString` :

```java
// Création de la connexion
PreparedStatement statement = connexion.prepareStatement("SELECT * FROM personnes WHERE nom = ?");
// Paramétrage de la requête
statement.setString(1, "Bowie");
// Appel de la requête SQL et récupération du résultat
ResultSet resultSet = statement.executeQuery();
```

> `setString` permet de remplacer le premier `?` par la valeur "Bowie"

----

## JDBC

### PreparedStatement

Avec plusieurs paramètres :

```java
// Création de la connexion
PreparedStatement statement = connexion.prepareStatement("SELECT * FROM personnes WHERE nom = ? AND prenom = ?");
// Paramétrage de la requête
statement.setString(1, "Bowie");
statement.setString(2, "David");
// Appel de la requête SQL et récupération du résultat
ResultSet resultSet = statement.executeQuery();
```

> Les paramètres sont numérotés à partir de 1.

----

## JDBC

### PreparedStatement

Les preparedStatement peuvent aussi servir à réaliser des requêtes d'insertion :

```java
// Création de la connexion
PreparedStatement statement = connexion.prepareStatement("INSERT INTO personnes (nom, prenom) VALUES (?, ?)");
// Paramétrage de la requête
statement.setString(1, "Bowie");
statement.setString(2, "David");
// Appel de la requête SQL
statement.executeUpdate();
```

> `executeUpdate` permet d'exécuter une requête d'insertion, de mise à jour ou de suppression

----

## JDBC

### ResultSet

- `ResultSet` est une classe qui permet de stocker les résultats d'une requête SQL
- Elle permet de parcourir les résultats de la requête
- Elle permet de récupérer les valeurs des colonnes

----

## JDBC

### ResultSet

Pour parcourir les résultats d'une requête, il faut utiliser la méthode `next` :

```java
// Création de la connexion
Statement statement = connexion.createStatement();
// Appel de la requête SQL et récupération du résultat
ResultSet resultSet = statement.executeQuery("SELECT * FROM personnes");
// Parcours des résultats
while (resultSet.next()) {
    // Récupération des valeurs
    int id = resultSet.getInt("id");
    String nom = resultSet.getString("nom");
    String prenom = resultSet.getString("prenom");
}
```

> `next` permet de passer à la ligne suivante du `ResultSet`

----

## JDBC

### ResultSet

- `getInt` permet de récupérer la valeur d'une colonne de type `int`
- `getString` permet de récupérer la valeur d'une colonne de type `String`
- `getBoolean` permet de récupérer la valeur d'une colonne de type `boolean`

> On doit toujours parcourir les résultats avec `next` avant de récupérer les valeurs !

----

## Démonstration !

Un petit CRUD musical !

----

## JDBC

### A vous de jouer !

---

# Aller plus loin avec JPA

La partie JDBC est terminée, mais il existe une autre API Java qui permet d'interagir avec une BDD : JPA.  

Elle sera abordée dans le cours dédié à Spring !