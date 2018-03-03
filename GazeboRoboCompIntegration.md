# Sample Code for Laser plugin in Gazebo for GazeboRoboComp integration

A plugin is a chunk of code that is compiled as a shared library and inserted into the simulation.
The plugin has direct access to all the functionality of Gazebo through the standard C++ classes.

Plugins are useful because they:

* let developers control almost any aspect of Gazebo
* are self-contained routines that are easily shared
* can be inserted and removed from a running system

So here is the code that i have written taking hints from the GazeboROS integration implementation and GazeboJdeRobot 
integration implementation, so it includes the features of both the implementations, keeping in view the interface of 
RoboComp.

```cpp
// Gazebo includes
#include "gazebo.hh"
#include "physics/physics.hh"
#include "physics/MultiRayShape.hh"
#include "common/common.hh"
#include "transport/transport.hh"
#include "plugins/RayPlugin.hh"
```
Necessary headers to include basic functionalities of Gazebo.
```cpp
#include <boost/bind.hpp>
#include <boost/algorithm/string.hpp>
#include <iostream>
```
General headers.
```cpp
// ICE utils includes
#include <Ice/Ice.h>
#include <IceUtil/IceUtil.h>
#include <easyiceconfig/EasyIce.h>
```
ICE headers for communication interface.
```cpp
// Qt includes
#include <QMutex>
#include <QObject>

// RoboComp includes
#include <Laser.h>

```
`Laser.h` for client side interface.

```cpp
class LaserData
{
public:
    std::vector<float> values;
    double minAngle = 0;
    double maxAngle = 3.1416;
    double minRange = 0;
    double maxRange = 10;
    double resolution;
    uint32_t id;
    int parentId;
};
```
Class for storing all the essential data coming from Gazebo topic.
```cpp
void *mainLaser(void* v);
```
To  instantiate the communicator.
```cpp
namespace gazebo
{
    class Laser : public RayPlugin
  	{

		public: Laser() : RayPlugin()
		{
			std::cout << "Laser Constructor" <<std::endl;
			pthread_mutex_init (&mutex, NULL);
		}

    public: ~Laser()
    {
      if (ic) {
          try {
              ic->destroy();
          } catch (const Ice::Exception& e) {
              std::cerr << e << std::endl;
          }
      }
    }

```
Communicator is destroyed in the destructor.
```cpp
public: void Load(sensors::SensorPtr _parent, sdf::ElementPtr _sdf)
{
  // Don't forget to load the camera plugin
  RayPlugin::Load(_parent,_sdf);
  this->parentSensor =  std::dynamic_pointer_cast<sensors::RaySensor>(_parent);

  if (!this->parentSensor)
    cerr << "GazeboRoboCompLaser controller requires a Ray Sensor as its parent" << endl;

  // Get the world name.
  std::string worldName = _parent->WorldName();
  this->world_ = physics::get_world(worldName);

  // save pointers
  this->sdf = _sdf;

  nameLaser = std::string("--Ice.Config=laser.cfg");

  pthread_t thr_gui;
  pthread_create(&thr_gui, NULL, &mainLaser, (void*)this);
}
```
`Load()` which is executed when the plugin is loaded and takes in the sdf element. By creating the thread here we can 
initialize the communication before the laser start scanning and that will remove the lagthat can happen when establishing 
the connection after the first laser scan also it removes the need of count.
```cpp
// Update the controller
public: void OnNewLaserScans()
{

  physics::MultiRayShapePtr laser = this->parentSensor->LaserShape();

  LaserData data;
  std::vector<float> laserValues(laser->GetSampleCount());

  for (int i = 0; i < laser->GetSampleCount(); i++){
    laserValues[i] = laser->GetRange(i);
  }

  data.values = laserValues;
  data.minAngle = float (laser->MinAngle());
  data.maxAngle = float (laser->MaxAngle());
  data.minRange = laser->GetMinRange();
  data.maxRange = laser->GetMaxRange();
  data.id = laser->GetId();
  data.parentId = laser->GetParentId();

  pthread_mutex_lock (&mutex);
  laserData = data;
  pthread_mutex_unlock (&mutex);
}
```
`OnNewLaserScans()` called after every new scan, to save the laser data.

```cpp
private:
std::string nameLaser;
LaserData laserData;
pthread_mutex_t mutex;

std::string worldName;

physics::WorldPtr worldPtr;
sensors::RaySensorPtr parentSensor;
sdf::ElementPtr sdf;

gazebo::transport::NodePtr gazeboNodePtr;
gazebo::transport::SubscriberPtr laserScanSubPtr;

Ice::CommunicatorPtr ic;
Ice::PropertiesPtr prop;
std::string Endpoints;
Ice::ObjectPtr object;
Ice::ObjectAdapterPtr adapter
```

Private variables to store all the necessary information related to the object.

```cpp
class LaserI: virtual public RoboCompLaser::Laser {
	public:
		LaserI (gazebo::Laser* laser)
		{
			this->laser = laser;
		}

		virtual ~LaserI(){};

		virtual TLaserData getLaserData(const Ice::Current&) {
		    RoboComp::LaserDataPtr laserData (new RoboComp::LaserData());
			  pthread_mutex_lock (&laser->mutex);

  			//Update laser values
  			for(int i = 0 ; i < laserData->numLaser; i++){
  			   laserData->distanceData[i] = laser->laserData.values[i]*1000;
  			}
  			pthread_mutex_unlock (&laser->mutex);
  			return laserData;
		};

	private:
		gazebo::Laser* laser;
};

```
For client side communication. Since the `parentSensor` doesn't have any member function to get the angles, there is some
error here.
```cpp
void *mainLaser(void* v)
{

  	gazebo::Laser* laser = (gazebo::Laser*)v;

  	char* name = (char*)laser->nameLaser.c_str();

    int argc = 1;

    char* argv[] = {name};
    try {
        this->gazeboNodePtr = gazebo::transport::NodePtr(new gazebo::transport::Node());
        this->gazeboNodePtr->Init(this->worldName);

        laser->ic = EasyIce::initialize(argc, argv);
        laser->prop = laser->ic->getProperties();

        laser->Endpoints = laser->prop->getProperty("Laser.Endpoints");
        std::cout << "Laser Endpoints > " << laser->Endpoints << std::endl;

        laser->adapter = laser->ic->createObjectAdapterWithEndpoints("Laser", Endpoints);
        laser->object = new LaserI(laser);
        laser->adapter->add(laser->object, laser->ic->stringToIdentity("Laser"));
        laser->adapter->activate();
        laser->ic->waitForShutdown();

    } catch (const Ice::Exception& e) {
        std::cerr << e << std::endl;
    } catch (const char* msg) {
        std::cerr << msg << std::endl;
    }

    return NULL;
}
```
To send the data to the client side application.
