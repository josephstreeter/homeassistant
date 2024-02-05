# Home Assistant

## Description

## Containers
- Home Assistant
- Mosquitto MQTT
- Zigbee MQTT
- https://github.com/Koenkk/zigbee2mqtt/issues/10858#issuecomment-1291567559

## Deployment
Setup
```
mkdir homeassistant
mkdir homeassistant/config
```
Deploy Containers
```
docker-compose up 
```
### Update Containers
The following commands will update the containers
```
docker-compose build
docker-compose down
docker-compose up -d --force-recreate
docker rmi $(docker images -f "dangling=true" -q) -f
```

## Configuration

## Services

## Automations

## Templates

## References
- https://www.home-assistant.io Home Assitant
- https://github.com/home-assistant  Home Assistant Github
- http://www.steves-internet-guide.com/mosquitto_pub-sub-clients/ Using The Mosquitto_pub and Mosquitto_sub MQTT Client Tools- Examples
