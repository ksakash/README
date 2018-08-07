# PR summary

This PR is in reference to the GSoC'18 project **Gazebo-RoboComp Integration**. Project proposal is [here](https://docs.google.com/document/d/1xzuv4wYToisrXVPon--sVB4EtqkE2oLA8LNIe0bQFIQ/edit#heading=h.xfgh775wbeh8)

* A new tool `gzserver` is created to communicate between RoboComp framework and Gazebo simulator. 
* Consists of all the major interfaces needed in a robotic system like `IMU`, `Laser`, `Joint Motor`, `Camera`, `Depth Camera`.
* Consist of ready to go models and scenarios to test software stack.
* Has two way communication with each interface.
* Nothing extra needed to do after a creating a component with the supported interfaces.
* Has a keyboard controller to control a differential robot model in simulator.
* Tried to keep it independent of any version of `Gazebo` installed.

There is basically two ways of using the installation: [integrated into the main code base](https://github.com/ksakash/robocomp/blob/gz-dev/tools/gzserver/README.md) and in a [separate repository](https://github.com/ksakash/gazebo-robocomp/blob/master/README.md).

The whole report for the code submission is [here](https://gist.github.com/ksakash/ea6c21487df14409a860787ff7a7f66d)

Feel free to review @marco.
