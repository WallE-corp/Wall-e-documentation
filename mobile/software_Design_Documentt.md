
**Software Design Document**

**Introduction**

In this document we are going to break down the different function and designs on the mobile part of the project.

**System overview**

The mobile part of the porject can be break down in two part.
The driving part and the vizualizer.

To do those two functionalities we had to be connected to the back-end via API call and sockets.

**Design consideration**

Before starting the design of the front part of the mobile app, we had to take into consideration a few things:
  - when the person is driving the robot we should not draw anymore the map, as this functionality is suppose to be used as a way for the user to transfer from one zone to another one.
  - and in the opposite way, we need to not be able to control the robot when it is in autonomous mode.
  -  another thing that we had to take into account is the fact that we don't know before starting the autonomous mode how big the map will be
  -  we also thought about showing the image we collid with (in the end we were not able to implement it in time).
 
 So with those constrain taken in account we decided to first create the design on figma (a collaborative app like google doc, to create desings).
 ![Figma](assets/figma?raw=true "Figma")
 
 To answer the first two needs we decided to create a button that will activate one or the other part, and will send a message to the back end about the change of state., and decided to add a place to draw.
 
 **Development method**
 
 *Choices*
 
 We decided to code with react native as it is a language we already new, and react native offers the possibility to easily test on the phone without having to build, it also allows to refresh live the mobile, without having to compile.
 
 We decided to not make the direction buttons accesible when in automatic mode so the user is not lost, and to prevent any problems due to a message being sent via socket about a direction.
 
 *Robot autonomous mode*
 
 In autonomous mode we need to be able to draw the path of the robot and were he collided with objects.
 Something that we had to take into account doing this is the fact that we don't know before starting the autonomous mode how big the map will be. To be able to adapt we decided to have a fix size on the screen on which to draw, we then, everytime we receive datas, search for the biggest X and y, and smallest ones, we then devide the number of pixels by this number, this gives us how big one coordinate will be translated in pixel.
 
 ```arr.forEach((element, index) => {
      if (element.x > tempHightX)
        tempHightX = element.x
      if (element.x < tempLowX) {
        tempLowX = element.x
      }
      if (element.y > tempHightY)
        tempHightY = element.y
      if (element.y < tempLowY) {
        tempLowY = element.y
      }
    });

    sizeMaxX = (tempHightX) - (tempLowX)
    sizeMaxY = (tempHightY) - (tempLowY)
    size1pX = sizeMaxX / 300;
    size1pY = sizeMaxY / 400;
    ```

We also had to check if some numbers were negative as our axis to draw were only positive. If there is a negative number we will then add the obasolute value to all coordinates.
```    if (tempLowX < 0)
      makePosX += (-1) * tempLowX
    if (tempLowY < 0)
      makePosY += (-1) * tempLowY```
      ```      x2 = ((element.x + makePosX) / size1pX);
      y2 = ((element.y + makePosY) / size1pY);
 ```
      
      We then draw thanks to a library call svg and create an array of lines, that we then draw.
```temp.push(<Line x1={x1} y1={y1.toString()} x2={x2.toString()} y2={y2.toString()} stroke="red" strokeWidth="2" />);
```

To receive all those data, we use a socket connexion and created and infinite loop that call the back-end road to get the points every 0.5 seconds.
```const getPathpoints = () => {
    fetch("http://13.49.136.160:3000/pathpoints/", {
      "method": "GET",
    })
      .then(response => response.json())
      .then(response => {
        let list = [];
        response.map(item => {
          list.push(item.coordinates);
        })
        setData(list);
        createMap(list, datasObj);
      })
      .catch(err => {
        console.log(err);
      });
  }
  ```

To draw the obstacles we are in the same function as the one drawing the path as we apply the same multiplying factor to make the coordinates adapt to the size of the map, and we draw thanks to the circle option of the svg library
```cx = (element[0] + makePosX) / size1pX
        cy = (element[1] + makePosY) / size1pY
        temp1.push(<Circle cx={cx.toString()} cy={cy.toString()} r="5" stroke="black" strokeWidth="2" />);
        ```
        
We get the obstacles via a socket call that the back end sends.

*Manual control*

To do the manual control we create four buttons, that are activated or not depending on the mode.
When pressed we call a function that sends a message via socket to the back end. On first press it sends start, on second press stop.
Here is the function to turn left 
```
const Left = () => {
    if (leftState == false) {
      const data = {
        "type": 4,
        "data": {
          "movement": "left",
          "action": "start"
        }
      }
      console.log("left start");
      socket.emit('message', JSON.stringify(data));
      setleftState(leftState => !leftState);
    } else {
      const data = {
        "type": 4,
        "data": {
          "movement": "left",
          "action": "stop"
        }
      }
      console.log("left stop");
      socket.emit('message', JSON.stringify(data));
      setleftState(leftState => !leftState);
    }
  }
  ```
