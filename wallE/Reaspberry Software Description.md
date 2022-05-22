# WallE Software Design Description

## Contents
* [Overview](#overview)
  * [Sub Components](#sub-components)
* [Mid-Level Requirements Breakdown]()


## Overview
WallE is the physical unit, it has three sub components a `Raspberry PI`, an `Arduino`, and the actual `Robot`. The Raspberry PI handles communication with the backend, it also has a camera attached and takes care of snapping photos of obstacles. The Raspberry has close communication with the Arduino. The Arduino handles controlling the movements of the Robot. The Arduino also takes care of reading data from various sensors which allows it to calculation position, detected obstacles, and detect the bounrdies of the confined area.


![WallE Overview](../assets/wallE_software_design.svg)

## Raspberry Requirements Breakdown
As noted, the Raspberry handles communication with the backend. For the requirements breakdown of the Arduino and Robot see [here](Software%20Design%20Description.md#high-level-requirements-&-low-level-requirements:).

* > ### Requirement: Be able to run autonomously within a confined area.
  **This requirement does not impose any requirements on the raspberry pi.**

  See [here](Software%20Design%20Description.md#high-level-requirements-&-low-level-requirements:) for arduino breakdown.

  ---

* > ### Requirement: Be able to avoid collision objects during autonomous operation.
  **This requirement does not impose any requirements on the raspberry pi.**
  
  See [here](Software%20Design%20Description.md#high-level-requirements-&-low-level-requirements:) for arduino breakdown.

  ---
* > ### Requirement: Be able to accept and execute drive commands given by a remote device.
  A drive commands was decided upon to be sent via a socket connection from the backend. The backend hosts a socket server to which clients can connect.
  
  #### Breakdown
  1. Needs to have a socket client connection with the backend.
     1. Because of backend, socket client needs to abide by the [Socket.IO](https://socket.io/) protocol.
  2. Be able to receive commands from backend.
  3. Needs to parse incoming messages and act accordingly to movement commands.
  4. Needs to have an open communcation channel with the arduino to forward the movement commands to. 
  
  #### Solution
  1. The python module [python-socketio](https://python-socketio.readthedocs.io/en/latest/) was used to create a base socket client as its already in compliance with the Socket.IO protocol.
  2. Upon an established socket connection with the backend, sends a [registration command](../backend/socket_connection_protocol.md#register-socket-client) to register as `wall-e` which tells the backend to forward movement commands to this client.
  3. Using the python [json](https://docs.python.org/3/library/json.html) module and following the [socket connection protocol](../backend/socket_connection_protocol.md) messages can be parsed.
  4.  Communication with the arduino is handled through a serial channel. See [here](Software%20Design%20Description.md#source-code-components-for-the-raspberry-pi(mainfile))

  ---

* > ### Requirement: Shall use a camera and send images to the backend when collision avoidance occurs.
  #### Break down
  1. Snap photo when obstacle detected message received from arduino.
  2. Send photo to backend.

  #### Solutioin
  1. Once message received from arduino for obstacle detected. Uses the [picamera](https://picamera.readthedocs.io/en/release-1.13/) python module to get access and take a picture with the camera connected to the raspberry pi. The photo is saved locally with a set filename.
  2. The backend has endpoint at `obstacle/event` for sendning obstacle images. An http request is made to the backend using the [requests](https://docs.python-requests.org/en/latest/)
