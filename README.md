Pour lancer le serveur, la base de données et swagger.

On doit lancer ces 3 fichiers :
`run-hsqldb-server.bat ou .sh`
`show-hsqldb.bat ou .sh`
`RestServer.java`

# Domain

Pour réaliser ce projet, nous avons d'abord commencé par créer le domain, qui regroupe toutes nos classes principales permettant de modéliser les différents objets : **Utilisateur, Organisateur, Administrateur, Evenement, Stats et Ticket**.

Chaque objet est une **entité** qui sera créée dans notre base de données.

Les classes **Administrateur** et **Organisateur**, qui héritent de **Utilisateur**, possèdent un `@DiscriminatorValue`. Elle permet d'indiquer, lors de la création d'un administrateur ou d'un organisateur, le type spécifique de l'utilisateur dans la colonne `type_utilisateur`.

# Dao

Dans un second temps, nous avons poursuivi avec la mise en place des classes **DAO.** Elles permettent d’interagir avec la base de données et d’exécuter des opérations **CRUD** sur les entités.

Grâce à `extends AbstractJpaDao`, les classes bénéficient de la structure générique proposé par JPA qui permet d’effectuer des requêtes sans avoir à réécrire la logique et la persistance.

Créer ces classes nous permet également d’isoler la logique d’accès aux données et de séparer les responsabilités dans l'application.

# Service

Nos classes service nous permettent de centraliser la logique métier, ce qui nous permet de créer nos différents besoinspour poursuivre l’application.

Elles servent d’intermédiaire entre la couche DAO et la couche Ressource.

# Dto

La couche DTO permet de restituer uniquement les données nécessaires, contrairement à la DAO, qui retourne l’intégralité des données de l’objet.

Cela permet de limiter l’exposition de données sensibles et améliore la sécurité en évitant d’exposer directement la structure de la base de données.

Les données renvoyé sont sous un format json.

# Ressource

Les classes ressource permettent d’interagir avec la partie web. Elles interagissent avec les classes services et les requêtes HTTP des clients. On utilise les méthodes des services pour récupérer les données souhaitées.

# JPATEST

Dans cette classe, nous avons créé des utilisateurs, des événements, etc. Par la suite, si nous avons besoin de créer de nouveaux éléments, nous passerons par Swagger, pour des raisons de simplicité.

# Swagger

Pour accéder à swagger il faut aller sur l’url :

http://localhost:8080/api/

# Pom.xml

Dans le fichier `pom.xml`, nous avons ajouté plusieurs plugins de reporting pour assurer la qualité du code. Ces plugins incluent :

- **maven-javadoc-plugin** : génère la documentation du code source.
- **maven-pmd-plugin** : détecte les erreurs potentielles et les mauvaises pratiques.
- **maven-checkstyle-plugin** : applique les règles de style de code.
- **maven-jxr-plugin** : facilite la navigation dans le code source.

Pour générer les rapports, on utilise la commande `mvn clean sit`:

On obtient le rapport sur la page html qui se trouve :  `target/site/index.html`.

# TestApplication

Elle permet de configurer les ressources de l’application.

L'annotation `@ApplicationPath("/")` définit le chemin racine de l'API, ce qui signifie que toutes les ressources seront accessibles à partir de l'URL de base du serveur.

Si on rajoute une classe nous devons le spécifier ici.

Grâce à ça l’app peut gérer des requêtes REST et générer la documentation pour swagger.

# RestServer

La classe `RestServer` initialise et démarre un service sur le port **8080**. Elle déploie l'application `TestApplication`.