## Templates
### View Entities
List the attributes of an Entity
```
{{ states.light.kitchen_sink_light.attributes }}
```
View the value of an attribute
```
{{ state_attr('light.kitchen_sink_light', 'friendly_name') }}
```
State of an Entity
```
{{ states.binary_sensor.oc_contact_bedroom_01_window_01_contact.state }}
```
Is State of an Entity Equal to 'x'
```
{{ is_state('states.binary_sensor.oc_contact_bedroom_01_window_01_contact.state', "off") }}
```

### Manipulate Text
Capitalize
{{ states('light.kitchen_sink_light') | capitalize}}

To Upper
```
{{ states('light.kitchen_sink_light') | upper}

```
To Lower
```
{{ states('light.kitchen_sink_light') | lower}

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
