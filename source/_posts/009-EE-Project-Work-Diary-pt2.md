---
title: EE Project Work Diary (pt2)
date: 2023-06-18 10:40:56
categories: Project
tags:
  - college
  - project
description: My day-to-day work diary of my college project. Part 2 (Week 3 - Week 5)
---
<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;"> Part 2 of my work diary. The last post got tooooo long. </small>

---

## Week 3

### May 5th (Mon)

<mark style="background-color: #7099a7;"> Worked on designing a new chassis for the rover. We were aiming to design a physical structure so that not only the camera would have a specific angle and height to ground, but also would ensure the control system to have enough space for balancing (before it hits the ground).  </mark>

#### New chassis design

Top layer: breadboard, FPGA (ribbon cables connecting to the camera)

Bottom layer: battery, motor driver (motors)


![chassis design ver2](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/chasis_design_ver2.jpg?raw=true)

![implemented chassis ver2](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/chassis_ver2.jpg?raw=true)

#### Powering

A 7.2V NiMH battery is directly connected to the PCB with motor driver, a USB - type A cable is then uesd to power the FPGA, together with the ESP32 (on top of it) and the D8M camera. The gyroscope on the breadboard connects to 5V output and GRD on ESP32. Therefore the whole system shares the common ground. 

--- 

### May 6th (Tue)

<mark style="background-color: #7099a7;"> Made some minor changes on physical design. Tested camera vision on new chassis version. </mark>


#### Chassis with supporting leg

More stable during testing. 

![supporting leg and camera angle](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/rover+supporting_legs.jpg?raw=true)

We were considering about an upside down implementation of camera, as it would provide a lower camera angle. However, we eventually went back to the original design as the current angle is already acceptable. 

--- 

### May 7th (Wed)

<mark style="background-color: #7099a7;"> Tested and improved the camera detection method.  </mark>

#### Camera vision optimisation

Considering about the following 2 scenarios (path on the side vs no path on the side), it would be helpful for the rover to be able to detect the paths without actually turning around (which, would be the only solution if the camera is only capable of telling whether or not there is a wall in front of the moving direction). 

![path detection examples](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/check_both_sides.jpg?raw=true)

We firstly approached by solving the problem "is there a path in the middle of a wall?" When we tried to turn the rover by a certain degree (so that the wall to be considered just goes through the camera vision), and program to check this white line pixel by pixel. 

![camera vision: wall checking](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/check_path.jpg?raw=true)

This is doable, however, with a few limitations: 

1. The angle to be turned is not very hard to calculate, as it is not linear with respect to the length of the wall. 
2. Apart from the wall to be considered, the other parts of the maze would also be in the camera view, which made the analysis more complicated. 
3. (Partially because of the last two points,) the precision of this method is rather poor, in order to get a reliable result, the rover had to double check for a few more times for a longer wall, which made the optimisation less effective. 

Another approach was checking the distance between the two wall in front of the vehicle on both sides. 

![wall checking algorithm](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/check_path_algorithm.jpg?raw=true)

As shown in the graph above, by setting a threshold on the upper 1/4 of the camera view, it constantly measures the distance between the left and right side's white pixels. (When the distance increases significantly, it is considered as a new path.)

The drawback of this method is that if the rover is not in the center of the path, it may wrongly detect a path in the front. 

---

### May 8th (Thur)

<mark style="background-color: #7099a7;"> Tested and decided the final detection method to be used (check two sides). </mark>

<mark style="background-color: #7099a7;"> Added corresponding analyse functions for optimisation. Fixed bugs in current code. </mark>

<mark style="background-color: #7099a7;"> Tried to help testing the motor function, ended up with burning another ESP32. </mark>

<small style="opacity: 0.7;"> Im never using my laptop to upload any code onto esp again. :((it still hurts</small>


#### Check two sides

The identical results should be as shown below. 

![camera vision: path detection](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/check_both_sides_camera1.jpg?raw=true)

![camera vision: path detection](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/check_both_sides_camera2.jpg?raw=true)

#### ESP32 algorithm

The starting position has to be detected at the beginning of exploring process. After which, the data memory would have to be initialised accoring to the detection result. This part is shown as the code below. 

**Data_memo.cpp**

| function name | input | output | usage |
| --- | --- | --- | --- |
| void next_state(int next) | conditions |  | update next state |
| int get_state() |  | current_state | check current state when entering loop |
| void init_stepper_count()    void add_stepper_count(int n) |  |  | track the stepper motor  |
| void init_turn_count()         void track_turn_count(int n) |  |  |  |
| std::vector<int> track_beacon_angle(String beacon_colour) | beacon detection string | [1] [2] [3]: whether or not the three beacons have been detected; 1 = yes;  0 = no;  | update motor_Y, motor_B and motor_R (motor steppers count when the beacon is detected) |
| void update_current_angle(double angle) |  |  | update current angle directly |
| void estimate_current_angle(int direction, int steps) |  | -Pi ≤ current angle ≤ Pi | keep track of current angle |
| double get_current_angle() |  | current_angle |  |
| void calculate_current_position() |  |  | use motor_Y, motor_B and motor_R to calculate current position |
| void estimate_current_position(int steps) |  |  | use current_position and currrent_angle to estimate current position |
| std::pair<int, int> get_current_position(int steps) |  | current_position |  |
| void insert(struct HashTable* ht, int key, Node value) | node |  | insert new node into hash table // USE add_node INSTEAD |
| void graph_init() |  |  | set up: insert node (0, 0) |
| Node get_node(struct HashTable* ht, int key) |  |  | look up a node in the graph (traversal) |
| String get_graph() |  | String with node, coordinates, and connection info  | / between attributes;                , between connected nodes (same vector);  |
| void add_node( std::pair<int, int> new_node_position, std::vector<std::vector<int>> connect ) | node coordinate, connection info |  | make a new node and call insert function |
| void update_node( struct HashTable* ht, int key, std::vector<std::vector<int>> new_connect ) | new connection info vector |  | update connection info of a node in the graph |
| std::vector<int> delete_connect( std::vector<std::vector<int>> &connect, int delete_id ) | connection info vector + id to be deleted | new connection info vector | delete an id in a connection info vector // FOR insert new node TO USE |
| void insert_new_node( struct HashTable* ht, int node1_id, int node2_id, int new_node_id) | node 1 id; node 2 id; new node id;  |  | add a new node between 2 exsited node + migrate the connection info to new node + delete old connection between 2 node. // NOTICE: new node must be initialised first |
| void add_process() |  |  | add process to process stack (at current position) |
| void finish_process() |  |  | delete the top process from process stack |
| std::pair<int, int> get_process() |  | node id pair (to be checked next) | return the top process from the process stack |
| void split_process() |  |  | // TODO: split 1 process into 2, when a new node is inserted between the precious 2 nodes |
| void add_node_at_current_position() |  |  |  |
| void check_node_at_new_node() |  |  | if the route passed a new node, add it to the graph |
| void check_node_at_new_path() |  |  | if current position is new node, add it to the graph |
| std::vector<int> find_new_path() |  | motor command vector  | // I want to change this later |
| std::vector<int> dijkstra() |  | node id route | shortest route  |

**Balance_control.cpp**

| function name | input | output | usage |
| --- | --- | --- | --- |
| void motor_control_custom(int STPL, int STPR, int DIRL, int DIRR, std::vector<int> command) | command [0] = DIRL; command [1] = DIRR; command [2] = STPL; command [3] = STPR |  |  |

**Calculation.cpp**

| function name | input | output | usage |
| --- | --- | --- | --- |
| double cot(double angle) |  |  |  |
| double degree_to_radian(double degree) |  |  |  |
| std::pair<double, double> angle_calculation(int motor_R, int motor_B, int motor_Y) | revolution counts at R, B, Y beacons | a = angle between Y and B; b = angle between B and R (radian) | position calculation  |
| double angle_estimation(double current_angle, int direction, int steps) |  |  | angle estimation |
| std::pair<int, int> position_calculation(double angle_a, double angle_b) | a and b in radian | x and y coordinates | position calculation |
| std::pair<int, int> position_estimation(std::pair<int, int> current_position, double current_angle, int steps) | current_position and current angle | estimated new current_position | position estimation |
| bool check_intersection(std::pair<int, int> node1, std::pair<int, int> node2, std::pair<int, int> node3, std::pair<int, int> node4) | x, y coordinates of 4 nodes | true = intersect; false = not intersect | optimisation (node adding) |
| std::pair<int, int> intersection_calculation(std::pair<int, int> node1, std::pair<int, int> node2, std::pair<int, int> node3, std::pair<int, int> node4) | x, y coordinates of 4 nodes | the intersection position of the 4 nodes | optimisation (node adding) |

**FPGA_communication.cpp**

| function name | input | output | usage |
| --- | --- | --- | --- |
| String read_FPGA_wall(int wall_0, int wall_1, int wall_2) |  |  |  |
| String read_FPGA_beacon(int beacon_0, int beacon_1) |  |  |  |
| void write_FPGA(int key_0, int key_1, int camera_command) |  |  |  |
| String UART_read_FPGA() |  |  |  |

**WiFi_connection.cpp**

| function name | input | output | usage |
| --- | --- | --- | --- |
| void wifi_connection() |  |  | wifi connection |
| bool check_wifi() |  |  |  |
| void client_post(String message) | normal: current position; finished state: full node graph;  |  | post to server |
| void client_get() |  |  |  |


---

### May 9th (Fri)

<mark style="background-color: #7099a7;"> Worked on ESP32 programming. </mark>

#### Motor command generator

```
std::vector<int> forward = {1, 0, 1, 1}; 
std::vector<int> left = {1, 0, 0, 1}; 
std::vector<int> right = {1, 0, 1, 0}; 
std::vector<std::vector<int>> motor_command_generator(std::vector<int> node_route)
{
    std::vector<std::vector<int>> motor_command; 
    int i = 1; 
    while ( i < node_route.size() )
    {
        // forward command
        std::vector<int> tmp = forward; 
        tmp.push_back( get_step_num( get_node(node_route[i-1]).position, get_node(node_route[i]).position ) ); 
        motor_command.push_back(tmp); 
        i++; 
        // turn direction
        if (i != node_route.size() )
        {
            int tmp_turn = get_angle_num(get_angle(get_node(node_route[i-1]).position, get_node(node_route[i]).position, 0), get_angle(get_node(node_route[i-1]).position, get_node(node_route[i]).position, 0) ); 
            if ( tmp_turn > 0 )
            {
                tmp = left; 
                tmp.push_back(tmp_turn); 
                motor_command.push_back(tmp); 
            }
            else if( tmp_turn < 0 )
            {
                tmp = right; 
                tmp.push_back(-tmp_turn); 
                motor_command.push_back(tmp); 
            }
        }
    }
    return motor_command; 
}
```

#### Check wall from memo

Following shows an optimisation method which was later abandoned (because of the complexity of code and the lack of precision of the location calculation). 

This method was initially planned to reduce the exploring time by not checking the same wall again. (If already checked on one side, do not look at it on the other side.)

```
// from node1 to node2
// direction: 1 = left; -1 = right; 
// return: true = checked; false = not_checked; 
bool check_wall_from_memo(int node3, int node4, bool direction)
{
    struct HashEntry* first = node_graph->table[0];
    while (first != NULL)    
    {
        int i = 0; 
        while ( i < first->value.connected_node.size() ) 
        {
            std::vector<int> tmp = first->value.connected_node[i]; // TODO: optimise the order of checking 
            if ( tmp[1] == 1 ) // left wall -> bool = true
            {
                if ( check_wall((first->value).position, get_node(tmp[0]).position, get_node(node3).position, get_node(node4).position, true, direction))
                {
                    return true; 
                }
            }
            if ( tmp[2] == 1 ) // right wall -> bool = false
            {
                if ( check_wall((first->value).position, get_node(tmp[0]).position, get_node(node3).position, get_node(node4).position, false, direction))
                {
                    return true; 
                }
            }
            i++; 
        }
        first = first->next;
    }
    return false; 
}
```

---

## Week 4

### Jun 11th (Sun)

<mark style="background-color: #7099a7;"> Changed the code into C++ and used iostream to manually input the camera detection result, and debugged by checking the output motor commands. </mark>

#### C++ code for debugging

Fixed errors in Hash Entry. (The orginal one was not correctly writing values into the table)

Full code on [GitHub](https://github.com/thisisamor/rover-test-code/blob/main/test/algorithm_test_demo.cpp). 


#### Find new path

A solution for finding new path is to assess different functions for main loop to execute with respect to the postion that is currently being checked. 

```
typedef enum 
{
    moving, 
    node_left, 
    node_right, 
    path_left, 
    path_right
} Checking;
Checking current_check; 
void update_checking()
{
    switch (current_check) 
    {
        case node_left:  
            stack->node_left_right_checked.first = true;
            break; 
        case node_right:  
            stack->node_left_right_checked.second = true;
            break; 
        case path_left:  
            stack->path_left_right_checked.first = true;
            break; 
        case path_right: 
            stack->path_left_right_checked.second = true;
            break; 
        default: 
            current_check = moving; 
    }
}
std::vector<std::vector<int>> find_new_path()
{
    std::vector<std::vector<int>> motor_command; 
    while (currentState != finish)
    {
        // TODO: check if at stack_second
        if ( same_position(current_position, get_node(stack->node_id_pair.second).position) == false )
        {
            std::cout << "Moving to stack position: check node " << stack->node_id_pair.second << std::endl; 
            // if not, move to that position
            for (int i=0; i<=last_id; i++)
            {
                if ( same_position(get_node(i).position, current_position) )
                {
                    motor_command = motor_command_generator({i, stack->node_id_pair.second}); 
                }
            }
        }
        // 1. top process: node_left_right_checked
            // not {true, true} -> motor command generate, [return]
            // {true, true} -> continue
        if ( stack->node_left_right_checked.first == false ) // node left not checked
        {
            current_check = node_left; 
            motor_command.push_back(left); // turn left
            (motor_command[0]).push_back(get_angle_num(0, M_PI_2)); 
            // std::cout << get_angle_num(0, M_PI_2) <<std::endl; 
            std::cout << "Node left not checked, motor command: "; 
            printVector(motor_command[0]); 
            return motor_command; 
            // TODO: modify node.left_wall
        }
        else if ( stack->node_left_right_checked.second == false ) // node right not checked
        {
            current_check = node_right; 
            motor_command.push_back(right); // turn right
            (motor_command[0]).push_back(get_angle_num(0, M_PI_2)); 
            std::cout << "Node right not checked, motor command: "; 
            printVector(motor_command[0]); 
            return motor_command; 
            // TODO: modify node.right_wall
        }
        // 2. top process: first -> second (backwards)
            // 2.1 check_wall_from_memo
                // wall not checked yet -> motor command generate, [return]
                // wall exist -> update process stack, continue
        if ( stack->path_left_right_checked.first == false ) // path left not checked
        {
            stack->path_left_right_checked.first = true; 
            if (check_wall_from_memo( stack->node_id_pair.first, stack->node_id_pair.second, true ) == false)
            {
                // generate motor command 
                // TODO: test camera detection path
                    // if accurate: one step
                    // else: many steps (one at a time)
                current_check = path_left; 
                motor_command.push_back(fake); 
                std::cout << "Path left not checked, logic not implemented, here is a fake command: "; 
                printVector(motor_command[0]); 
                return motor_command; 
            }
        }
        if ( stack->path_left_right_checked.second == false ) // path left not checked
        {
            stack->path_left_right_checked.second = true; 
            if (check_wall_from_memo( stack->node_id_pair.first, stack->node_id_pair.second, false ) == false)
            {
                // generate motor command 
                // TODO: test camera detection path
                    // if accurate: one step
                    // else: many steps (one at a time)
                current_check = path_right; 
                motor_command.push_back(fake); 
                std::cout << "Path right not checked, logic not implemented, here is a fake command: "; 
                printVector(motor_command[0]); 
                return motor_command; 
            }
        }
        // 3. if everything have been checked, delete top process
        std::cout << "Top stack all checked, delete stack: " << stack->node_id_pair.first << "->" << stack->node_id_pair.second << std::endl; 
        finish_process(); 
        // NOTE: currently using recursive call until new motor command is found
        // not sure if this is the best way
        return find_new_path(); 
    } 
    // if stack is empty, return {{0, 0, 0, 0, 0}} to stop exploring
    return {{0, 0, 0, 0, 0}}; 
}
// when a new path is found
// check if current_position is a new node
// if so, add the new one
void check_node_at_new_path() 
{
    bool indicator = false; 
    struct HashEntry* current = node_graph->table[0];
    while (current != NULL)    
    {
        if (same_position((current->value).position, current_position) == 0)
        {
            indicator = true; 
        }
        current = current->next;
    }
    if (indicator == false)
    {
        std::cout << "Checking current position: node not added yet" << std::endl; 
        std::pair<int, int> new_node_position = current_position; 
        std::vector<std::vector<int>> tmp; 
        add_node(new_node_position, tmp); 
        // 1. split top process
            // 1.1  new_node -> stack_second  : checking side = true; other side = migrate; 
            // 1.2  stack_first -> new_node   : checking side = false; other side = migrate; 
            // TODO: checking side memo
        Process* new_process = new Process; 
        new_process->node_id_pair = std::make_pair(last_id, stack->node_id_pair.second); 
        new_process->path_left_right_checked = 
        stack->node_id_pair = std::make_pair(stack->node_id_pair.first, last_id); 
        // 2. add stack_first and stack_second to tmp
        // (connection info)
        // std::printf("New node added at (" + String(new_node_position.first) + ", " + String(new_node_position.second) + " ).");
    }
}
```

---

### Jun 12th (Mon)

<mark style="background-color: #7099a7;"> Fixed bug in position calculation and the corresponding algorithm errors. </mark>

#### Position calculation

In the given 2.4 * 3.6 (m) arena, the rover locates itself by measuring the angles between each two beacons ($\alpha$, $\beta$). 

The position is unique, as shown below. Location M is on the intersection of circumcircle of triangle MRB and triangle MBY. 

![position calculation 1](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/angle%20calc%200%20(2).png?raw=true)

Angle $RMB$ is the inscribed angle of circle $O_2$, therefore the central angle $RO_2B$ is twice the size of $\alpha$. 

Using trigonometric, the coordinate of $O_2$ can be representated as $(\frac{length}{2}, \frac{length}{2}cot(\alpha))$. 

Similarly, $O_1 (length-\frac{width}{2}cot(\beta), -\frac{width}{2})$.

![position calculation 2](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/angle%20calc%201%20(2).png?raw=true)

Let the coordinate of M be $(x, y)$. 

Both $MO_2B$ and $MO_1B$ are isosceles triangles, therefore $O_1O_2$ is the perpendicular bisector of segment $MB$. The coordinate of N can be represented as $(\frac{length}{2}+\frac{x}{2}, -\frac{width}{2}+\frac{y}{2})$. 

![position calculation 3](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/angle%20calc%20(2).png?raw=true)

The three angles in the figure above have same size $\theta$. Using trigonometric, $tan(\theta)$ could be represented as: 

$$ tan(\theta) = \frac {width+y} {length-x}$$
$$ tan(\theta) = \frac {\frac{length}{2}+\frac{x}{2}-\frac{length}{2}} {-\frac{width}{2}+\frac{y}{2}-\frac{length}{2}cot(\alpha)} $$
$$ tan(\theta) = \frac {length-\frac{width}{2}cot(\beta)-\frac{length}{2}+\frac{x}{2}} {-\frac{width}{2}+\frac{width}{2}-\frac{y}{2}}  $$

Solve the above equations for x and y, the coordinate of M is: 

$$x = length - \frac{length*width}{2} * \frac { ((\frac{length}{2}*cot(α))-(\frac{width}{2})) * (cot⁡(α)*cot⁡(β)-1) } {(\frac{length}{2}*cot⁡(α)- \frac{width}{2})^2 + (\frac{length}{2}-\frac{width}{2}*cot⁡(β) )^2}$$

$$y= - width+  \frac{length*width}{2} * (1- \frac {((\frac{width}{2}*cot(β))-(\frac{length}{2})) * (cot⁡(α)*cot⁡(β)-1)} {(\frac{length}{2}*cot⁡(α)-\frac{width}{2})^2  +(\frac{length}{2}-\frac{width}{2}*cot⁡(β))^2})$$

---

### Jun 13th (Tue)

<mark style="background-color: #7099a7;"> Tested data parsing and recontructing on server side (when receiving requests from esp). (Used plain text instead of jsonpayload.) </mark>

#### Angle calculation 

Currently using input as gyroscope reading for testing the algorithm (with position and node data processing). The following code is a simple calculator which uses python to provide the simulated gyro reading. 

```
import math

def range(angle):
    if angle < 0:
        angle += 360
    return angle

while True:
    x = float(input("current position x: "))
    y = float(input("current position y: "))
    angle = float(input("current angle: "))

    Y = math.atan(-y / (360.0 - x)) * 180 / math.pi
    B = -math.atan((240.0 + y) / (360.0 - x)) * 180 / math.pi
    R = -90.0 - math.atan(x / (240.0 + y)) * 180 / math.pi

    if angle >= Y:
        print("angle_Y =", angle - Y)
        print("angle_B =", angle - B)
        print("angle_R =", angle - R)
    elif angle >= B:
        print("angle_Y =", 360 + angle - Y)
        print("angle_B =", angle - B)
        print("angle_R =", angle - R)
    elif angle >= R:
        print("angle_Y =", 360 + angle - Y)
        print("angle_B =", 360 + angle - B)
        print("angle_R =", angle - R)
    else:
        print("angle_Y =", 360 + angle - Y)
        print("angle_B =", 360 + angle - B)
        print("angle_R =", 360 + angle - R)
```
---
### Jun 14th (Wed)

<mark style="background-color: #7099a7;"> Finished and tested trace back algorithm with path checking. Full code see [GitHub](https://github.com/thisisamor/rover-test-code/blob/main/test/algorithm_test_demo.cpp). </mark>

<mark style="background-color: #7099a7;"> Wrote something in the report (having a consulting session on Thursday). </mark>

---

### Jun 15th (Thur)

<mark style="background-color: #7099a7;"> Added motor function and gyro read into main loop, tested full program (without pid controller). MPU-6050 failed to work with FPGA capboard. </mark>

The output from cap board (supposed to be 5V) failed to supply the ideal work voltage for MPU, ended up with the gyro unable to activate. The battery is not an ideal power supply either, cuz that would be too big a voltage for the chip. 

The FPGA currently had to be powered with external cable, as the flashed programme is not permenantly stored. This might be the reason why the power from cap board is not enough for activating the chip. 

---

### Jun 16th (Fri)

<mark style="background-color: #7099a7;"> Fixed issues in UART connection. </mark>

<mark style="background-color: #7099a7;"> Had problem while testing the motor commands with pwm. Blew up a third esp32. TTTT</mark>

<mark style="background-color: #7099a7;"> The beacons were tested working with the SMPS power control system. </mark>

#### UART 

Must not use `pinMode` to define the UART ports in setup function. (Error: UART not available.)

```
\\ DO NOT: 
  pinMode(UART_RXD, INPUT); 
  pinMode(UART_TXD, OUTPUT); 

\\ error: 
  Serial2.available() == 0; 
```

```
\\ This is enough
  Serial.begin(115200);
  Serial2.begin(115200, SERIAL_8N1, UART_RXD, UART_TXD); 

\\ receive data: 
uint32_t getMsg(){
  byte b0, b1, b2, b3;
  uint32_t msg;
  if (Serial2.available() > 0) {
    b0 = Serial2.read();
    b1 = Serial2.read();
    b2 = Serial2.read();
    b3 = Serial2.read();
    msg = (b3<<24)|(b2<<16)|(b1<<8)|(b0);
    return msg;
  }
  else {
    //Serial.println("UART not available.");
    return 0;
  }
}
std::vector<String> read_FPGA(){
  String wall;   // 3-bit: {wall_flag, hori_flag}
  String beacon;
  String near;
  String enter_two;
  uint32_t msg;
  std::vector<String> read_out;

  do {
    msg = getMsg();
  } while ((msg == 0x00524242) || (msg==0));
      
    wall = String(bitRead(msg,31)) + String(bitRead(msg,30)) + String(bitRead(msg,29));
    beacon = String(bitRead(msg,28)) + String(bitRead(msg,27));
    near = String(bitRead(msg,26));
    enter_two = String(bitRead(msg,25)) + String(bitRead(msg,24));

    read_out.push_back(wall);
    read_out.push_back(beacon);
    read_out.push_back(near);
    read_out.push_back(enter_two);
    return read_out;
}
```

The other problem is that the UART does not provide synchronization of reading messages (ie. results are out of order due to the delays while executing other functions in main loop). 

#### PWM Motor

The current design (wtih 2 supporting legs) is giving the rover too much drag so it doesn't move at all. 

(If you lift the rover up you can see that the wheels are actually rotating, but not when you put it in the arena.)

We increased the torque by changing the pwm frequency, then the esp32 stoppped working. This time it is not powered by two power supplies (im pretty sure) but somehow the esptool setup was burnt. 

#### Power system - beacon

![beacon with SMPS](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/beacon+SMPS.jpg?raw=true)

---

### Jun 17th (Sat)

<mark style="background-color: #7099a7;"> Stepper motors were working (finally!!!) </mark>

<mark style="background-color: #7099a7;"> Tested simple exploring, seems to be working. </mark>

#### Stepper Motor

Solved the following problems: 

1. MUST include the AccelStepper library correctly in platformIO. (Check the version information in platformio.ini config file)

2. DO NOT use `pinMode` to initialise the motor pins in setup function, only use the lib functions to define Lstepper and Rstepper is enough. 

3. `Lstepper.move(numSteps);` sets the stepper motor with a certain value to move, this is triggered by `Lstepper.run();`. Check the remainning steps by calling `Lstepper.distanceToGo();`. 

```
#include <AccelStepper.h>
#define dirPin 26
#define stepPin 25
#define stepsPerRevolution 200
#define dirPin2 33 //only use IO pins not RxTx and digital 
#define stepPin2 32
//// Define a stepper and the pins it will use
AccelStepper Lstepper (AccelStepper::DRIVER, stepPin, dirPin);
AccelStepper Rstepper (AccelStepper::DRIVER, stepPin2, dirPin2); // Defaults to AccelStepper::FULL4WIRE (4 pins) on 2, 3, 4, 5
int numSteps;
float stepAngle = 1.8;  // 1.8degrees per step for NEMA17
float gearRatio = 1.0; //Should be 1, but vary for testing

void motor_init()
{
  Lstepper.setMaxSpeed(200);
  Rstepper.setMaxSpeed(200);
  Lstepper.setAcceleration(100);
  Rstepper.setAcceleration(100);
}

int numstepscalcyaw (double yaw){
  numSteps = yaw*2.5/stepAngle/gearRatio;//assume yaw is degree for now
  return numSteps;  // Move stepper motor 2 by the specified number of steps
}

int numstepscalcdist(double dist){
  numSteps = dist/1000*200/0.2136;//divide by 1000 to be in meters,divide by circumference of wheel and multiply by number of steps
  return numSteps;  // Move stepper motor 2 by the specified number of steps
}

void motor (int dir, double value){
  if (dir == -1){//turn right
    numSteps = numstepscalcyaw(value);
    Lstepper.move(numSteps);
    Rstepper.move(numSteps);
  }
  else if (dir == 0){//forward
    numSteps = numstepscalcdist(value);
    Lstepper.move(numSteps);
    Rstepper.move(-1*numSteps);
  }
  else if (dir == 1){//turn left
    numSteps = numstepscalcyaw(value);
    Lstepper.move(-1*numSteps);
    Rstepper.move(-1*numSteps);
  }
  while (Lstepper.distanceToGo() != 0 || Rstepper.distanceToGo() != 0) {
    // mpu6050.readSensor();
    Lstepper.run();
    Rstepper.run();
  }
}
```

(The final rover should look a bit tidier.)

![rover with stepper motor](https://github.com/thisisamor/blog_pic/blob/main/year2/EE-Project/rover+motor.jpg?raw=true)

---






