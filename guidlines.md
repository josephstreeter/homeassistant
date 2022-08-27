## Guidlines
The purpose of this page is to lay out general guidlines for configuring Home Assistant in such a way as to maximize functionality and flexibility while limiting complexity as much as possible. 

## Components
- Entities
- Template Sensors
- Groups
- Scripts
- Automations
- Areas

## Layers
### Layer 1
The first layer consists of the device entities themselves

### Layer 2
The second layer consolidates entity states using Template Sensors or Groups. 

Additional Template Sensors are used to enable or disable functionality through conditions in automations
Examples
- Manually disable automations
- Trigger automations based on entity state(s)

### Layer 3
The third layer consists of AUtomations. Automations will use Template Sensors or Groups as tiggers and conditions

## Layer 4
THe fourth layer consists of Areas, Scripts, and Scenes that are executed by Automations.

## Layer 5
The fifth layer consists of Groups that are used to consolidate recipients for notifications
