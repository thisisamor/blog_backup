---
title: EE Project Work Diary
date: 2023-05-26 19:14:13
categories: Project
tags:
  - college
  - project
description: My day-to-day work diary of my college project. 
---
<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;"> Our second end-of-year project is to build a dynamically balanced rover which is capable of exploring and solving a irregular maze through camera detection and analysis, and present the result on a website with the help of reliable server communication and database. </small>

---

## Week 0

### May 18th (Thu)

<mark style="background-color: #7099a7;">Year 2 project introduction. </mark>

#### Functional Requirements

Autonomously move through a maze without crossing an illuminated line

1. Autonomously survey the layout of the maze and produce a map of the
discovered layout, overlaid with the position of the robot and the shortest
path through the maze
2. Balance on two wheels

#### Non-Functional Requirements

1. Reliable and able to complete its task without human intervention
2. Robust and efficient construction
3. Coded for: a. Usability b. Testability c. Maintainability d. Scalability

#### Year 1 and Year 2 Content

Prototyping (labs)

Test and diagnosis (labs)

Software Skills (C++, AWS, node.js, React, MySQL)

Embedded Programming

Control Systems

Digital systems (FPGA, Verilog, NIOS II)

Power systems

#### Constraints

The project has constraints to maintain the level of challenge:

1. The permitted means of locating the robot within the maze are:
- Dead-reckoning based on accelerometer, gyroscope and wheel revolution
counting
- Optical detection of the maze markings and up to three illuminated beacons by
the robot
2. Illuminated beacons shall be powered by a specified PV-array emulator and provided
energy conversion modules
3. The robot will use the provided FPGA-based camera system

![Arena illustration](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/arena-illustration.png?raw=true)

![rover mode](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/rover-mode.png?raw=true)

---

### May 19th (Fri)

<mark style="background-color: #7099a7;">First group meeting, discussed about the project requirements, went up with the basic idea of rover algorithm, listed and assigned tasks. </mark>

#### Tasks

1. balance (mechanical, dynamic) motor controller (Tracey & Nicolas)
2. solar powering - EEE (Tracey & Nicolas)
3. fpga (camera control) - EIE  (Daisy)
4. ESP32 microcontroller - (Amor)
    - Best route finding algorithm 
5. AWS server - EIE (Jessica)
    - ( database? )
6. website ( refresh? ) front end - EIE (Annie)
7. Data processing (visual detection) - EIE (needed for every task)

#### Initial plan

A very basic algorithm includes: 

1. Store data in matrix ( blocks / walls detected )
2. detecting the three adjacent blocks
    1. any walls?
    2. new blocks? 
    3. IF only one possible path remains: exploring algorithm can be optimised

For the above algorithm, the following assumptions were made:

1. **the rover is capable of calculating the distance it have moved (Distance moved can be calculated using the number of rotations of the stepper motors) in terms of blocks**
2. the rover can only detect the nearest block 
    1. need to find out whether there is better algorithm IF the camera has a wider view - test camera first
3. the beacons are only used to mark the corners

---

### May 20th (Sat)

<mark style="background-color: #7099a7;">Went though basic JavaScript tutorial, set up AWS server environment for node.js, express and dynamoDB. Designed basic polling algorithm. </mark>

#### Server checklist

- [x]  set up EC2 instance (IP Lab 5 & 6)
- [x]  install node.js on the EC2 instance  ([Tutorial](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html))
- [x]  Run HTTP server on AWS using node.js
    - [x]  `$npm install express`
    - [x]  `$npm install body-parser`
    - [x]  `$npm install node-html-parser`
    - [x]  types of HTML response ( table / hyperlink / tags )
- [x]  (Use Express framework) (a web application framework for Node.js which makes the web application code much easier to maintain and extend)
    - [x]  Data processing on server side
        - [x]  parsing
        - [x]  reconstructing
    - [ ]  (Use react-polling)
        - [x]  `npm i react-polling save`
- [ ]  Use DynamoDB (FOR WHAT)
- [ ]  Best route-finding algorithm (after the map is completed)

---

## Week 1

### May 22nd (Mon)

<mark style="background-color: #7099a7;">Wrote code for ESP32 microcontroller to connect to WiFi. Tested with mobile phone hot spot. Set up VS code environment for arduino platform (bcs it looks better). </mark>

<mark style="background-color: #7099a7;">Established and tested D8M camera with FPGA connection. (Can see the image from laptop now.) </mark>

#### ESP32 WiFi connection

main set up: 
```
#include <Arduino.h>
#include <string>
#include "wifi_connection.h"

void setup() {
  Serial.begin(9600);
  wifi_connection(); 
}
```

wifi connection function: 
```
#include <string>
#include <Arduino.h>
#include <ArduinoJson.h>
#include <WiFi.h>
#include <HTTPClient.h>

const char* ssid = "ssid";
const char* password = "password";

// ----- connect to wifi -----
void wifi_connection()
{
  Serial.println("hello"); 
  WiFi.begin(ssid, password);
  Serial.println("start to connect");
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected to WiFi");
}
```

---

### May 23rd (Tue)

<mark style="background-color: #7099a7;">Wrote and tested HTTP POST request with a simple AWS web server. </mark>

POST request sent by the rover: 
```
// ------- POST request ------------------
void client_post()
{
  // Serial.print("[HTTP] begin...\n");
  // HTTPClient http; 
  // configure traged server and url
  // http.begin("http://35.176.36.0:8080/"); //HTTP
  // Serial.print("[HTTP] POST...\n");
  // int httpCode = http.POST("something");

  // Create JSON payload
  DynamicJsonDocument jsonPayload(128); // the capacity of the JSON document
  
  // TODO: change jsonPayload keys to the ones we want to use in database

  String i, j, UP, DOWN, LEFT, RIGHT; // input parameters
  // change these with processed data from camera
  jsonPayload["i"] = "0"; 
  jsonPayload["j"] = "2"; 
  // jsonPayload['UP'] = UP; 
  // jsonPayload['DOWN'] = DOWN; 
  // jsonPayload['LEFT'] = LEFT; 
  // jsonPayload['RIGHT'] = RIGHT; 

  // Serialize the JSON document to a string
  String payload;
  serializeJson(jsonPayload, payload);

  // Send POST request with JSON payload
  HTTPClient http;
  http.begin("http://35.176.36.0:8080/");
  http.addHeader("Content-Type", "application/json");
  int httpCode = http.POST(payload);

  if(httpCode > 0) 
  {
    Serial.printf("[HTTP] GET... code: %d\n", httpCode);
    if(httpCode == HTTP_CODE_OK) 
    {
      String payload = http.getString();
      Serial.println(payload);
    }
  } 
  else 
  {
    Serial.printf("[HTTP] GET... failed, error: %s\n", http.errorToString(httpCode).c_str());
  }

  http.end();
  // delay(5000);
}

// -------- GET request --------- (for testing only)
void client_get() 
{
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    // Specify the target URL
    http.begin("http://35.176.36.0:8080/");
    // Send the HTTP GET request
    int httpResponseCode = http.GET();
    if (httpResponseCode == HTTP_CODE_OK) 
    {
      // Successful response
      String payload = http.getString();
      Serial.println(payload);
    } 
    else 
    {
      // Error in HTTP request
      Serial.print("Error code: ");
      Serial.println(httpResponseCode);
    }
    // Close the connection
    http.end();
  }
  // Wait for 5 seconds before making the next request
  delay(5000);
}
```

Code for testing the connection between ESP board and server: 
```
// when ESP is connected to wifi
// it should be able to send a POST request to the server
// and receive 200 OK as the http code

var express = require('express');
var server = express();
const port = 8080; 

// web content
let htmlContent = `
<!DOCTYPE html>
<html>
 <body>
 <h2>This is a test message</h2>
 <!-- this is a comment: br tag is for line break-->
 </body>
</html>
`;

server.use(express.json());

server.get('/', function(req, res) {
    res.writeHead(200, {'Content-Type':'text/html'});
    res.end(htmlContent);
});  

server.post('/', (req, res) => {
  const postData = req.body;
  // data processing example
  const responseContent = "<p>Received data: " + postData.i + postData.j + "</p>";
  // Send a response back to the client
  res.send(responseContent);
});

server.listen(port, () => {
  console.log(`Server listening on port ${port}`);
}); 
```

---

### May 24th (Wed)

<mark style="background-color: #7099a7;">Met Ed Stott, who instructed more on maze complexity. </mark>

<mark style="background-color: #7099a7;">Tested camera detection: when approaching to a wall, set the wall flag to HIGH. (Visual detection was done by setting restrictions on the region of pixels whose colour code is considered as white.) </mark>

<mark style="background-color: #7099a7;">Established connection between FPGA output and ESP32 input, encountered problem when using digitalRead on the arduino side. (The voltage output was as expected according to the value measured with a multimeter, but ESP32 constantly read a HIGH input). </mark>

#### Maze complexity

> The robot can locate itself by measuring the angle between the observed positions of the three beacons, when the positions of the beacons are known in advance. The camera is probably the simplest way of observing the beacons.
> The maze will always start in one corner and finish in the opposite corner. It's approximately 2.4m x 3.6m. The perimeter of the maze has a light strip.
> The walls will be (nominally) straight. They won't be absolutely straight because the corners have a minimum bend radius, but you can map them as straight lines. 
> Some walls will not be parallel with any side of the arena. The smallest gap will be 350mm. 
> There could be multiple paths to solve the maze, aka “floating islands” inside. 

<small style="opacity: 0.7;">I'm not satisfied with this maze complexity specification thing at all. Our department literally had three weeks during our final exams to come up with ideas of our summer project. But in the end, they gave us this briefing with very few components ready and they had no idea what the maze should be like. They initially wanted to build a test arena with blocks and lighting strips where the walls are all parallel to the sides. It was a few days later when they realised that students were planning to map the whole arena as a matrix and decided this would be too straight-forward. They changed the whole arena to make it irregular - and impossible if not discretely calculate the location of the rover. 

I'm very glad to have some challenging tasks, but it made me a bit frustrated that they didn't prepare everything properly and i had to re-plan our algorithm for so many times. </small>

![test arena photos](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/test-arena-final-photo.jpg?raw=true)

---

### May 25th (Thu)

<mark style="background-color: #7099a7;">Solved yesterday's problem (by applying FPGA cap board pin mapping), tested ESP32 digital read pin from FPGA (a flag showing whether or not a wall is detected). Tested digital write to control FPGA key (zoom in / zoom out / focus). </mark>

<mark style="background-color: #7099a7;">Changed the basic wall detection into more specific requirements: detecting whether or not the wall is horizontal in the camera view. </mark>

<mark style="background-color: #7099a7;">Planned new maze solving algorithm on updated maze complexity rules and current ability of camera detection (based on experiment). </mark>

#### ESP32 checklist (updated)

- [x]  Downloaded Adafruit MPU6050 Arduino Library 
- [x]  Installed the PlatformIO VS Code Extension 
- [ ]  maze algorithm
    - [ ]  data structure
    - [ ]  optimisation?
        - [ ]  UART communication → calculate wall angle faster?
        - [ ]  calculate distance faster?
        - [ ]  more efficient data structure
- [ ]  camera detection
- [ ]  motor command
- [ ]  location calculation
- [x]  wifi + http POST request

#### Pin mapping

| FPGA pin | cap board pin | ESP 32 pin | Usage | Note |
| -------- | ------------- | ---------- | ----- | ---- |
| KEY0 | 8 | IO17 | camera auto focus | HIGH=push button;  |
| KEY1 | 9 | IO16 | camera bin level (zoom) | HIGH=push button;  |
| IO[11:10] | [11:10] | IO4 + IO14 | wall detection | 00 = no_wall; 10 = `\`; 01 = `/`; 11 = both;  |
| IO[12] | 12 | IO15 | horizontal flag | 1 = horizontal;  |
| IO[7:6] | [7:6] | IO5 + IO18 | beacon detection |  |

![pin mapping](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/FPGA-cap-board-pin-mapping.png?raw=true)

#### Maze solving algorithm

1. Depth-first searching
    - move as far as possible; 
    - if “dead-end’, return to the last node; 
    - stop as few times as possible; 
    - benefit from the “already explored” regions; 
2. Camera detection
    - FPGA keeps sending a 2-bit signal `wall_detection` to ESP32
        - 00 = `no_wall`   
            If there is no wall in front of the rover, continue forward; 
            
        - 10 = `left_wall` 
            There is wall in front of the rover, the rover must stop immediately. 
            The wall is in `/` direction. 
            
        - 01 = `right_wall`
            There is wall in front of the rover, the rover must stop immediately. 
            The wall is in `\` direction. 
            
        - 11 = `horizontal_wall`
            When the rover finds itself approaching to a wall with some degree (rather than perpendicularly), it would then turn around, until the camera detects that the wall in front of it becomes “horizontal” in the lower part of the view. 
            
            If the rover is approaching a wall perpendicularly, this signal should change from 00 directly to 11. 
            
    - FPGA keeps sending a 2-bit signal `beacon_detection` to ESP32
        - need to do experiments with the beacon lighted up
        - Current idea is to use 01, 10, 11 to refer to the three colours. The microcontroller would calculate the angles between each beacon from the revolution of the stepper motors. With the information of the length and width of the whole maze, the microcontroller is then capable to calculate the following values: (using trigonalmetric)
            - its current postion (i, j index)
            - adjust the horizontal and vertical routes
        
        (We have to use this method to locate the rover, because the slight uncertainty in distance and angle mesaurement might be carried forward, and could end up with large errors in the final plot.)
        
3. During any movement, the microcontroller would keep track of the “positions it have been to”, and store the trace in a map. At the points where it have turned around and successfully found new path, a node would be made. 
    There are two possible ways to represent the maze data (when sending request to the server): 
    - Method 1: 
        The microcontroller sends the nodes’ information to the server, this is a graph data structure (like we used in discrete maths course). (It cannot be a matrix as the walls are not plotted on a grid.) 
        The website would have to use the trace to recontruct the maze. 
        The walls’ angles are measured (as described in “wall detection”), these should also be stored as attributes of the nodes. 
    
    - Method 2: 
        Instead of sending the trace to the server, the microcontroller tries to calculate the intersection points of the walls. This time the nodes in the database would be representing the corners in the maze (or when the wall just goes like: `|` )
        Both methods have the same maze exploring algorithm, the latter one would require the microcontroller to do a bit more calculation. Either way is possible, the server could insert a new line into its database for the website to use. 
    
4. Trial and error
    - the closest point (before the rover have to stop in front of a wall)  ↔ threshold for FPGA to check the y value of wall detection
    - measure that distance ↔ for calculation of wall intersection coordinate
    - angle of camera installation ↔ able to detect beacons and walls at the same time
        (the zoom in function on FPGA could be controlled by ESP32, so it is possible to change the view during maze exploration.)

--- 

### May 26th (Fri)

<mark style="background-color: #7099a7;">Tested 3-bit wall detection, wrote and tested digital read function with threshold values for unstable camera detection scenarios. Lit up the beacons and tested colour detection. </mark>

<mark style="background-color: #7099a7;">Discussed and planned the data structure for communication between ESP32 and web server. Tested the plotting code which uses the trace of rover to reconstruct the maze (JS + CSS). </mark>

<mark style="background-color: #7099a7;">Designed the state machine for motor control algorithm and added conditions on wall detection. </mark>

#### Stepper motor control

NEMA-17 is unipolar, hybrid stepping motor with a 1.8° step angle (200 steps/revolution). Each phase draws 1.2 A at 4 V. 

Stepper motors move in discrete steps, and the stepper motor driver generates the appropriate control signals to drive the motor in the desired direction and step size. It receives step commands from the microcontroller and translates them into the required sequence of electrical pulses to move the motor shaft.

Some stepper motor drivers also support microstepping, which allows for smoother and more precise movements by dividing each step into smaller microsteps.

We need the motor driver to be precise, in order to precisely:

1. turn the rover around untill the beacon is detected & located in the center of the camera view
2. control & count the number of steps & revolutions (calculate the distance & angle based on the revolution counts)
3. Powering?

#### Database attribute structure

|  node_id    |  x   |  y  | connect |
|-------------|------|-----|---------|

|  node_id    |   connect_id |
|-------------|--------------|

(Many-to-many relationships got created new relations in relational schema.)

---

This weekend: 

- [ ] Design the state machine so that it fits into the loop function in arduino. 
- [ ] Go through the graph searching algorithm, see if there is anything to improve. 
    - [ ] memory data structure 
    - [ ] least amount of turning around
- [ ] If red and yellow beacons are hard to recognise, position calculation optimisation. 


