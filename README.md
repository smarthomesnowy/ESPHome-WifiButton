# ESPHome-WifiButton

## Idea

Create a wireless button to control Home Assistant from a big industrial control ON/OFF button.
The button will have an ESP8266 running ESPHome and will have the logic for single press, double press and long press therefore being able to do three different actions on one single button.
It will sit in my bedrrom and run autumations to turn on off all the lights and a wake up autumation.
The button will be powered with a mobile phone powerbank to make it portable.
Onboard there will be a PIR motion sensor that ties into the Home Assistant Alarmo home alarm system and a temprature and humidty sensor that ties into the climate control system.

## Parts used

- An industrial control buttton used to open and close roller shutter doors.

- Wemos D1 mini ESP8266
- DHT22 Temprature and Humidity sensor
- Small PIR motion sensor
- 10MHA powerbank

## Coding
### ESPHome
The main challenge in the coding of this device was making the battery sensor.
I wanted to have the actual voltage the battery was supplying and also a % amount of the battery remaining.
```yaml
sensor:

  - platform: adc
    pin: A0
    id: "BigbuttonBattery"
    name: "BigbuttonBattery"
    update_interval: 300s
    accuracy_decimals: 1
    filters:
      - multiply: 4.15
      
  - platform: template
    name: "BigbuttonBatteryVoltage"
    unit_of_measurement: 'V'
    update_interval: 300s
    accuracy_decimals: 1
    lambda: |-
      return (id(BigbuttonBattery).state);
      
  - platform: template
    name: "BigbuttonBatteryPercentage"
    unit_of_measurement: '%'
    update_interval: 300s
    accuracy_decimals: 1
    lambda: |-
      return ((id(BigbuttonBattery).state /4.1) * 100.00);


```
### Home Assistant
some coding

## Construction
some text
## Pictures / Video

