# WallE Software Design

# Requirements
* Mower
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
* Architecture Overview
* Mower
* Mobile
* Backend

# Architecture
**[NOTE] explain overall architecture here**


![Architectural Overview](assets/Overall_architecture_WallE.png)

# Requirements Breakdown
This chapter lists each high level requirement and gives an overview of how it has been achieve. 

## Mower Requirments
### Requirement: Be able to run autonomously within a confined area.
Explain

### Requirement: Be able to avoid collision objects during autonomous operation.
Explain

### Requirement: Be able to accept and execute drive commands given by a remote device.
Explain

### Requirement: Shall use a camera and send images to the backend when collision avoidance occurs.
Explain

---

## Mobile Requirements
### Shall take user input and translate them to drive commands for the WallE.
Explain

### Shall visualize the path travelled by WallE including collision avoidance events.
Explain

---

## Backend Requirements
### Publish REST API for reading and writing position data from WallE.
Explain


### REST API shall contain a service for reading and writing image data.
Explain


### When image data is written, shall perform an image classification via for example Google API.