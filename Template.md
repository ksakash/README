# Template for a Plugin

```cpp
// Gzebo includes
#include <gazebo/physics/physics.hh>
#include <gazebo/transport/TransportTypes.hh>
#include <gazebo/common/Time.hh>
#include <gazebo/common/Plugin.hh>
#include <gazebo/common/Events.hh>

// ICE includes
#include <Ice/Ice.h>
#include <IceUtil/IceUtil.h>
#include <easyiceconfig/EasyIce.h>

// executed when the plugin is loaded
void *mainThread(void* v);

namespace gazebo
{

   class GazeboRoboComp : public ModelPlugin
   {
      // Constructor
      public: GazeboRoboComp();

      // Destructor
      public: virtual ~GazeboRoboComp();

      // Called after the plugin has been constructed 
      public: void Load( physics::ModelPtr _parent, sdf::ElementPtr _sdf ){
        
        // Store the pointer to the model
        this->model = _parent;

        // Listen to the update event. This event is broadcast every
        // simulation iteration.
        this->updateConnection = event::Events::ConnectWorldUpdateBegin(
          std::bind(&ModelPush::OnUpdate, this));
          
        // by creating the thread here we initialize the communication setup before the sensor starts
        // working, through which it will interact with the robotics component system
        pthread_t thr_gui;
        pthread_create(&thr_gui, NULL, &mainThread, (void*)this);
      }

      // Update the controller
      public: void OnUpdate(){
        // some action to be performed
      }
      
      // Pointer to the model
      private: physics::ModelPtr model;

      // Pointer to the update event connection
      private: event::ConnectionPtr updateConnection;

   };
   
   // Register this plugin with the simulator
  GZ_REGISTER_MODEL_PLUGIN(GazeboRoboComp)

}
```
