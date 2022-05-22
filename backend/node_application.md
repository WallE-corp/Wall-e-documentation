# Node Application

## Contents
* [Architecture](#architecture)

## Architecture
The node application --or just application-- is structure using a [Three Layered Architecture](https://medium.com/@deanrubin/the-three-layered-architecture-fe30cb0e4a6) which consists of a `Presentation Layer` -> `Business Logic Layer` -> `Data Access Layer`. This allows for more clean separation of code which in turn helped development, testing, documentaiton, management, etc. The Presentation Layer is what is presented to the outside (to WallE and Mobile), for the REST API is handles incoming requests, and for the Socket Server it handles bidirectional communicatioin. The Presentation Layer calls upon the Business Logic Layer to handle calculations. The Business Logic Layer handles calculating and correction position data, managing obstacle events, etc. When data needs to be stored or accessed, the Business Logic Layer uses the Data Access Layer. The Data Access Layer manages the connection to the Cloud Firestore and Cloud Storage databases. It holds all the functions necessary for manipulating the data in the database.

![Application Architecture](../assets/node_application_architecture.svg)


### Source Tree View
Here is a tree strucutred view of the `src/` folder in the backend repository. The `src/` holds the Node.js application source code.

![Architecture Minor Components View](../assets/node_application_tree_view.svg)
