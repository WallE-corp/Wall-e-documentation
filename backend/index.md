# Backend Software Design Description

## Contents
* [Introduction](#introduction)
* [Architecture](#architecture)
* [Mid-Level Requirements Breakdown](#mid-level-requirements-breakdown)
* [Node Application](node_application.md)
* [Database](database.md)

## Introduction

## Architecture
The backend architecture can mainly be broken down into two parts, a running `Node.js application` and the `databases hosted by google`. The Node.js application holds both a REST API and a Socket Server. It is what communicates with the other WallE and Mobile major components. This allows for resource data on request, but also a constant comminication channel. The Node.js application is also hosted by the us (the development team). For data storage, Google's [Cloud Firestore](https://firebase.google.com/docs/firestore) and [Cloud Storage](https://cloud.google.com/storage) is used. Cloud Firestore being a document database and Cloud Storage for raw data storage. Being google services, they are accessed over the internet.


![Backend Architecture](../assets/backend_software_design.svg)


## Requirements Breakdown
Breakdown the high-level requirements into what is specifically required. 

* > ### Publish REST API for reading and writing position data from WallE.
  #### Breakdown
  1. Need to set up an http server.
  2. Need to expose routes on that server.
  3. Need to persist data.
  
  ### Solution
  1. An http server is set up using the [ExpressJs](https://expressjs.com/) web application framework running on the [Node.js](https://nodejs.org/en/) platform. Node.js and ExpressJs were primarily choosen based on team competence.
  
     See [REST API implementation]().

  2. Using the ExpressJs framework, route endpoints were created and exposed. For Reading and Writing, the routes exposed are `GET` & `POST` on `/pathpoints`.
      
     See [REST API endpoints]().

  3. For data persistence, Google's [Cloud Firestore](https://firebase.google.com/docs/firestore) was chosen. It is a document database which when setup is hosted by google. This was decided upon by the team for being perceivably easy to use and does not require own database hosting.

      See [Database](database.md).  

---


* > ### REST API shall contain a service for reading and writing image data.
  #### Breakdown
  1. Need to expose new endpoints. 
  2. Need to handle storing image data.

  #### Solution
  1. Using the ExpressJs framework, a new route endpoint was created, that being `POST` `/obstacle/event`.

      See [REST API endpoints]().
  
  2. Cloud Firestore does not allow storing images. So to store image Google's [Cloud Storage](https://cloud.google.com/storage). This allows to storage image files and access each specific one via a unique url. Cloud Storage also can be set up together with Cloud Firestore.

    See [Database](database.md)

  ---


* > ### When image data is written, shall perform an image classification via for example Google API.
  #### Breakdown
  1. Need to find an available API that can classify images.
  2. Need some way of storing the classification given to an image.

  #### Solution
  1. Google's [Cloud Vision API](https://cloud.google.com/vision) was chosen as a solution. It takes an images as an input and returns a list of labels of what it thinks the image is. It was also chosen because it is part of Google Cloud Platform which the [Database](database.md) solutions are also a part of.
  2. Using the Firestore database in [Database](database.md) a new collection of documents is created to hold _Obstacle Events_ (i.e. collision events). Everytime an image is uploaded, it gets classified and a new obstacle event document is created holding: A link to the image source, the image classification lable, and the obstacle position.
  
  ---

### Special Case Requirement - Movement Controls
The chosen solution for manual control of WallE from Mobile was through a socket connection. As the backend already takes the roll of a central control base, it was chosen to host the socket server with which WallE and Mobile would connected to. However, there are pros and cons with this descision:
* pros
  - WallE and Mobile do not need to be connected to the same network.
  - Niether WallE nor Mobile needs to host a socket server, which may causes issue with battery levels and processing power.
  - Distance between WallE and Mobile does not matter.
  - Socket Connection can be used by backend to push out data to Mobile and WallE.
* cons
  - For manual control, having movement commands run first through the backend causes slight extra delay.
  - Requires stable internet connection.

#### Breakdown
1. Backend needs to host a Socket Server.
2. Socket connection needs a protocol to correctly recognize messages and data.
3. Backend needs to know which connect clients is WallE and which is Mobile.
4. Backend needs to forward movement commands to WallE.

#### Solution
1. For setting up a socket server, [Socket.IO](https://socket.io/) was used. This allows us to bipass the details of low level socket protocol and setup. Socket.IO is a well estable solution for bidirectional and low-latency communication that has solutions in `javascript` which is used on the backend and Mobile, but also for `python` which is used on WallE.

2. A protocol was decided upon by the team. Data is sent as stringified JSONs. Each data object is marked with a property to say what type of message it is, it also carries a payload of data related to the type of message. 

   See [Socket Connection Protocol](socket_connection_protocol.md) for details on the communication protocol.

3. A specific command was created for both WallE and Mobile to use to _register_ themselves as either `wall-e` or `remote`. Upon receiving these command through the socket connection, the backend keeps track of which connected clients are Mobile and WallE.

   See [Register Command](socket_connection_protocol.md#register-socket-client).

4. Once the backend knows which of its clients is WallE and which is Mobile. Once message is received from Mobile, using the connection protocol the backend can parse the message and send the same data to the client marked as WallE.

   See [Movement Commands](socket_connection_protocol.md#movement-commands).