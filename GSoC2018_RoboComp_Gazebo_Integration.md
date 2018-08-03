# RoboComp Gazebo Integration

**Student:** Akash Kumar Singh \
**Mentors:** Marco A. Gutiérrez, Ramon Cintas \
**Link to Commits:** https://github.com/ksakash/gazebo-robocomp/commits/master \
**Link to Codes:** https://github.com/ksakash/gazebo-robocomp

## Introduction

Currently RoboComp uses **RoboComp Innermodel Simulator (RCIS)**, an inbuilt simulator, to check its applications and algorithms. It provides a lot of basic tools and features to easily test an application developed by a developer. But besides its usefulness, more tools and features need to be developed to make it better. So, **Gazebo** was chosen to serve as an secondary options to the developers to test their algorithms and fulfill their needs to the fullest.

So, the project aims to provide RoboComp with a simulator that has better options and features for the developers. So, here we are with a working model of the integration that uses ICE framework as a communication layer combined with the Gazebo’s transport layer to have a seamless communication between gazebo simulator and any robocomp simulator. For the ones who don’t what is Ice, Internet Communications Engine (Ice) is an object-oriented RPC framework that helps you build distributed server-client base applications with minimal effort.

## Work Proposed

The aim of the project was to build an integration which has the following capabilties:

* **Two way communication between the interface and the gazebo plugin** \
There are mainly two components in a robotic system: sensors and actuators. So sensor is mainly for getting data from the environment. It can't take any commands but configuration settings. But sometimes we may not need data in a continuous stream but at demand at any moment. So, for that we need to have a two way communication between sensor and the controller, so that controller can send request to sensor when to send data. Likewise in case of actuators we can send instructions to actuator but can also know the state of the actuator at any moment. 

* **No lag in communication between the simulator and the framework** \
One of the major shortcomings of the RCIS was that there was too much communication lag between simulator and the framework. So, it was a major goal of this integration to have a no lag communication between the framework and the simulator. 

* **Better physical models to test the algorithm** \
Way to accurate simulation is the models which we simulate in a virtual environments. The better rendering geometry it has the better it will look. Better physical models also need to have collision geometry and inertial geometry.  

* **Easy access to all the aspects of the simulation** \
In order to use the simulator to its maximum capability. We need to have access to its physics engine, rendering engine, all kinds of sensor it provides and other auxiliary options it provides.

* **Flexibility due to code generation with the robocomp’s DSL** \
Generation of generic code, needed to build a robotic architecture, is one of the prime features of RoboComp framework. So, it was also very necessary to provide the same kind of flexibility with the integration. Aim was that developers don't have to modify the code generated and can start using it without any change. 

## Work Done

* To have a two way communication between Gazebo and RoboComp each Gazebo plugin consist of a `publisher` and a `subscriber`. `publisher` advertises all the data from sensors and `subscriber` hears for any commands from the client.

* The integration uses gazebo transport layer to communicate between simulator and the framework and testing it there is no lag at all in communication.

* There are gazebo plugins for all the important sensors like camera, depth camera, laser, IMU and contact sensor. And, for joint motor and differential robot.

* The integration also contains keyboard controller to control a differential robot in the simulator.

* There are custom messages for different sensor messages to communicate between the gazebo plugin and the corresponding ice interface.

* It contains model of common robots that are generally common in robotics. It also contains different scenarios ported from RoboComp.

* A distributed build integration system.

* The integration is merged to the main code base, building and installing.

## Work Left
Currently the installation has 3 main dependencies: OpenCV, zeroc-ice, Gazebo-7. To install RoboComp with the integration one will need to install these dependencies manually. Also there was no need of a feature for code generation using `cog` module of python.

## Possible Improvements and Unexecuted Ideas
* The integration has all the necessary plugins for sensors. But there can be additions like Kinetic plugin, octabot plugin or a plugin to publish states of all the joints in a robotic model.

* The integration can have more scenarios to test a robot.

* Plugins are the easiest way to excess Gazebo, but it isn't the only way. One can try to connect with gazebo without using plugins. This will reduce a layer of communication which can help in reducing time lag in communication and can give more flexibility to the framework. 

## Implementation Details

For implementation details one can look [here](https://robocomp.github.io/web/gsoc/2018/index), for each and every aspect of the integartion.
