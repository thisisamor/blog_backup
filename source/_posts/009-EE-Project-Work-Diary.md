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

<small style="opacity: 0.7;"> Our second end-of-year project is to build a dynamically balanced rover which is capable of exploring and solving an irregular maze through camera detection and analysis, and present the result on a website with the help of reliable server communication and database. </small>

---

## Week 0

### May 18th (Thu)

<mark style="background-color: #7099a7;"> Year 2 project introduction. </mark>

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

<mark style="background-color: #7099a7;"> First group meeting, discussed about the project requirements, went up with the basic idea of rover algorithm, listed and assigned tasks. </mark>

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

<mark style="background-color: #7099a7;"> Went though basic JavaScript tutorial, set up AWS server environment for node.js, express and dynamoDB. Designed basic polling algorithm. </mark>

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

<mark style="background-color: #7099a7;"> Wrote code for ESP32 microcontroller to connect to WiFi. Tested with mobile phone hot spot. Set up VS code environment for arduino platform (bcs it looks better). </mark>

<mark style="background-color: #7099a7;"> Established and tested D8M camera with FPGA connection. (Can see the image from laptop now.) </mark>

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

<mark style="background-color: #7099a7;"> Wrote and tested HTTP POST request with a simple AWS web server. </mark>

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

<mark style="background-color: #7099a7;"> Met Ed Stott, who instructed more on maze complexity. </mark>

<mark style="background-color: #7099a7;"> Tested camera detection: when approaching to a wall, set the wall flag to HIGH. (Visual detection was done by setting restrictions on the region of pixels whose colour code is considered as white.) </mark>

<mark style="background-color: #7099a7;"> Established connection between FPGA output and ESP32 input, encountered problem when using digitalRead on the arduino side. (The voltage output was as expected according to the value measured with a multimeter, but ESP32 constantly read a HIGH input). </mark>

#### Maze complexity

> The robot can locate itself by measuring the angle between the observed positions of the three beacons, when the positions of the beacons are known in advance. The camera is probably the simplest way of observing the beacons.
> The maze will always start in one corner and finish in the opposite corner. It's approximately 2.4m x 3.6m. The perimeter of the maze has a light strip.
> The walls will be (nominally) straight. They won't be absolutely straight because the corners have a minimum bend radius, but you can map them as straight lines. 
> Some walls will not be parallel with any side of the arena. The smallest gap will be 350mm. 
> There could be multiple paths to solve the maze, aka “floating islands” inside. 

<small style="opacity: 0.7;"> I'm not satisfied with this maze complexity specification thing at all. Our department literally had three weeks during our final exams to come up with ideas of our summer project. But in the end, they gave us this briefing with very few components ready and they had no idea what the maze should be like. They initially wanted to build a test arena with blocks and lighting strips where the walls are all parallel to the sides. It was a few days later when they realised that students were planning to map the whole arena as a matrix and decided this would be too straight-forward. They changed the whole arena to make it irregular - and impossible if not discretely calculate the location of the rover. 

I'm very glad to have some challenging tasks, but it made me a bit frustrated that they didn't prepare everything properly and i had to re-plan our algorithm for so many times. </small>

![test arena photos](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/test-arena-final-photo.jpg?raw=true)

---

### May 25th (Thu)

<mark style="background-color: #7099a7;"> Solved yesterday's problem (by applying FPGA cap board pin mapping), tested ESP32 digital read pin from FPGA (a flag showing whether or not a wall is detected). Tested digital write to control FPGA key (zoom in / zoom out / focus). </mark>

<mark style="background-color: #7099a7;"> Changed the basic wall detection into more specific requirements: detecting whether or not the wall is horizontal in the camera view. </mark>

<mark style="background-color: #7099a7;"> Planned new maze solving algorithm on updated maze complexity rules and current ability of camera detection (based on experiment). </mark>

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

<mark style="background-color: #7099a7;"> Tested 3-bit wall detection, wrote and tested digital read function with threshold values for unstable camera detection scenarios. Lit up the beacons and tested colour detection. </mark>

<mark style="background-color: #7099a7;"> Discussed and planned the data structure for communication between ESP32 and web server. Tested the plotting code which uses the trace of rover to reconstruct the maze (JS + CSS). </mark>

<mark style="background-color: #7099a7;"> Designed the state machine for motor control algorithm and added conditions on wall detection. </mark>

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

- [x] Design the state machine so that it fits into the loop function in arduino. 
- [ ] Go through the graph searching algorithm, see if there is anything to improve. 
    - [x] memory data structure 
    - [ ] least amount of turning around
- [ ] If red and yellow beacons are hard to recognise, position calculation optimisation. 

---

## Week 2

### May 29th (Mon)

<mark style="background-color: #7099a7;"> Modified function to read beacon detection result from FPGA (mostly the same as wall detection), wrote the corresponging functions for position calculation and estimation (the first one uses angle measured between 2 beacons to locate itself inside the maze, the second one estimates its position simply by adding the revolution of motor to the previous position). </mark>

<mark style="background-color: #7099a7;"> Designed and implemented a state machine in ESP32 data memory to keep track of the state. The state would be checked at the beginning of each loop, after which the program would enter the corresponding condition to execute the functions. </mark>

#### Angle calculation



#### State machine

The following code shows how the state machine is implemented inside data memo. More sates may have to be added later (eg: `try` state is when a wall is supposed to be "horizontal" but the rover is approaching from a certain angle because of motor uncertainty, and would not trigger the flag). 

```
typedef enum 
{
    no_wall,
    turn_left,
    turn_right,
    // may have: try
    new_node,
    find_next_path, 
    // optional: fast
    finish
} State;
State currentState = no_wall;
void next_state(int next)
{
    switch (currentState) 
    {
        case no_wall:  // 0
            if (next == 010) 
            {
                currentState = turn_left;
            } 
            else if(next == 100)
            {
                currentState = turn_right; 
            }
            else if(next == 111)
            {
                currentState = new_node; 
            }
            break;
        case turn_left:  // 1
            currentState = new_node;
            break;
        case turn_right:  // 2
            currentState = new_node;
            break;
        case new_node:  // 3
            currentState = find_next_path;
            break;
        case find_next_path:  // 4
            currentState = no_wall;
            break;
        default:
            currentState = no_wall;
    }
}
```

---

### May 30th (Tue)

<mark style="background-color: #7099a7;"> Wrote code for Node data structure and process stack. This should enable the rover to store the explored part of the maze for optimisation. More algorithm have to be implemented though. </mark>

#### Data structure - Node

The following is the hash table that nodes are going to be stored in. I'm not that confirmed that we are using hash table? may have to think about the best way to store & look things up later... TODO TODO TODO...

```
struct Node
{
    int node_id; 
    std::pair<int, int> position; 
    std::vector<std::vector<int>> connected_node; 
    // [0] = id; [1] = left_wall; [2] = right_wall; 
}; 

struct HashEntry 
{
    int key;
    Node value;
    struct HashEntry* next;
};

#define TABLE_SIZE 30
struct HashTable 
{
    struct HashEntry* table[TABLE_SIZE];
};

int hashFunction(int key) 
{
    // Perform some hash calculation based on the key
    return key % TABLE_SIZE;
}

HashTable* node_graph = NULL; 
Node null_node; 
int last_id; 

void insert(struct HashTable* ht, int key, Node value) 
{
    int index = hashFunction(key);
    
    // Create a new entry
    struct HashEntry* newEntry = (struct HashEntry*)malloc(sizeof(struct HashEntry));
    newEntry->key = key;
    newEntry->value = value;
    newEntry->next = NULL;
    
    // Check if the index is empty
    if (ht->table[index] == NULL) 
    {
        ht->table[index] = newEntry;
    } 
    else 
    {
        // Handle collision by chaining
        struct HashEntry* current = ht->table[index];
        while (current->next != NULL) {
            current = current->next;
        }
        current->next = newEntry;
    }
}

void graph_init()
{
    Node node0; 
    node0.node_id = 0; 
    node0.position = std::make_pair(175, -175); 
    insert(node_graph, 0, node0); 
    last_id = 0; 
}

Node get_node(struct HashTable* ht, int key)
{
    int index = hashFunction(key);
    
    // Traverse the linked list at the index
    struct HashEntry* current = ht->table[index];
    while (current != NULL) 
    {
        if (current->key == key) 
        {
            return current->value;
        }
        current = current->next;
    }
    // should not happen
    Serial.println("Wrong: cannot find node!"); 
    return null_node; // (if not found)
}

void add_node( std::pair<int, int> new_node_position, std::vector<std::vector<int>> connect )
{
    Node tmp; 
    last_id ++; 
    tmp.node_id = last_id; 
    tmp.position = new_node_position; 
    tmp.connected_node = connect; 
    insert(node_graph, last_id, tmp); 
    Serial.println("New node added!"); 
}

void update_node( struct HashTable* ht, int key, std::vector<std::vector<int>> new_connect )
{
    int hashValue = hashFunction(key); 

    struct HashEntry* current = ht->table[hashValue];
    while (current != NULL) 
    {
        if (current->key == key) 
        {
            (current->value).connected_node = new_connect; 
            Serial.println("Node updated!"); 
            break; 
        }
        current = current->next;
    }
}
```
#### Data structure - process stack

This is how the rover keeps track of the processes when using the depth-first algorithm. Whenever it meets a dead-end, it goes back and takes the top process on the stack, which would instruct it to check for paths between the last two nodes stored in the process stack. 

```
struct Process
{
    std::pair<int, int> node_id_pair; 
    Process* last_process; 
}; 

Process* stack = new Process; 

void add_process(std::pair<int, int> new_node_id_pair)
{
    Process* new_process = new Process; 
    new_process->node_id_pair = new_node_id_pair; 
    new_process->last_process = stack; 
    stack = new_process; 
    delete new_process; 
}

void finish_process()
{
    if (stack != NULL)
    {
        stack = stack->last_process; 
    }
    else 
    {
        currentState = finish; 
    }
}

std::pair<int, int> get_process()
{
    return stack->node_id_pair; 
}
```

---

### May 31st (Wed)

<mark style="background-color: #7099a7;"> Finished the data structure code (mostly). </mark>

<mark style="background-color: #7099a7;"> We are having an interim presentation tomorrow (20% of this project's mark, 15 mins of presentation, 5 mins Q&A), so i wrote smth for the PPT (also had a rehearsal with teammates). </mark>

#### Interim Presentation

I'm just putting some screenshots from the PPT. I used animation but they wont show. 

![data processing diagram](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/data_processing_diagram.jpg?raw=true)

![basic algorithm](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/basic_algorithm.jpg?raw=true)


![data structure](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/data_structure.jpg?raw=true)

![state machine](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/state_machine.jpg?raw=true)

![optimisation](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/optimisation_methods.jpg?raw=true)

#### FPGA algorithm

This part shows how we do the image processing on the FPGA side. It is how we technically realised our maze exploring. (Also, it explaines WHY we were using such an approach to do the wall detection. )

![wall detection](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/wall_detection.jpg?raw=true)

![wall detection algorithm](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/wall_detection_algorithm.jpg?raw=true)

![beacon detection](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/beacon_detection_algorithm.jpg?raw=true)

The VGA display used here is just a way to highlight the pixels with different flags so that when debugging with image synchronously showing on the computer, we humans know what the FPGA is "looking at". 

![VGA display](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/VGA_display.jpg?raw=true)

#### Server communication 

A very simple diagram showing how the communication system with server would be like. Also, since the front end plots the maze with rover position only, the json payload would only have 2 keys (x and y coordinates). And once the rover finishes exploring the maze (ie: the process stack is empty), it would enter a state called "finished", and send the final node graph to the server. (The server would then use dijkstra algorithm to find the best route and plot it on the website. ) I couldnt convince Jessica to write a fixed window size reliable data transport system though. 

![server system](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/server_communication.jpg?raw=true)

<small style="opacity: 0.7;"> life is so difficult, I cried twice today. </small>

---

### Jun 1st (Thur)

<mark style="background-color: #7099a7;"> The presentation was ok. </mark>

<mark style="background-color: #7099a7;"> Finally the motor driver arrived today. It worked fine. NiMH batteries are still not available. Currently we are using the power supply (idk its name, but it's huge, used for EEE2 experiments i think). </mark>

<mark style="background-color: #7099a7;"> Wrote code for the optimisation algorithm, I hope we can test the whole procedure with the motors soon. </mark>

<mark style="background-color: #7099a7;"> Wrote and tested UART communication between ESP32 and FPGA. </mark>

#### Optimisation 

The following code is used in scenarios as mentioned before in the PPT. 

```
std::vector<int> delete_connect( std::vector<std::vector<int>> &connect, int delete_id )
{
    std::vector<int> tmp; 
    for ( int i=0; i<connect.size(); i++ )
    {
        if ( (connect[i])[0] == delete_id)
        {
            tmp = (connect[i]); 
            for ( int j=i; j<connect.size()-1; j++ )
            {
                connect[j] = connect[j+1]; 
            }
            connect.pop_back(); 
            return tmp; 
        }
    }
    // should not happen
    return tmp; 
    Serial.println("Wrong: delete connection failed!"); 
}

void insert_new_node( struct HashTable* ht, int node1_id, int node2_id, int new_node_id )
{
    std::vector<int> node1_migrate; 
    std::vector<int> node2_migrate; 

    // modify node1 connect
    int hashValue = hashFunction(node1_id); 
    struct HashEntry* current = ht->table[hashValue];
    while (current != NULL) 
    {
        if (current->key == hashValue) 
        {
            node1_migrate = delete_connect((current->value).connected_node, node2_id); 
            break; 
        }
        current = current->next;
    }
    // modify node2 connect
    hashValue = hashFunction(node2_id); 
    current = ht->table[hashValue];
    while (current != NULL) 
    {
        if (current->key == hashValue) 
        {
            node2_migrate = delete_connect((current->value).connected_node, node1_id); 
            break; 
        }
        current = current->next;
    }
    // add node 1 and node 2 connection into new_node
    hashValue = hashFunction(new_node_id); 
    current = ht->table[hashValue];
    while (current != NULL) 
    {
        if (current->key == hashValue) 
        {
            ((current->value).connected_node).push_back(node1_migrate); 
            ((current->value).connected_node).push_back(node2_migrate); 
            break; 
        }
        current = current->next;
    }
    
    Serial.println("Node inserted between " + String(node1_id) + " and " + String(node2_id) + "!"); 
}

void add_node_at_current_position()
{
    std::vector<std::vector<int>> tmp; 
    add_node(current_position, tmp); 
    Serial.println("New node added at current position."); 
}

void check_node_at_new_node()
{
    struct HashEntry* first = node_graph->table[0];
    while (first != NULL)    // TODO: (not necessary?) optimise this algorithm 
    {
        struct HashEntry* second = node_graph->table[0];
        while (second != NULL) 
        {
            // TODO: double check the parameters! 
            if (check_intersection((first->value).position, (second->value).position, (get_node(node_graph, last_id)).position, (get_node(node_graph, last_id-1)).position))
            {
                std::pair<int, int> new_node_position = intersection_calculation((first->value).position, (second->value).position, (get_node(node_graph, last_id)).position, (get_node(node_graph, last_id-1)).position); 
                std::vector<std::vector<int>> tmp; 
                add_node(new_node_position, tmp); 
                insert_new_node(node_graph, (first->value).node_id, (second->value).node_id, last_id); 
                Serial.println("New node added at (" + String(new_node_position.first) + ", " + String(new_node_position.second) + " )."); 
            }
            second = second->next; 
        }
        first = first->next;
    }
}
```

#### UART communication 

This would enable ESP32 to read 32 bits messages from FPGA (rather than digital read from pin which is only 1 bit). The 7 pins currently reserved for FPGA could be reduced to only 2 pins. 

Set up two serials (one for monitor output, one for UART communication): 

```
  pinMode(UART_RXD, INPUT); 
  pinMode(UART_TXD, OUTPUT); 
  Serial.begin(115200);
  Serial2.begin(115200, SERIAL_8N1, UART_RXD, UART_TXD); 
```

Read and print in serial monitor: 
```
String UART_read_FPGA()
{
    if (Serial2.available() > 0) 
    {
        // read the incoming bytes:
        Serial.println(Serial2.read());
        return String(Serial2.read()); 
    }
    else 
    {
        Serial.println("UART not available.");
    }   
}
```

With the help of UART, more complexed image processing might be possible. 

<small style="opacity: 0.7;"> idk, it's still so difficult. </small>

<small style="opacity: 0.7;"> i heard that one group was using UART to send the full image to ESP32, who post it to the server. the server then process it using open cv, and send command back to the rover. but the time delay was quite long. </small>

---

### Jun 2nd (Fri)

<mark style="background-color: #7099a7;"> Tested stepper motor control code. </mark>

<mark style="background-color: #7099a7;"> Discussed what data would be sent bia UART for most efficient image processing. (we might build a very simple instruction set for that purpose) However the actual scenarios would be so complicated, i dont see that realised very soon. </mark>

<mark style="background-color: #7099a7;"> Redesigned chasis for a vertically balanced rover (a higher centre of mass would shorten the distance to be moved by the rover to dynamically balance) and planned the part where provides ajustable angle and height of the camera implementation. </mark>

<mark style="background-color: #7099a7;"> Discussed Nicholas's work on MCU6050 gyroscope and accelerometer, planned on design of controller for balnacing. </mark>

#### Stepper motor control

A step of 2 seconds high and 2 seconds low would result in 1.8 degree of motor move. The current design would allow 200 steps per motor revolution. 

```
void motor_control(int STPL, int STPR, int DIRL, int DIRR, int n) 
{
  digitalWrite(DIRL, LOW);
  digitalWrite(DIRR, HIGH);

  for(int i=0; i<200*n; i++)
  {
    digitalWrite(STPL, HIGH);
    digitalWrite(STPR, HIGH);
    delayMicroseconds(2000);
    digitalWrite(STPL, LOW);
    digitalWrite(STPR, LOW);
    delayMicroseconds(2000);
  }
}
```

#### MCU6050 data processing 

The MCU6050 module has built-in gyroscope and accelerometer, which would give reading in three different axis (x, y, z, left-hand rules tho). 

```
MPU_6050::MPU_6050(float aCompFilterParam, Cartesian aAccAngles, Cartesian aGryoAngles, Cartesian aFilteredAngles, Cartesian aGyroCalValues, Cartesian aGyroAngleVelCal){
  mpu = Adafruit_MPU6050();
  accAngles = aAccAngles;
  gyroAngles = aGryoAngles;
  filteredAngles = aFilteredAngles;
  gyroCalValues = aGyroCalValues;
  gyroAngVelCal = aGyroAngleVelCal;
  compFilterParam = aCompFilterParam;
  previousTime = 0.0;
  currentTime = 0.0;
}

MPU_6050::MPU_6050(const MPU_6050& anMPU){
  *this = anMPU;
}

MPU_6050::~MPU_6050(){}

MPU_6050& MPU_6050::operator=(const MPU_6050& anMPU){
  mpu = Adafruit_MPU6050(anMPU.mpu);    // Calling the Adafruit_MPU6050 copy constructor.
  a = anMPU.a;
  g = anMPU.g;
  temp = anMPU.temp;
  accAngles = anMPU.accAngles;
  gyroAngles = anMPU.gyroAngles;
  filteredAngles = anMPU.filteredAngles;
  gyroCalValues = anMPU.gyroCalValues;
  gyroAngVelCal = anMPU.gyroAngVelCal;
  compFilterParam = anMPU.compFilterParam;
  previousTime = anMPU.previousTime;
  currentTime = anMPU.currentTime;
  return *this;
}


// Activates and configures the MPU 6050. Returns "false" if failed, otherwise "true".
// NOTE: Remember that inside member methods of a class, "this->" is implied when referencing other class members.
bool MPU_6050::activate() {
  // Activating the MPU 6050. 
  if (!mpu.begin()) {
    Serial.println("Failed to find MPU6050 chip.");
    return false;
  }
  Serial.println("MPU6050 Found.");

  // Configuring the accelerometer.
  mpu.setAccelerometerRange(MPU6050_RANGE_4_G);

  // Configuring the gyro.
  mpu.setGyroRange(MPU6050_RANGE_500_DEG);
  mpu.setFilterBandwidth(MPU6050_BAND_44_HZ);

  return true;
}

// Calibrates the Gyroscope. Should be run when the MPU 6050 is in a stationary position.  Returns "false" if failed, otherwise "true".
bool MPU_6050::gyro_calibrate(){
  Serial.println("Calibrating gyro, place on level surface and do not move.");
  gyroCalValues = Cartesian{0, 0, 0};   // Reset the gyro calibration values to 0.

  // Take 3000 readings for each coordinate.
  for (int cal_int = 0; cal_int < 3000; cal_int ++){
    if(cal_int % 200 == 0) Serial.print(".");
    mpu.getEvent(&a, &g, &temp);     // Get new sensor event values without updating data members.
    gyroCalValues.x += g.gyro.x;
    gyroCalValues.y += g.gyro.y;
    gyroCalValues.z += g.gyro.z;
  }

  // Average the values
  gyroCalValues.x /= 3000;
  gyroCalValues.y /= 3000;
  gyroCalValues.z /= 3000;

  // The MPU should start running right after calibration, meaning that we now set our initial "currentTime".
  currentTime = micros();

  return true;
}

// Measures sensor event values and updates relevant data members. Returns "false" if failed, otherwise "true".
bool MPU_6050::readSensor(){
  mpu.getEvent(&a, &g, &temp);     // Get new sensor event values.
  updateData();
  return true;
}

// Updates relevant data members in the class with new values read from the sensor. Returns "false" if failed, otherwise "true".
bool MPU_6050::updateData(){
  // Updating time variables.
  previousTime = currentTime;
  currentTime = micros();    // micros() returns the time in microseconds since the current program started running.

  // Updating calibrated angular velocity values.
  gyroAngVelCal = calcAngVelCal();

  // Updating the accelerometer calculated angle values.
  accAngles = calcAccAngles();

  // Updating the gyroscope calculated angle values.
  gyroAngles = calcGyroAngles();

  // Updating the Complementary Filter calculated angle values.
  filteredAngles = calcFilteredAngles();

  return true;
}

// Calculates the Calibrated Gyroscope Angular Velocity values.
Cartesian MPU_6050::calcAngVelCal(){
  Cartesian adjustedGyro;
  // Define new gyroscope angular velocities in degrees/s with the calibration offset values subtracted.
  adjustedGyro.x = (g.gyro.x - gyroCalValues.x)*RAD_TO_DEG;
  adjustedGyro.y = (g.gyro.y - gyroCalValues.y)*RAD_TO_DEG;
  adjustedGyro.z = (g.gyro.z - gyroCalValues.z)*RAD_TO_DEG;

  return adjustedGyro;
}

// Calculates the orientation of the MPU 6050 using only accelerometer data.
Cartesian MPU_6050::calcAccAngles(){
  Cartesian angles;
  angles.x = getAccRoll(a);    // Roll is the rotation about the x-axis.
  angles.y = getAccPitch(a);   // Pitch is the rotation about the y-axis.
  angles.z = getAccYaw(a);     // Yaw is the rotation about the z-axis.
  return angles;
}

// Calculates the orientation of the MPU 6050 using only gyroscope data.
Cartesian MPU_6050::calcGyroAngles(){
  Cartesian angles = gyroAngles;    // Initialize with the current gyro estimated angles.
  float dt;
  Cartesian adjustedGyroVel = calcAngVelCal();

  // Calculating the change in time elapsed since the last measurement in seconds. This change in time is used for the integration approximation.
  dt = (currentTime - previousTime)/1000000.0;

  // Gyroscope estimated angle values. These are integrals over time and will tend to drift.
  angles.x += adjustedGyroVel.x*dt;     
  angles.y += adjustedGyroVel.y*dt;
  angles.z += adjustedGyroVel.z*dt;

  // Changing angles to be within -180 and 180 degrees.
  angles.x = normalizeAngle(angles.x);
  angles.y = normalizeAngle(angles.y);
  angles.z = normalizeAngle(angles.z);

  return angles;
}

// Calculates the orientation of the MPU 6050 using accelerometer and gyroscope data with a Complementary Filter.
Cartesian MPU_6050::calcFilteredAngles(){
  Cartesian angles;
  float dt;
  Cartesian adjustedGyroVel = calcAngVelCal();
  Cartesian accelAngles = calcAccAngles();

  // Calculating the change in time elapsed since the last measurement in seconds. This change in time is used for the integration approximation.
  dt = (currentTime - previousTime)/1000000.0;

  // Complementary filter. (NOTE: A complementary filter is a computationally inexpensive sensor fusion technique that consists of a low-pass and a high-pass filter).
  // In this case the low-pass and high-pass filter are modeled as scalar multiplications to different terms.
  // This way we can combine the gyro and accelerometer measurements to get more accurate pitch, roll, and yaw values.
  angles.x = (compFilterParam)*(filteredAngles.x + adjustedGyroVel.x*dt) + (1-compFilterParam)*(accelAngles.x);   // The first term in this equation represents the previous filtered angle plus the integral of angular velocity from the gyro. The second term is the estimated accelerometer angle.
  angles.y = (compFilterParam)*(filteredAngles.y + adjustedGyroVel.y*dt) + (1-compFilterParam)*(accelAngles.y);
  angles.z = (compFilterParam)*(filteredAngles.z + adjustedGyroVel.z*dt) + (1-compFilterParam)*(accelAngles.z);

  return angles;
}
```

For usage in main function, the following class is used so every time `mpu6050.readSensor();` is called, it updates all readings for controller to use. 

```
class MPU_6050{
public:
    // Default constructor (Remember that you do not need a default argument for every data member in a class. Additionally, when using the default constructor, variables with default arguments can be omitted as parameters and their default value will be assumed.)
    // It is also important to note that if you want to initialize the object with different values for certain default arguments, you need to enter a value for each default argument preceding the one you want to specify in the constructor input parameters. Hence, put the variables you are most likely to specify at the beginning of you input parameters.
    MPU_6050(float aCompFilterParam = 0.98, Cartesian aAccAngles=Cartesian{0, 0, 0}, Cartesian aGryoAngles=Cartesian{0, 0, 0},
            Cartesian aFilteredAngles=Cartesian{0, 0, 0},  Cartesian aGyroCalValues=Cartesian{0, 0, 0},
            Cartesian aGyroAngleVelCal=Cartesian{0, 0, 0});
    MPU_6050(const MPU_6050& anMPU);        // Copy constructor
    ~MPU_6050();        // Destructor

    MPU_6050& operator=(const MPU_6050& anMPU);     // Assignment operator

    // Methods
    bool activate();
    bool gyro_calibrate();
    bool readSensor();
    bool updateData();
    Cartesian calcAngVelCal();
    Cartesian calcAccAngles();
    Cartesian calcGyroAngles();
    Cartesian calcFilteredAngles();

    // Data Members
    Adafruit_MPU6050 mpu;       // Adafruit MPU 6050 object.
    sensors_event_t a, g, temp;     // Accelerometer, gyroscope, and temperature sensor last measured sensor event values.
    Cartesian accAngles;       // Accelerometer calculated angle values.
    Cartesian gyroAngles;      // Gyroscope calculated angle values.
    Cartesian filteredAngles;  // Calculated angle values after the Complementary Filter.
    Cartesian gyroCalValues;   // Stores the x, y, and z gyro calibration values.
    Cartesian gyroAngVelCal;   // Stores the calibrated angular velocity values in degrees/s.
    float compFilterParam;          // Parameter that defines the LPF and HPF for the angle calculation Complementary Filter.
    unsigned long previousTime;     // Stores the previous time when a sensor event was read. Used for integration calculations.
    unsigned long currentTime;      // Stores the current time when a sensor event was read. Used for itegration calculations.
};
```

#### Chasis and balance

The major concern is that, the camera requires a rather critical angle between itself and ground to read pixels from the correct threshold region (and also the height from the ground matters for the beacons). 

However, the balancing system could only dynamically try to stay in that balnaced state (by constantly, slightly, moving forward and backward) and not collide, obviously. 

These two kinda conflict and we would have to find a trade-off in between. We may even have to add some command to judge whether or not the current position is "a state where the camera detection result is usable". 

---

This weekend: 

- [ ] Read doc on controller modelling
- [ ] try? designing a simple controller model, settling time very important!!!
- [ ] How to put the controller and motor function together? 

- [ ] (optional) is there a way to model all scenarios that might be captured by camera at sometime throughout demo? 
   - [ ] calculate angle from camera image? 
   - [ ] detection of path from camera image? 
