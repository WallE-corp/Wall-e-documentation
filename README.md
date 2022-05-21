# WallE Software Design
In this documentation, any mention of "WallE" refers to our WallE robot.

# Requirements
* WallE
  - [x] [🔗](#requirement-be-able-to-run-autonomously-within-a-confined-area) Be able to run autonomously within a confined area.
  - [x] [🔗](#requirement-be-able-to-avoid-collision-objects-during-autonomous-operation) Be able to avoid collision objects during autonomous operation.
  - [x] [🔗](#requirement-be-able-to-accept-and-execute-drive-commands-given-by-a-remote-device) Be able to accept and execute drive commands given by a remote device.
  - [x] [🔗](#requirement-shall-use-a-camera-and-send-images-to-the-backend-when-collision-avoidance-occurs) Shall use a camera and send images to the backend when collision avoidance occurs.
* Mobile
  - [x] [🔗](#shall-take-user-input-and-translate-them-to-drive-commands-for-the-walle) Shall take user input and translate them to drive commands for the WallE.
  - [x] [🔗](#shall-visualize-the-path-travelled-by-walle-including-collision-avoidance-events) Shall visualize the path travelled by WallE including collision avoidance events.
* Backend
  - [x] [🔗](#publish-rest-api-for-reading-and-writing-position-data-from-walle) Publish REST API for reading and writing position data from WallE.
  - [x] [🔗](#rest-api-shall-contain-a-service-for-reading-and-writing-image-data) REST API shall contain a service for reading and writing image data.
  - [x] [🔗](#when-image-data-is-written-shall-perform-an-image-classification-via-for-example-google-api) When image data is written, shall perform an image classification via for example Google API.

# Contents
* [Architecture Overview](#architecture)
* [Requirement Breakdown](#requirements-breakdown)
* [WallE](wallE/index.md)
* [Mobile](mobile/index.md)
* [Backend](backend/index.md)

# Architecture
The architecture of the entire system can be divided into three major components `Backend`, `Mobile` and `WallE`. These major components, in turn, have their own sub-components. This chapter will provide an overview of each major component along with links to more detailed implementations. `LINK TO FIGURE`

### Major Components
## WallE
WallE or Mower is the major component that is considered the physical unit. This hosts everything for controlling low level movements, collision(obstacle) events, calculating relative position, etc. WallE has three sub components, each to take care of the low level requirements needed. WallE has two communication channel with the [Backend](#backend), HTTP requests and a socket connection. 

See [WallE](wallE/index.md) for implementation details.

## Backend
The backend or 'Backend' is the major component that handles data correction calculations, data storage, collision events handling, mediator between [WallE](#walle) and [Mobile](#mobile), path generation, etc. 
Its main application is a REST API which exposes endpoints for both [WallE](#walle) and [Mobile](#mobile). However, attached to the same appliation is a socker server which is also exposed.
The backend uses Google's [Cloud Firestore](https://firebase.google.com/docs/firestore) and [Cloud Storage](https://cloud.google.com/storage) for data storage. 

See [Backend](backend/index.md) for implementation details.

## Mobile
The app or 'Mobile' is the major component that deals with user interaction and interface. It displays visual feedback of [WallE](#walle)'s position and collision events. Mobile also provides an [WallE](#walle) control interface for manual/automatic control over [WallE](#walle).

See [Mobile](mobile/index.md) for implementation details.

![Architectural Overview](assets/architecture_overview.svg)

# Requirements Breakdown
This chapter lists each high level requirement and gives an overview of how it has been achieve. 

## WallE Requirments
* > ### Requirement: Be able to run autonomously within a confined area.
  Explain overall how it is achieved then link to implementation details

  ---

* > ### Requirement: Be able to avoid collision objects during autonomous operation.
  Explain overall how it is achieved then link to implementation details
  
  ---
* > ### Requirement: Be able to accept and execute drive commands given by a remote device.
  Explain overall how it is achieved then link to implementation details
  
  ---

* > ### Requirement: Shall use a camera and send images to the backend when collision avoidance occurs.
  Explain overall how it is achieved then link to implementation details

---

## Mobile Requirements
* > ### Shall take user input and translate them to drive commands for the WallE.
  Explain overall how it is achieved then link to implementation details
  
  ---

* > ### Shall visualize the path travelled by WallE including collision avoidance events.
  Explain overall how it is achieved then link to implementation details

---

## Backend Requirements
* > ### Publish REST API for reading and writing position data from WallE.
  Explain overall how it is achieved then link to implementation details
  
  ---


* > ### REST API shall contain a service for reading and writing image data.
  Explain overall how it is achieved then link to implementation details
  
  ---


* > ### When image data is written, shall perform an image classification via for example Google API.
  Explain overall how it is achieved then link to implementation details
  
  ---

