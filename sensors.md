## Sensors

### Time of Day

```
- platform: tod
  name: Night
  after: sunset
  before: sunrise

- platform: tod
  name: Quiet TIme
  after: '21:00'
  before: '06:00'
```

### Workday
```
- platform: workday
  country: US
  province: WI
  workdays: [mon, tue, wed, thu, fri]
```

### History Stats
```
- platform: history_stats
  name: Lamp on today
  entity_id: light.my_lamp
  state: "on"
  type: time
  start: "{{ now().replace(hour=0, minute=0, seconds=0 }}"
  end: "{{ now() }}
  
- platform: history_stats
  name: Washer Running
  entity_id: sensor.washer_status
  state: "running"
  type: time
  end: "{{ now() }}"
  duration:
    days: 7
```
### Utility
```
- platform: history_stats
  name: Front Door Motion
  entity_id: binary_sensor.sensor.motion_front_door
  state: 'on'
  end: '{{ now() }}'
  duration:
    days: 7
    
utility_meter:
  hourly_frontdoor_motion:
    source: sensor.front_door_motion
    cycle: hourly
  daily_frontdoor_motion:
    source: sensor.front_door_motion
    cycle: daily
```

### Rest API
```
binary_sensor:
  - platform: template
    sensors:
      nws_alerts_are_active:
        friendly_name: NWS Alerts Are Active
        #entity_id: sensor.nws_alerts
        value_template: >
          {{ states('sensor.nws_alerts') | int(0) > 0 }}
        icon_template: >-
          {% if states('sensor.nws_alerts') | int(0) > 0 %}
            mdi:weather-lightning
          {% else %}
            mdi:weather-sunny
          {% endif %}
```
