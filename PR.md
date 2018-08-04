# PR summary

This PR is in reference to the GSoC'18 peoject 'Gazebo-RoboComp Integration'. Project proposal is [here](https://docs.google.com/document/d/1xzuv4wYToisrXVPon--sVB4EtqkE2oLA8LNIe0bQFIQ/edit#heading=h.xfgh775wbeh8)

* A new tool `gzserver` is created to communicate between RoboComp framework and Gazebo simulator. 
* Consists of all the major interfaces needed in a robotic system like `IMU`, `Laser`, `Joint Motor`, `Camera`, `Depth Camera`.
* Consist of ready to go models and scenarios to test software stack.
* Has two way communication with each interface.
* Nothing extra needed to do after a creating a component with the supported interfaces.
* Has a keyboard controller to control a differential robot model in simulator.
* Tried to keep it independent of any version of `Gazebo` installed.

Feel free to review @marco.
