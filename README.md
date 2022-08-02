# Home Assistant

## Description

## Containers
Home Assistant
Mosquitto MQTT
Zigbee MQTT

## Deployment
Docker Compose
Deployment script

## Configuration

## Services

## Automations

## Templates
###Conditions
For
```
{%- for light in states.light -%}
  {%- if light.state == 'off' -%}
    {{ light.entity_id }},
  {%- endif -%}
{%- endfor -%}
```
For (Print Index)
```
{%- for light in states.light if light.state == 'off'-%}
    {{loop.index}} - {{ light.entity_id }}
{% endfor %}
```
For (filtered)
```
{%- for light in states.light if light.state == 'off'-%}
    {{ light.entity_id }},
{%- endfor -%}
```

If/Else
```
{% if is_state('climate.thermostat', 'cool') %}
A window or door is open and the AC is on
{% endif %}
```

If/Else If
```
{% if is_state('climate.thermostat', 'cool') %}
The AC is on
{% elseid %}
The AC is off
{% endif %}
```
If with And/Or Logic
```
{% if 
is_state('binary_sensor.oc_contact_bedroom_01_window_01_contact', 'on') 
or is_state('binary_sensor.oc_contact_bedroom_01_window_02_contact', 'on')
or is_state('binary_sensor.oc_contact_livingroom_window_01_contact', 'on')
or is_state('binary_sensor.oc_contact_door_patio_contact', 'on')
or is_state('binary_sensor.oc_contact_office_window_contact', 'off')
%}
{% endif %}
```
```
{% if 
is_state('binary_sensor.oc_contact_bedroom_01_window_01_contact', 'off') 
and if is_state('climate.thermostat', 'cool') %}
{% endif %}
```
Nested If
```
{% if 
is_state('binary_sensor.oc_contact_bedroom_01_window_01_contact', 'on') 
%}
  {% if is_state('climate.thermostat', 'cool') %}
    A window or door is open and the AC is on
  {% endif %}
{% endif %}
```
Filter (To Upper or To Lower)
```
{% filter upper %}
    This text becomes uppercase
{% endfilter %}

{% filter lower %}
    This TEXT Becomes Lowercase
{% endfilter %}
```
State of an Entity
```
{{ states.binary_sensor.oc_contact_bedroom_01_window_01_contact.state }}
```
Is State of an Entity Equal to 'x'
```
{{ is_state('states.binary_sensor.oc_contact_bedroom_01_window_01_contact.state', "off") }}
```
### Snippets
Show the state of an entity
```
{{ states.binary_sensor.oc_contact_bedroom_01_window_01_contact.state }}
{{ states.binary_sensor.oc_contact_bedroom_01_window_02_contact.state }}
{{ states.binary_sensor.oc_contact_livingroom_window_01_contact.state }}
{{ states.binary_sensor.oc_contact_door_patio_contact.state }}
{{ states.binary_sensor.oc_contact_office_window_contact.state }}
{{ states.climate.thermostat.state }}
```

Alert if there is a door or window open when the AC is on
```
{% if 
is_state('binary_sensor.oc_contact_bedroom_01_window_01_contact', 'on') 
or is_state('binary_sensor.oc_contact_bedroom_01_window_02_contact', 'on')
or is_state('binary_sensor.oc_contact_livingroom_window_01_contact', 'on')
or is_state('binary_sensor.oc_contact_door_patio_contact', 'on')
or is_state('binary_sensor.oc_contact_office_window_contact', 'off')
%}
  {% if is_state('climate.thermostat', 'cool') %}
    A window or door is open and the AC is on
  {% endif %}
{% endif %}
```

Show a filtered list of entities
```
{{ 
  expand(states.light) 
  |selectattr('state', 'eq', 'off') 
  |map(attribute='entity_id')
  |list  
}}
```
## References
https://www.home-assistant.io Home Assitant
https://github.com/home-assistant  Home Assistant Gitgub
http://www.steves-internet-guide.com/mosquitto_pub-sub-clients/ Using The Mosquitto_pub and Mosquitto_sub MQTT Client Tools- Examples
