---
layout: post
title: ROS notes for my future self
---


# Intro

## What is ROS?
Robot Operating System is a free open-source common software framework for interfacing between sensors and actuators. It is _not_ an Operating System by itself as it goes on top of Linux/Ubuntu(or other distros of Linux), recent(as of Dec 2021) experimental support has also been extended to Windows, but offers many functionalities that an operating system should - multithreading,low level device control,package management, hardware abstraction. It is maintained by Open Robotics. 

There is a recent release of ROS2 which has better functionality than ROS but for learning purposes it is better to stick with learning ROS because the community support for it is high.

Documentation for ROS is provided in [http://wiki.ros.org/](http://wiki.ros.org/). This is the vast ocean of information,tutorials and book resources regarding everything ROS and it's easy to get lost and sunk. That's why this quickguide exists and you should read on.

## Why use ROS?
- helps to decouple software robot programming from the hardware prototyping
- Common framework for different types of robots that need same functionality. Eg: collision avoidance function in a car can be adapted to a drone as well as a 2 wheeled vehicle, the same code can be re-used. Don't have to reinvent the wheel everytime.
- Can run with high level programming languages. Python and C++ are fully supported. Python is handy for prototyping and C++ for optimization on the robot. There are other languages in the experimental stage that can be supported.
- Can include many third party ML libraries. Tensorflow and Pytorch libraries like openCV, PCL can be incorporated. This makes it very powerful to cater functionalities to the robot
- Contains off-the-shelf algorithms for basic robot requirements like SLAM, path planning, etc. to be experimented on. These can be improved upon with self algorithms.
- Can easily be combined with robot simulators like RViz and Gazebo. This reduces the cost of physical prototyping by simulating the links,sensors and actuators and their physics
  
## Install ROS

ROS,like Linux, comes in various distributions - Kinetic, Noetic, Galactic, etc. For ROS(1) the final release is Noetic Ninjemys. Further developments have been happening in ROS2. Recommended to use Linux based systems. Although Windows is recently supported, life is harder with ROS+Windows when trying to figure out ROS itself is hard. Ubuntu is very well supported. This is the site to install Noetic from : [http://wiki.ros.org/noetic/Installation](http://wiki.ros.org/noetic/Installation)

### Onto your PC

- Download the latest recommended version of ROS distibution (Noetic).
- `sudo apt-get install ros-noetic-desktop full` - to install noetic 
- `rosdep init` \
  `rosdep update` - to install dependencies
- `echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc` - sets the ROS environment in every new terminal
- `source ~/.bashrc` - to add environment to the current terminal
- `sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential` - this commands lets rosinstall install packanges that need to be installed on our robot lateron
- `rosversion -d` - check step to see whether the install has been successful. shows Noetic is installed

### Onto Raspberry Pi

- [https://varhowto.com/install-ros-noetic-raspberry-pi-4/](https://varhowto.com/install-ros-noetic-raspberry-pi-4/)

### Onto Arduino
- Import ROS packages to interphase the linux xomputer with Arduino. `#include <ros.h>` -is the command
- 

## Getting started with terminologies of the ROS paradigm
### Workspace
ROS utilises catkin workspace. Why do we need catkin worksapace? Catkin is the build system of ROS. catkin is a custom build system made from CMake build system and python scripting tp provide more functionality than CMake normal build workflow. Why do we meed to use Catkin instead of CMake? Building a set of ROS packages is complicated and that is more difficult with more number and larger packages. catkin makes it easier./ More info can be found here about why- [http://wiki.ros.org/catkin/conceptual_overview](http://wiki.ros.org/catkin/conceptual_overview)\
**What is a build system?**\
A build system is responsible for generating 'targets' from raw source code that can be used by an end user. eg - libraries, executable programs, interphases, etc. To build targets, the build system needs information such as the locations of tool chain components (e.g. C++ compiler), source code locations, code dependencies, external dependencies, where those dependencies are located, which targets should be built, where targets should be built, and where they should be installed. This is typically expressed in some set of configuration files read by the build system. In an IDE, this information is typically stored as part of the workspace/project meta-information (e.g. Visual C++ project file). With CMake, it is specified in a file typically called 'CMakeLists.txt' 
  
We download all packages under the `catkin_ws` directory. Subdirectories in catkin_ws :
1. Build
2. devel
3. src - has all the packages along with `CMakeLists.txt` that contains a set of directives and instructions describing the project's source files and 'targets'.
   
### Package 
- ROS package consists of all the dependencies, source code, build files for executing the communication between actuators and sensors.
- we created hello_world package\
Typical ROS package structure consists of:
1. config
2. include - package header files
3. launch - store launch files here. Launch files can launch multiple messages, topics and nodes at the same time
4. msgs
5. scripts - store python code here
6. src - store c++ code code here
7. package.xml - contains primary info about the name of the package, description, author,dependencies,etc.
8.  CMakeLists.txt - Has all the commands to build the ROS
### Node
a sensor or actuator that can pass information
### Master
top level node that is the big boss of other nodes. All information transaction is accessible by it.
### Parameter
info accessible by all nodes. Usually associated with Master
### Topic
Analagous to a bus that carries passengers from point A to point B. In this case, a channel to carry certain types of information from node A to node B. A node can publish and subscribe any number of topics.
### Message
The information itself that is carried from A to B. A message is always _published_ through a topic.
### Service
function that can call node B whenever node A calls. Request/Reply mechanism.


### turtlesim for getting a hand of these concepts and terminologies

## Some common ROS commands
- roscore -start ros
- `rostopic list` |`rosnode list`
- `rostopic pub topic-name msg-type data` - to publish a message through a topic on commandline
- `rosrun <rospkg> <node>` - to run a code from a package
- `roslaunch <rospkg> <code.launch>` - to run multiple linked nodes and functionality.
- `rosservice list` - services ongoing
- 

## Creating building blocks of ROS

### Workspace
- `mkdir -p ~/catkin_ws/src`
- `cd catkin_ws/src`
- `catkin_init_workspace` - initialise new ROS workspace. This creates a CMakeLists.txt
- `catkin_make`- build the catkin aka ROS workspace
We need to add the workspace environment so that they are visible and executable by bash.
- `code .bashrc`
- Add `~/catkin_ws/devel/setup.bash` to the end of .bashrc file. We do this because .bashrc file executes when new terminal starts.After execution, when we use any termnial we can access the packages inside catkin_ws.
### Package
In catkin_ws/src, execute `catkin_create_pkg <pkg-name> roscpp rospy std_msgs`
To compile the created nodes, add the below lines of code to CMakeLists.txt
```cmake
include_directories(
    ${catkin_INCLUDE_DIRS}
)

add_executable(talker src/talker.cpp)
    target_link_libraries(talker
    {catkin_LIBRARIES}
    )

add_executable(listener src/listener.cpp)
    target_link_libraries(listener
    {catkin_LIBRARIES}
    )
```
### Node, topic, message
This is usually a .py file kept inside scripts/.
Essential components
1. `#!/usr/bin/env python` this ensures that script is executed as python script
2. `rospy.init_node('name-of-node',anonymous=True)` - anonymous = True ensures that your node has an unique name by adding rando numbers at the end of the name
3. `pub = rospy.Publisher('topic-name',type-of-message,queue_size = 10)` 
   To find the type of message that can be sent via a topic - `rostopic type topic-name`. To list all the topics available - `rostopic list`
   Queue size limits the number of queued messages if the subscriber is not receiving them fast enough.
4. rospy.Subscriber('topic-name',type-of-message,action)
   action is the function performed by the subscriber upon receiving the message from the publisher.
5. Loop to keep the node active:
   - `rospy.spin()` - keeps python from exiting until the node is stopped
   - or `while not rospy.is_shutdown():`

Execute by first changing the permission of the executable by using `sudo chmod +x code.py/launch`in the terminal. The file name changes to green from white if successful when entering `ls`.

### Service
- `rosservice list` - all services that are possible
- serv = rospy.ServiceProxy('/service-name',type-of-message). Type of message that can be passed by called by the service can be found using `rosservice type /<service-name>`

## Simulations 
### Rviz 
ROS vizualization comes along with the ROS installation, default tool to visualise  robot models,etc.
`rosrun rviz rviz`
### Gazebo
```cmd
cd ~/catkin_ws/src/
git clone -b noetic-devel https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
cd ~/catkin_ws && catkin_make
export TURTLEBOT3_MODEL=burger
roslaunch turtlebot3_gazebo turtlebot3_empty_world.launch
```
or we can launch other world in Gazebo using `roslaunch turtlebot3_gazebo turtlebot3_world.launch` or  `roslaunch turtlebot3_gazebo turtlebot3_house.launch`
# Intro to Gazebo


## Rough notes
- rostopic list (all kinds of channels that lets 2 nodes communicate)
- rosnode list (all nodes that exist)
- rosrun turtlesim turtlesim_node
- roslaunch hello_world .launch
- Initialisation in python `chmod +x "filename.py"`

To install library that interphases ROS and arduino

Download arduino ide
Find the path for sketchbook from preferences tab in the ide
execute `rosserial_arduino make_libraries.py <sketchbook-path>/libraries/`



