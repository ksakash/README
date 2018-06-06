# Slice definition for each sensor

For `LaseData.ice`

```cpp
#ifndef LASERDATA_ICE
#define LASERDATA_ICE

module RoboComp{ 

  // laser information 
  class LaserData
  {
    sequence<int> distanceData;
    int numLaser;
    float minAngle;
    float maxAngle;
    float minRange;
    float maxRange;
  };

}; //module

#endif //LASERDATA_ICE
```

Taken `Laser` for example 
```cpp
#ifndef LASER_ICE
#define LASER_ICE

#include <RoboComp/LaserData.ice>

module RoboComp{  

   // Interface to the Gazebo laser sensor
  interface Laser
  {
     idempotent	 LaserData getLaserData();
  };

}; //module

#endif //LASER_ICE
```
