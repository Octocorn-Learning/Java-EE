---
title: Servlet
theme: solarized
author: Alexandre Devos
company: Octocorn
contributors:
  - Alexandre Devos
sources:
  - 
---

# Servlet

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

---

## Servlet

### Rappels

- Comme expliqué, un serveur web est un programme qui tourne en permanence sur une machine et qui attend des requêtes
  HTTP
- Lorsqu'une requête arrive, le serveur web va chercher le fichier correspondant à la requête et le renvoie au client

----

## Servlet

### Déroulement du cours

- Le cours sera réalisé sous la forme d'une démonstration fil rouge
- Tous les points abordés lors de cette démonstration seront expliqués dans les slides !

----

## Servlet

### Fichiers

- Le `Servlet` est une classe Java qui va être exécutée par le serveur web
- Le `.jsp` est un fichier similar à un `.html` qui va être exécuté par le serveur web

----

## Servlet

### `HttpServlet`

Tous les servlets doivent hériter de la classe `HttpServlet`

```java [3]

@WebServlet(name = "HelloServlet", value = "/hello")

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Des trucs à faire
    }
}
```

----

## Servlet

### Annotations

- `@WebServlet` : Indique au serveur web que la classe est un servlet
- `name` : Nom du servlet
- `value` : URL du servlet (chemin d'accès)

```java [1]

@WebServlet(name = "HelloServlet", value = "/hello")

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Des trucs à faire
    }
}
```

----

## Servlet

### `doGet`

`doGet` est une méthode qui est appelée par le serveur web lorsqu'une requête `GET` est reçue

```java [4-7]

@WebServlet(name = "HelloServlet", value = "/hello")

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Des trucs à faire
    }
}
```

----

## Servlet

### `HttpServletRequest`

- `HttpServletRequest` : Requête HTTP reçue par le serveur web
- L'objet contient toutes les informations de la requête

```java [5]

@WebServlet(name = "HelloServlet", value = "/hello")

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Des trucs à faire
    }
}
```

----

## Servlet

### `HttpServletResponse`

- `HttpServletResponse` : Réponse HTTP à envoyer au client
- L'objet contient toutes les informations de la réponse

```java [5]

@WebServlet(name = "HelloServlet", value = "/hello")

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Des trucs à faire
    }
}
```

----

## Servlet

### `doGet`

- Dans `doGet`, on peut générer du code HTML et l'envoyer au client

```java [6-7]

@WebServlet(name = "HelloServlet", value = "/hello")

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html"); // On indique au client que la réponse est du HTML
        response.getWriter().println("<h1>Hello World !</h1>"); // On envoie du code HTML au client
    }
}
```

----

## Servlet

### Le HTML

- On peut générer du code HTML dans `doGet` et l'envoyer au client
- On peut aussi utiliser des fichiers `.jsp` qui vont être exécutés par le serveur web

```java [6]

@WebServlet(name = "HelloServlet", value = "/hello")

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.getServletContext().getRequestDispatcher("/WEB-INF/nomDuFichier.jsp").forward(request, response);
    }
}
```

----

## Servlet

### `getServletContext`

- `getServletContext()` : Récupère le contexte de l'application
- Inutile de lui passer le dossier `webapp`

```java [6]

@WebServlet(name = "HelloServlet", value = "/hello")

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.getServletContext().getRequestDispatcher("/WEB-INF/nomDuFichier.jsp").forward(request, response);
    }
}
```

----

## Servlet

### `getRequestDispatcher`

- `getRequestDispatcher` : Récupère le dispatcher de la requête
- Permet de rediriger la requête vers un autre fichier

```java [6]

@WebServlet(name = "HelloServlet", value = "/hello")

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.getServletContext().getRequestDispatcher("/WEB-INF/nomDuFichier.jsp").forward(request, response);
    }
}
```

----

## Servlet

### `forward`

- `forward` : Prend en paramètre la requête et la réponse
- Il permet de passer les éléments de la requête à la page `.jsp`

```java [6]

@WebServlet(name = "HelloServlet", value = "/hello")

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.getServletContext().getRequestDispatcher("/WEB-INF/nomDuFichier.jsp").forward(request, response);
    }
}
```

---

# JSP

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## JSP

### Fichiers

- Les fichiers JSP sont à placer dans le dossier `webapp/WEB-INF`
- Les fichiers JSP sont similaires à des fichiers HTML
- Il sera possible d'utiliser des balises spéciales pour exécuter du code Java

----

## JSP

### Transmettre des attributs

- On peut transmettre des attributs à la page JSP en utilisant `request.setAttribute("nom", valeur);`
- Pour ce faire, on manipule l'objet `request` qui est passé en paramètre de `doGet`

```java [6]

@WebServlet(name = "HelloServlet", value = "/hello")

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setAttribute("nom", "Alexandre");
        this.getServletContext().getRequestDispatcher("/WEB-INF/nomDuFichier.jsp").forward(request, response);
    }
}
```

----

## JSP

### Récupérer des attributs

- Les balises `<% %>` permettent d'exécuter du code Java
- Ils sont similaires aux balises PHP `<?php ?>`
- Elles peuvent être utilisées pour récupérer des attributs

```xhtml
<% 
String nom = (String) request.getAttribute("nom"); 
%>
```

----

## JSP

### Afficher des attributs

- On pourra afficher les variables dans les balises `<%= %>`

```xhtml
<%
String nom = (String) request.getAttribute("nom");
%>
<%= nom %>
```

> Une fois la variable importée dans le document, on peut l'utiliser dans d'autres balises !

----

## JSP

### Afficher des attributs

- On peut aussi utiliser `out.print()` pour afficher des variables

```xhtml
<%
String nom = (String) request.getAttribute("nom");
out.print(nom);
%>
```

----

## JSP

### Exécuter du code Java

- N'importe quel code peut être exécuté dans les balises `<% %>`
- On peut donc utiliser des boucles

```xhtml
<%
String nom = (String) request.getAttribute("nom");
for (int i = 0; i < 10; i++) {
     out.print("Bonjour" + nom + "<br/>");
}
%>
```

----

## JSP

### Exécuter du code Java

- On peut aussi utiliser des conditions

```xhtml
<%
  LocalDateTime date = LocalDateTime.now();
  if (date.getHour() < 12) {
      out.print("Bonjour");
  } else {
      out.print("Bonsoir");
  }
 %>
```

----

# JSP

## Les inclusions

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## Inclusions

### Définition

- Les inclusions permettent d'inclure un fichier dans un autre
- Ils sont utiles pour rendre générique des morceaux de code HTML
- C'est utile pour éviter la duplication de code !

> Exemple : Un header et un footer !

----

## Inclusions

### Inclure un fichier

- Pour inclure un fichier, on utilise la balise `<%@ include file="nomDuFichier.jsp" %>`
- Le fichier doit être dans le même dossier que le fichier qui l'inclut

----

## Inclusions

### Exemple

`menu.jsp`

```xhtml
<nav>
    <ul>
        <li><a href="hello-servlet">Accueil</a></li>
        <li><a href="autre-page">Autre Page</a></li>
        <li><a href="encore-une-page">Encore une page</a></li>
    </ul>
</nav>
```

----

## Inclusions

### Exemple

- On pourra alors inclure le fichier `menu.jsp` dans les autres fichiers

```xhtml
<%@ include file="menu.jsp" %> <!-- Inclusion du fichier menu.jsp -->
```

---

# Expression Language

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## Expression Language

### Définition

- L'Expression Language (EL) est un langage de template
- Il permet d'insérer des variables dans du code HTML
- Il est utilisé dans les fichiers `.jsp`

----

## Expression Language

### Syntaxe

- L'EL est utilisé avec `${ }`
- Elle permet notamment d'insérer des variables dans du code HTML

```xhtml
<%
String nom = (String) request.getAttribute("nom");
out.print(nom);
%>
```

devient

```xhtml  
<h1>Bonjour ${nom}</h1>
```

----

## Expression Language

### Ternaire

- L'EL permet d'utiliser des ternaires

```xhtml
<p> ${date.getHour() < 12 ? "Bonjour" : "Bonsoir"} </p>
```

----

## Expression Language

### Tableaux

- L'EL permet d'accéder aux éléments d'un tableau

```xhtml
<p> ${noms[0]} </p>
```

----

## Expression Language

### Objets

- L'EL permet d'accéder aux attributs d'un objet
- Les objets sont appelés *JavaBeans*
- Pour définir un objet comme *JavaBean*, il faut :
    - Définir une classe
    - Définir des attributs privés
    - Définir des getters et des setters

----

## Expression Language

### Objets - Exemple

```java
public class Personne {
    private String nom;
    private String prenom;

    public Personne(String nom, String prenom) {
        this.nom = nom;
        this.prenom = prenom;
    }

    public String getNom() {
        return nom;
    }

    public void setNom(String nom) {
        this.nom = nom;
    }

    public String getPrenom() {
        return prenom;
    }

    public void setPrenom(String prenom) {
        this.prenom = prenom;
    }
}
```

----

## Expression Language

### Objets - Exemple

```java
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Personne personne = new Personne("Devos", "Alexandre");
        request.setAttribute("personne", personne);
        this.getServletContext().getRequestDispatcher("/WEB-INF/nomDuFichier.jsp").forward(request, response);
    }
}
```

```xhtml
<p> ${personne.nom} </p>
<p> ${personne.prenom} </p>
```

----

## Expression Language

### Aller plus loin

- Utiliser du code Java dans les fichiers `.jsp` n'est pas une bonne pratique
- Il est préférable de séparer le code Java et le code HTML
- On verra comment faire dans la suite du cours !

---

# JSTL

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## JSTL

### Définition

- La JSTL est une librairie qui permet d'utiliser des boucles et des conditions dans les fichiers `.jsp`
- L'utilisation passe par l'import de la librairie dans le fichier `.jsp`

----

## JSTL

### Importer la librairie : `pom.xml`

```xml

<dependencies>
    <dependency>
        <groupId>org.glassfish.web</groupId>
        <artifactId>jakarta.servlet.jsp.jstl</artifactId>
        <version>3.0.1</version>
    </dependency>
    <dependency>
        <groupId>jakarta.servlet.jsp.jstl</groupId>
        <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
        <version>3.0.0</version>
    </dependency>
</dependencies>
```

----

## JSTL

### Importer la librairie : `.jsp`

```xhtml
<%@ taglib prefix="c" uri="jakarta.tags.core" %>
```

- `prefix` : Préfixe à utiliser pour appeler la librairie
- `uri` : Chemin vers la librairie

----

## JSTL

### Afficher une variable

- L'instruction `<c:out>` permet d'afficher une variable

```xhtml

<c:out value="${nom}"/>
```

> Utile pour éviter les failles XSS !

Note: Faille XSS = Injection de code dans un formulaire

----

## JSTL

### Afficher une variable

- Il est aussi possible d'ajouter une valeur par défaut

```xhtml

<c:out value="${nom}" default="Inconnu"/>
```

----

## JSTL

### Définir une variable

- L'instruction `<c:set>` permet de définir une variable

```xhtml

<c:set var="nom" value="Alexandre" scope="page"/>
```

- `var` : Nom de la variable
- `value` : Valeur de la variable
- `scope` : Portée de la variable

----

## JSTL

### Les portées

- `page` : La variable est accessible uniquement dans la page
- `request` : La variable est accessible dans la page et dans les pages incluses
- `session` : La variable est accessible dans la page, dans les pages incluses et dans la session
- `application` : La variable est accessible dans la page, dans les pages incluses, dans la session et dans
  l'application

----

## JSTL

### Les Beans

Ils sont accessibles avec :

```xhtml

<c:set target="${nomDuBean}" property="nomDeLaPropriete" value="valeur"/>
```

----

## JSTL

### Supprimer une variable

- L'instruction `<c:remove>` permet de supprimer une variable

```xhtml

<c:remove var="nom" scope="page"/>
```

----

## JSTL

### Conditions

- On peut utiliser `if` pour exécuter du code si une condition est vraie

```xhtml

<c:if test="${requestScope.date.getHour() < 12}">
    <p>Bonjour</p>
</c:if>
```

> Avec cette syntaxe, on ne peut pas utiliser de `else`/`else if` !

----

## JSTL

### Enregistrer le résultat d'un test

- On peut utiliser `if` pour exécuter du code si une condition est vraie

```xhtml

<c:if test="${requestScope.date.getHour() < 12}" var="resultat">
    <p>Bonjour</p>
</c:if>
<c:out value="${resultat}"/>
```

----

## JSTL

### Conditions

- On peut utiliser `choose` pour exécuter du code en fonction d'une condition

```xhtml

<c:choose>
    <c:when test="${requestScope.date.getHour() < 12}"> <!-- Si la condition est vraie -->
        <p>Bonjour</p>
    </c:when>
    <c:when test="${requestScope.date.getHour() > 12}"> <!-- else if-->
        <p>Bonsoir</p>
    </c:when>
    <c:otherwise> <!-- Sinon -->
        <p>Coucou</p>
    </c:otherwise>
</c:choose>
```

----

## JSTL

### Boucles

- Il est aussi possible de créer une boucle de type `for`

```xhtml

<c:forEach begin="1" end="10" var="i" step="1">
    <li>${i}</li>
</c:forEach>
```

- `begin` : Valeur de départ (i = 0)
- `end` : Valeur de fin (i < 10)
- `step` : Pas (i++)
- `var` : Nom de la variable

----

## JSTL

### Boucles - Autre syntaxe

```xhtml

<c:forEach begin="1" end="10" var="i" step="1">
    <p>
        <c:out value="${i}"/>
    </p>
</c:forEach>
```

----

## JSTL

### Boucles

- On peut utiliser `forEach` pour parcourir un tableau

```xhtml

<c:forEach var="name" items="${requestScope.names}">
    <li>${name}</li>
</c:forEach>
```

OU

```xhtml

<c:forEach var="name" items="${requestScope.names}">
    <li>
        <c:out value="${name}"/>
    </li>
</c:forEach>
```

> `begin` et `end` peuvent aussi être utilisés

----

## JSTL

### Boucles - statut

- L'attribut `varStatus` permet d'accéder à des informations sur la boucle

```xhtml

<c:forEach var="name" items="${requestScope.names}" varStatus="status">
    <li>${status.index} : ${name}</li>
</c:forEach>
```

- `index` : Index de la boucle
- `count` : Nombre d'itérations
- `first` : Vrai si c'est la première itération
- `last` : Vrai si c'est la dernière itération

---

# JEE

## Les formulaires

![JEE](assets/JEE.png) <!-- .element width="30%" align="left" -->

![IntelliJ](assets/intellij.png) <!-- .element width="30%" align="right" -->

----

## Les formulaires

### Définition

- Comme pour tous les sites web, il est possible de créer des formulaires en JEE
- Ils permettent de récupérer des informations de l'utilisateur
- Ils sont utilisés pour créer des comptes, envoyer des messages, etc.

----

## Les formulaires

### Créer un formulaire

- Pour créer un formulaire, on utilise la balise `<form>`
- `action` : URL de la page qui va traiter le formulaire
- `method` : Méthode HTTP à utiliser pour envoyer le formulaire

```xhtml

<form action="form" method="post">
    <!-- Contenu du formulaire -->
</form>
```

> Ici, la méthode `doPost` de la servlet `form` sera appelée !

----

## Les formulaires

### Les champs

- Pour créer un champ, on utilise la balise `<input>`
- `type` : Type du champ
- `name` : Nom du champ
- `value` : Valeur du champ
- `placeholder` : Placeholder du champ

```xhtml

<form action="form" method="post">
    <label for="nom"> Nom :</label>
    <input type="text" name="nom" placeholder="Nom" id="nom"/>
    <input type="submit" value="Envoyer"/>
</form>
```

----

## Les formulaires

### Récupérer la valeur d'un champ

- Pour récupérer la valeur d'un champ, on utilise `request.getParameter("nomDuChamp")`
- Cette méthode doit être appelée dans la méthode `doPost` de la servlet

```java
public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    String nom = request.getParameter("nom");
    request.setAttribute("nom", nom);

    this.getServletContext().getRequestDispatcher("/WEB-INF/form.jsp").forward(request, response);
}
```

----

## Les formulaires

### Récupérer la valeur d'un champ

- On peut ensuite afficher la valeur dans la page JSP

```xhtml
<c:out value="Bonjour ${nom}" />
```

----

## Les formulaires

### Un formulaire plus complet

- On passe généralement par la création d'une classe pour représenter le formulaire
- On peut ensuite utiliser cette classe dans la servlet

```java
package fr.octocorn.helloworld;

public class User {
    private String nom;
    private String prenom;
    private String email;
    private String telephone;
    private String ville;

    public User(String nom, String prenom, String email, String telephone, String ville) {
        this.nom = nom;
        this.prenom = prenom;
        this.email = email;
        this.telephone = telephone;
        this.ville = ville;
    }

    public String getNom() {
        return nom;
    }

    public void setNom(String nom) {
        this.nom = nom;
    }

    public String getPrenom() {
        return prenom;
    }

    public void setPrenom(String prenom) {
        this.prenom = prenom;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getTelephone() {
        return telephone;
    }

    public void setTelephone(String telephone) {
        this.telephone = telephone;
    }

    public String getVille() {
        return ville;
    }

    public void setVille(String ville) {
        this.ville = ville;
    }

    public String getNomComplet() {
        return this.prenom + " " + this.nom;
    }
}
```

----

## Les formulaires

### Un formulaire plus complet

```xhtml
<h2> Formulaire d'identité</h2>
<form action="form" method="post">
    <label for="nom"> Nom :</label>
    <input type="text" name="nom" placeholder="Nom" id="nom"/>
    <label for="prenom"> Prénom :</label>
    <input type="text" name="prenom" placeholder="Prénom" id="prenom"/>
    <label for="email"> Email :</label>
    <input type="text" name="email" placeholder="Email" id="email"/>
    <label for="telephone"> Téléphone :</label>
    <input type="tel" name="telephone" placeholder="Téléphone" id="telephone"/>
    <label for="ville"> Ville :</label>
    <input type="text" name="ville" placeholder="Ville" id="ville"/>
    <input type="submit" value="Envoyer"/>
</form>
```

----

## Les formulaires

### Un formulaire plus complet

```xhtml

<style>
    body {
        margin: 0px;
    }

    .id-card-wrapper {
        height: 100vh;
        width: 100%;
        background-color: #122023;
        display: flex;
    }

    .id-card {
        flex-basis: 100%;
        border: 2px solid #0ae0df;
        max-width: 30em;
        margin: auto;
        color: #fff;
        padding: 1em;
        background-color: #0D2C36;
        box-shadow: 0px 0px 3px 1px #12a0a0, inset 0px 0px 3px 1px #12a0a0;
    }

    .profile-row {
        display: flex;
    }

    .profile-row .dp {
        flex-basis: 33.3%;
        align-self: center;
        position: relative;
    }

    .profile-row .desc {
        flex-grow: 66.6%;
        padding: 1em;
        font-family: 'Orbitron', sans-serif;
        color: #a4f3f2;
        text-shadow: 0px 0px 4px #12a0a0;
        letter-spacing: 1px;
    }

    .profile-row .desc h1 {
        margin: 0px;
    }

    .profile-row .dp img {
        max-width: 100%;
        border-radius: 50%;
        display: block;
    }

    .profile-row .dp .dp-arc-inner {
        position: absolute;
        width: 100%;
        height: 100%;
        border: 6px solid transparent;
        border-top-color: #0ae0df;
        border-radius: 50%;
        top: -6px;
        left: -6px;

        animation-duration: 1s;
        animation-name: rotate-clock;
        animation-iteration-count: infinite;
        animation-timing-function: linear;
    }

    @keyframes rotate-clock {
        from {
            transform: rotate(0deg);
        }
        to {
            transform: rotate(360deg);
        }
    }

    .profile-row .dp .dp-arc-outer {
        position: absolute;
        width: calc(100% + 12px);
        height: calc(100% + 12px);
        border: 6px solid transparent;
        border-bottom-color: #0AE0DF;
        border-radius: 50%;
        top: -12px;
        left: -12px;

        animation-duration: 1s;
        animation-name: rotate-anticlock;
        animation-iteration-count: infinite;
        animation-timing-function: linear;
    }

    @keyframes rotate-anticlock {
        from {
            transform: rotate(0deg);
        }
        to {
            transform: rotate(-360deg);
        }
    }
</style>
```

> Source HTML + CSS : https://codepen.io/ilybel/pen/BVGrKa

----

## Les formulaires

### Un formulaire plus complet

On peut alors appeler notre classe dans le `doPost` de la servlet

```java
public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    User user = new User(
            request.getParameter("nom"),
            request.getParameter("prenom"),
            request.getParameter("email"),
            request.getParameter("telephone"),
            request.getParameter("ville")
    );

    request.setAttribute("user", user);

    this.getServletContext().getRequestDispatcher("/WEB-INF/form.jsp").forward(request, response);
}
```

----

## Les formulaires

### Passer l'objet à la JSP

- Il est aussi possible de passer directement l'objet `request` complet à notre classe.
- C'est alors notre classe qui va récupérer les informations

```java
public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    User user = new User(request);

    request.setAttribute("user", user);

    this.getServletContext().getRequestDispatcher("/WEB-INF/form.jsp").forward(request, response);
}
```

----

## Les formulaires

### Passer l'objet à la JSP

- On peut alors utiliser l'objet dans la JSP

```java [8 | 8-14]
public class User {
    private String nom;
    private String prenom;
    private String email;
    private String telephone;
    private String ville;

    public User(HttpServletRequest request) {
        this.nom = request.getParameter("nom");
        this.prenom = request.getParameter("prenom");
        this.email = request.getParameter("email");
        this.telephone = request.getParameter("telephone");
        this.ville = request.getParameter("ville");
    }

    public String getNom() {
        return nom;
    }

    public void setNom(String nom) {
        this.nom = nom;
    }

    public String getPrenom() {
        return prenom;
    }

    public void setPrenom(String prenom) {
        this.prenom = prenom;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getTelephone() {
        return telephone;
    }

    public void setTelephone(String telephone) {
        this.telephone = telephone;
    }

    public String getVille() {
        return ville;
    }

    public void setVille(String ville) {
        this.ville = ville;
    }

    public String getNomComplet() {
        return this.prenom + " " + this.nom;
    }
}
```

---

## A vous de jouer !

Une petite to do list ?

---

# La suite !

[Retour à l'index](index.html)