## Templates
### View Entities

State of an Entity
```
{{ states.sensor.oc_temp_upstairs_temperature.state }}
```
or
```
{{ states('sensor.oc_temp_upstairs_temperature') }}
```

Is State of an Entity Equal to 'x'
```
{{ is_state('states.binary_sensor.oc_contact_bedroom_01_window_01_contact.state', "off") }}
```

List the attributes of an Entity
```
{{ states.light.kitchen_sink_light.attributes }}
```

View the value of an attribute
```
{{ state_attr('light.kitchen_sink_light', 'friendly_name') }}
```

### Manipulate Text
Capitalize
```
{{ states('light.kitchen_sink_light') | capitalize}}
```

To Upper
```
{{ states('light.kitchen_sink_light') | upper}

```

To Lower
```
{{ states('light.kitchen_sink_light') | lower}

```

Concatination
```
{% for state in states.sensor %}
{{state.name ~ '=' ~ state.state }}
{% endfor %}
```

Join
```
{% set output = namespace(sensors=[]) %}
{% for state in states.sensor %}
{% set output.sensors = output.sensors + [state.name ~ "(" ~ state.state ~ ")"] %}
{% endfor %}
{{ output.sensors | join(',') }}
```
### Variables
Set Variable
```
{% set outside_temp = state_attr('climate.thermostat', 'current_temperature') %}
```

Print variable
```
{{ outside_temp }}
```

Create an Array
```
{% set my_array = (1,2,4,5) %}
```
Create an Array from HA data
```
{% set output = namespace(sensors=[]) %}
{% for state in states.sensor %}
{% set output.sensors = output.sensors + [state.name] %}
{% endfor %}
{{ output.sensors }}
```

### Math
Subtraction
```
{% set outside_temp = state_attr('climate.thermostat', 'current_temperature') %}
{% set inside_temp = states('sensor.oc_temp_upstairs_temperature') %}
{% set difference = (outside_temp | float - inside_temp | float) %}
{{ difference }}
```

Greater than or less than
```
{% set outside_temp = state_attr('climate.thermostat', 'current_temperature') %}
{% set inside_temp = states('sensor.oc_temp_upstairs_temperature') %}
{% set difference = (outside_temp | float < inside_temp | float) %}
{{ difference }}
```

Average
```
{% set outside_temp = state_attr('climate.thermostat', 'current_temperature') %}
{% set inside_temp = states('sensor.oc_temp_upstairs_temperature') %}
{% set average = (((outside_temp | float + inside_temp | float)) / 2) | round(2)%}
{{ average }}
```

### Conditions
For
```
{%- for light in states.light -%}
  {%- if light.state == 'off' -%}
    {{ light.entity_id }},
  {%- endif -%}
{%- endfor -%}
```
or 
```
{% set output = namespace(sensors=[]) %}
{% for state in states.sensor %}
{{ state.entity_id }} = {{ state.state }}
{% endfor %}
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

Assign results of loop to an array
```
{% set output = namespace(sensors=[]) %}
{% for state in states.sensor %}
{% set output.sensors = output.sensors + [state.name ~ ',' ~ state.state] %}
{% endfor %}
{{ output.sensors }}
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
or
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
Show all entities from an integraion
```
{{ integration_entities('mqtt') }}
```
