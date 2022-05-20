# Software Design Description for Wall-E

## Source code components for the Arduino:

### The following functions accompany the creation of an mblock project and are essential for operating its components
void isr_process_encoder1(void);

void isr_process_encoder2(void);

void move(int direction, int speed);

void _delay(float seconds);

void _loop();

void loop();

### The following functions are called during manual driving to handle the Wall-E movement, all the functions with the exception of Stop_Moving run until designated otherwise.

void Move_Forward(); | This function calls the basic move function to drive forward by settings its direction.

void Turn_Left(); | This function sets the PWM for both motors in order to facilitate a left turn.

void Stop_Moving(); | This function sets both encoder motors to zero thus stopping Wall-E momentum.

void_Turn_Right(); | This function sets the PWM signal for both motors in order to facilitate a right turn.

void Move_Backward(); | This function calls the basic move function to drive backwards by setting its direction.

### The following fucntions are called to facilitate autonomous running.

bool checkObstacle(); | This function utilizes the ultrasonic sensor to avoid collision objects by checking if an obstacle event has occured or not. If true it then performs a pre-determined set of moves of reversing up and turning before continuing forward. Additionally it sets the variable object to either detected or no object detected.

void checkBorder(); | This function utilizes the linefollower sensor to ensure that it remains within the confined area. If the border is detected it performs a pre-determined set of moves of reversing and turning before continuing forward. Additionally it sets the object variable to either border detected or no object detected.

void waitUntilStopped(); | This function ensure that all motors are stopped before continuing.


### The following function is the main function that runs Wall-E

void setup(); | This function first of initializes the serial communication between the Arduino and Raspberry pi. It then initializes the physical mbot components which are being employed. If a command is available through the serial communication, depending on the command it runs through the switch case and acts accordingly. Once done the arduino calculates its current velocity based upon the rotation of the wheels alongside the angle retrieved by the gyroscope. It then sends the object string alongside both the x and y distances traveled to the Raspberry pi.

## High level requirements & Low level requirements:

**1: The Mower shall be capable of running autonomously within a confined area.**
1.1: The Arduino shall be capable of detecting when it has encountered the border line for the confined area.
1.2: The Ardunio shall be capable of handling a border detection through a pre-determined set of moves to remain within the confined area.

**2: The Mower shall be able to avoid collision objects during automous operation.**
2.1: The Arduino shall be capable of detecting when it has an object in front of it.
2.2: The Ardunio shall be capable of handling an obstacle event through a pre-determined set of moves to avoid collision. 

**3: The Mower shall be able to accept and execute drive commands given by a remote device.**
3.1: The Arduino shall be capable of receing commands from the Raspberry Pi through serial communication.
3.2: The Arduino shall have a function for Moving Forward.
3.3: The Arduino shall have a function for Turning Left.
3.4: The Arduino shall have a function for Turning Right.
3.5: The Arduino shall have a function for Reversing.
3.2-5: The Arduino shall have functions to facilitate drive commands.
3.6: The Arduino shall be capable of executing the command received through movement functions.
3.7: The Raspberry Pi shall be capable of receiving commands through the socket connection.
3.8: The Raspberry Pi shall be able to send movement commands to the Arduino through serial communication according to command received. 

**4: The Mower shall use a camera and send images to the backend REST API when collision avoidance occurs.**
4.1: The Arduino shall be capable of sending data through serial communication.
4.2: The Raspberry Pi shall be capable of capturing a picture through its camera when an obstacle event occurs.
4.3: The Raspberry Pi shall be capable of sending an obstacle event to the backend when encountered.

## Source code components for the Raspberry Pi(main file):

Checks if a new command has been received or not. If true forward the appropriate command through the serial communication line to the Arduino.

Checks if there is a command on the line. If true it decodes the data received into 3 separate lines: If there is an object or not as well as the distance traveled in the x and y directions.  

It then handles the handles the position data through the handle_position_data function.

If an object was detected it instructs the camera to capture a picture with a designated file name. It then creates an obstacle event compiled of this picture and alongside its coordinates. It then uploads this obstacle event through upload_obstacle_event to the backend.
