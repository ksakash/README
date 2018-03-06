# Tmeplate for the GazeboRoboComp interface

```cpp
// RoboComp includes
#include <Ice/Ice.h>
#include <Component.h>
#include <innermodel/innermodel.h>

class ComponentI: virtual public RoboComp::Component {
public:
  componentI (gazebo::Component* component) {
		this->component = component;
  }
  
  virtual ~ComponentI(){};
  
  virtual RoboComp::ComponentDataPtr getComponentData(const Ice::Current&) {
    
    RoboComp::ComponentDataPtr ComponentData (new RoboComp::ComponentData());
    ComponentData->parameter1 = component->Parameter1;
    ComponentData->parameter2 = component->Parameter2;
    return ComponentData;
  
  }
  
  virtual setParameter (RoboComp::ComponentData ComponentData) {
    component->Value1 = ComponentData->value1;
    component->Value2 = ComponentData->value2;
  }
private:
  gazebo::Component* component;
};

```
