---
title: JEE - Introduction
author: Alexandre Devos
company: Octocorn
contributors:
  - Alexandre Devos
sources:
  - 
---

# Java EE

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## Java EE

### Introduction

- Java Enterprise Edition
- Framework Java pour le développement d'applications web
- API Java pour le développement d'applications web

----

## Java EE

### Introduction

- Orienté serveur
- Comparable à .NET, PHP, Ruby on Rails, etc.

----

## Java EE

### Jakarta EE

- Projet repris par la fondation Eclipse
- Anciennement Java EE
- Depuis 2019, Java EE est devenu Jakarta

> N'a pas pu être nommé Java EE à cause de la marque déposée par Oracle

----

## Java EE

### Orienté serveur

![Schema HTTP](assets/schema_http.png) <!-- .element width="50%" -->

- Les applications sont déployées sur un serveur
- Il traite les requêtes des clients et renvoie les réponses HTTP

----

## Java EE

### Orienté serveur

![Schema HTTP](assets/schema_http.png) <!-- .element width="50%" -->

- **Serveur HTTP** : Serveur qui traite les requêtes HTTP
- **Conteneur** : Partie qui exécute les applications Java EE

----

## Java EE

### Serveur Tomcat

- Serveur web open source que nous utiliserons
- Il existe de nombreux autres serveurs web Java EE comme Glassfish, Wildfly, etc.
- Tomcat est le plus simple à prendre en main, et surtout est gratuit

---

# Le MVC

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## MVC

### Définition

- **M**odel **V**iew **C**ontroller
- Architecture logicielle (design pattern)
- Sépare les données, la logique et l'interface utilisateur

----

## MVC

### Usage

- En JEE, il n'y a pas de structure imposée
- Il y a donc de multiples façons de structurer son application
- Le MVC est une des plus utilisées

----

## MVC

### Fonctionnement

![MVC](assets/mvc.png) <!-- .element width="50%" -->

----

## MVC

### Définitions

- **Controller** : Gère les requêtes HTTP et les envoie au bon **Model**
- **Model** : Gère les données et la logique de l'application
- **View** : Gère l'interface utilisateur (HTML, CSS...)

----

## MVC

### Avantages

- Séparation des responsabilités
- Facilite la maintenance et l'évolution de l'application
- Connu et utilisé par de nombreux développeurs
- Facilite le travail en équipe

----

## MVC

### Inconvénients

- Risque de cloisonnement des développeurs (front, back, etc.)
- Augmentation du nombre de fichiers
- Augmente la complexité de l'application

----

## MVC et JEE

- **Controller** : Servlet
- **Model** : Objets Java (Java Beans)
- **View** : Pages JSP (Java Server Pages)

> Le modèle peut communiquer avec la BDD

----

## JEE et Frameworks

- Il existe de nombreux frameworks Java EE qui se basent sur le MVC
- Spring, Struts, JSF, etc.
- Ils sont livrés avec des fonctionnalités supplémentaires !

> Nous utiliserons uniquement les composants de base de Java EE

---

# Setup

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## Setup

- Installez IntelliJ IDEA
- Installez le JDK 17 ou supérieur

----

## Setup

### Tomcat

- Sur la page de [Tomcat 10](https://tomcat.apache.org/download-10.cgi)
- Sur la page, cliquez sur le lien "64-bit Windows Zip" de la section "Core"
- Extraire le dossier à la racide de `C:\Tomcat`

---

# Hello World

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## Hello World

### Création du projet

- Dans IntelliJ, cliquez sur "Create New Project"
- Sélectionnez `Java Enterprise` dans la liste à gauche
- Sélectionnez `Web Application` dans la liste à droite
- Dans `Application Server`, cliquez sur `Configure...` et sélectionnez le dossier contenant Tomcat
- Dans la page suivante, sélectionnez la version de JEE que vous souhaitez utiliser

----

## Hello World

### Démarrer l'application

- Une fois les dépendances chargées, cliquez sur `Run` en haut à droite
- La page devrait s'ouvrir dans votre navigateur par défaut !

----

## Hello World

### Trouble shooting

- Il arrive que Tomcat charge indéfiniment
- Dans ce cas, cliquez sur File > Invalidate Cache > Invalidate and Restart
- Redémarrez l'application

----

## Hello World

### Contenu du projet

```shell
.
├── src
│   └── main
│       ├── java # Code Java
│       │   └── com
│       │       └── example
│       │           └── HelloWorldServlet.java # Servlet
│       └── webapp # Contenu du site web, généré par l'IDE
│           ├── index.jsp # Page JSP
│           └── WEB-INF # Dossier protégé qui contient les fichiers de configuration et les JSP
│               └── web.xml # Fichier de configuration
└── pom.xml
```

----

## Hello World

### Servlet

`HelloWorldServlet.java`

```java
package fr.octocorn.hellodemo;

import java.io.*;

import jakarta.servlet.http.*;
import jakarta.servlet.annotation.*;

// Annotation qui permet de définir le chemin de la servlet
@WebServlet(name = "helloServlet", value = "/hello-servlet")
// Classe qui hérite de HttpServlet
public class HelloServlet extends HttpServlet {
    // Attribut de la classe
    private String message;

    // Méthode appelée lors de l'initialisation de la servlet
    public void init() {
        // Initialisation de l'attribut avec le message à afficher
        message = "Hello World!";
    }

    // Méthode appelée lors d'une requête HTTP GET
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException {
        // Définit le type de contenu de la réponse (ici HTML)
        response.setContentType("text/html");

        // Récupère le flux de sortie de la réponse
        PrintWriter out = response.getWriter();
        // Écrit dans le flux de sortie
        out.println("<html><body>");
        // Injections de la variable message dans le HTML
        out.println("<h1>" + message + "</h1>");
        out.println("</body></html>");
    }

    // Méthode appelée lors de la destruction de la servlet
    public void destroy() {
    }
}
```

----

## Hello World

### JSP

`index.jsp`

```html
// Définit le type de contenu de la page
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
    <title>JSP - Hello World</title>
</head>
<body>
// Écrit dans le flux de sortie
<h1><%= "Hello World!" %>
</h1>
<br/>
// Lien vers la servlet
<a href="hello-servlet">Hello Servlet</a>
</body>
</html>
```

----

## Hello World

### `web.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
         version="6.0">
</web-app>
```

> Avant, la configuration des servlets se faisait dans ce fichier

Note: Maintenant, on utilise les annotations

----

## Hello World

### web.xml

- Fichier de configuration de l'application
- Contient les informations sur les servlets, les filtres, les listeners, etc.
- Dans notre cas, il est vide car nous utilisons les annotations

----

## Hello World

### URL

- On peut lancer le projet en cliquant sur la flèche verte en haut à droite
- Lors du lancement de l'application, le navigateur ouvre la page `index.jsp`
- Sur intelliJ, il prend en URL le nom du build : `http://localhost:8080/hellodemo_war_exploded/`
- Il est possible de le changer dans les paramètres de lancement

----

## Hello World

### Changer URL

- En haut à droite, dérouler le menu `Edit Configurations`
- Dans l'onglet `Server`, retirer la fin de l'URL (`nomProjet_war_exploded`)
- Dans l'onglet `Deployment`, changer le `Application context` par `/`
- Rebuild l'application et relancez-la

----

## Hello World

### Don't Panic

Nous verrons en détail le fonctionnement des servlets et des JSP dans les prochains chapitres !

---

# Maven

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## Maven

### Définition

- Outil de gestion et d'automatisation de production des projets logiciels Java
- Gère les dépendances, les tests, la compilation, etc.

> Equivalent à NPM pour NodeJS

----

## Maven

### Structure

- `pom.xml` : Fichier de configuration
- Il est rédigé en XML

----

## Maven

### Fonctionnement

- Maven télécharge les dépendances dans un dossier local
- Il les ajoute au projet lors de la compilation
- Il est possible de définir des dépendances dans le `pom.xml`

----

## Maven

### Dépendances

- Les dépendances sont définies dans la balise `<dependencies>`
- Chaque dépendence est définie dans une balise `<dependency>`

```xml
<dependencies>
    <dependency>
        <groupId>jakarta.servlet</groupId>
        <artifactId>jakarta.servlet-api</artifactId>
        <version>5.0.0</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

----

## Maven

### Dépendances

- `groupId` : Groupe de l'application
- `artifactId` : Nom de l'application
- `version` : Version de l'application
- `scope` : Portée de la dépendance

----

## Maven

### Dépendances : scope

- `compile` : Dépendance nécessaire à la compilation
- `provided` : Dépendance fournie par le conteneur
- `runtime` : Dépendance nécessaire à l'exécution
- `test` : Dépendance nécessaire aux tests
- `system` : Dépendance fournie par le système
- `import` : Dépendance fournie par un autre POM

----

## Maven

### Ajouter des dépendances

- Les dépendances sont stockées dans des dépôts (repositories)
- Il en existe plusieurs : Maven Central, MvnRepository, etc.
- Pour les ajouter au projet, il faut les définir dans le `pom.xml`
- Les balises sont listés sur la page de la dépendance (RTFM)

----

## Maven

### Ajouter des dépendances

```xml [ 1, 8 | 3-7]
<dependencies>
  <!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
  <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.16.0</version>
  </dependency>
</dependencies>
```

----

## Maven

### Installation

- Une fois le `pom.xml` rédigé, il faut lancer la commande `mvn install`
- Avec IntelliJ, il est possible de lancer la commande depuis l'IDE :
  1. Clic droit sur le `pom.xml` > `Maven` > `Reload Project`
  2. Un tooltip est également présent en haut à droite
  3. Sur la barre d'outils, il y a également un bouton "M" pour Maven

----

## Maven

### Installation

- Sur un projet existant, il faudra également lancer la commande `mvn install` !
- Comme pour `npm install` dans un projet NodeJS !

> Les dépendances sont à recharger à chaque fois que le `pom.xml` est modifié !

----

## Maven

### `properties`

- Il est possible de définir des variables dans le `pom.xml`
- Elles sont définies dans la balise `<properties>`
- Elles sont utilisées avec `${nom_de_la_variable}`

> Très utile pour définir des versions de dépendances !

----

## Maven

### `properties`

```xml [ 2, 9]
<properties>
    <jakarta.servlet-api.version>5.0.0</jakarta.servlet-api.version>
</properties>

<dependencies>
    <dependency>
        <groupId>jakarta.servlet</groupId>
        <artifactId>jakarta.servlet-api</artifactId>
        <version>${jakarta.servlet-api.version}</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

---

# La suite !

[Retour à l'index](index.html)