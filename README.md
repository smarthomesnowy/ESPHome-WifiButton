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
In the ESPHome yaml file you can see how I made the two different "presses" of the button. I then used the Home Assistant call service to run a scene to turn off different zones of lighting at the same time with a single press and turn everything on with a double press.

```yaml
binary_sensor:

  - platform: gpio
    pin: D2
    name: "Bigbutton Motion Sensor"
    device_class: motion
    
  - platform: gpio
    pin:
      number: D3
      mode: INPUT_PULLUP
      inverted: True
    name: "Bigbutton"
    device_class: light
    filters:
      - delayed_on: 15ms
    on_press:
      then:
        - homeassistant.service:
            service: light.turn_off
            data:
              entity_id: light.all_lights
        - homeassistant.service:
            service: scene.turn_on
            data:
              entity_id: scene.fireplace_off
    on_double_click:
      then:
        - homeassistant.service:
            service: light.turn_on
            data:
              entity_id: light.all_lights
        - homeassistant.service:
            service: scene.turn_on
            data:
              entity_id: scene.fireplace_on
```

## Construction
I really want to get a battery inside the case of the button but I tried two different options before settling on a larger battery glued to the bottom of the case. This also adds a lot better battery capacity.

Fitting the D1 mini board in the area inside the case was also a challenge.

I used hot glue to keep the PIR sensor and DHT sensor inplace in the "knock out" holes in the sides of the button.

## Pictures / Video

## Conclusion
I spent about 12 euro on the D1 mini and the sensors, the button I found in a scrap bin and the phonebank I have had lying around for years and never used.
I use it every night to shut the house down and every morning to wake it up.
The battery life is about 5 days at the moment and I have set an alert in Home Assisant to warn me when the battery is at 5% so I can hook it up to the smart charger I have on my nightstand that charges my phone.

