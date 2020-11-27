# Smart Bulb Pairing Device

I'm a huge fan of Ikea Trådfri Smart Zigbee Bulbs. But I have a problem with pairing them.
Here is an official video how to make a factory reset (after that the device will start pairing) — https://www.youtube.com/watch?v=npxOrPxVfe0

I have created a simple hardware-software system to automate the paring process.

## Hardware part

You will need:

 * [Sonoff bacic](https://s.click.aliexpress.com/e/_d6ok6xX)
 * [Power cord](https://s.click.aliexpress.com/e/_dUehkP5)
 * [Light socket](https://s.click.aliexpress.com/e/_dXjacBz)

To flash Sonoff basic you will need:

 * [USB Uart](https://s.click.aliexpress.com/e/_dX3KEcR)
 * [Female to female dupont](https://s.click.aliexpress.com/e/_dVSVDjD)
 * [Soldering iron](https://s.click.aliexpress.com/e/_d8MqQZD)
 * [Soldering flux](https://s.click.aliexpress.com/e/_d85HF7a)
 * [Solder wire](https://s.click.aliexpress.com/e/_dXHQo0P)
 * [Male pins](https://s.click.aliexpress.com/e/_dSrw2O3)

### Soldering pins

First of all you need to solder pins to sonoff basic.

### Flasing ESPHome Code

First of all you need to install ESPHome. There are several ways you can do it.
If you are using Home Assistant, you can install ESPHome addon with one click,
or you can install in on your machine with python command `pip3 install esphome`.

You need to connect USB Uart to Sonoff Basic (make sure NOT to plug it into power socket).

Put all the files from the section `ESHome code` to your direcotry.

And then you need to run:

```
esphome basic.yaml run
```

After successfull flash you can disconect USB Uart.

Then you should connect all the wires.

And then you can use the device.

## ESHome code

File `secrets.yaml` (you need to change the uppercase words with your data):

```
wifi_ssid: YOUR_SSID
wifi_password: YOUR_WIFI_PASSWORD

api_password: RANDOM_STRING_1
ota_password: RANDOM_STRING_2
```

file `basic.yaml`:

```
esphome:
  name: sonoffbasic
  platform: ESP8266
  board: esp01_1m
  board_flash_mode: dout

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

logger:

api:
  password: !secret api_password

ota:
  password: !secret ota_password

switch:
  - platform: gpio
    id: relay
    pin: GPIO12
    name: "Sonoff Relay"

binary_sensor:
  - platform: gpio
    pin:
      number: GPIO0
      mode: INPUT_PULLUP
      inverted: True
    name: "Sonoff Button"
    on_press:
      - switch.turn_on: relay
      - delay: 2s
      - switch.turn_off: relay

      - delay: 1000ms
      - switch.turn_on: relay
      - delay: 250ms
      - switch.turn_off: relay
      - delay: 1000ms
      - switch.turn_on: relay
      - delay: 250ms
      - switch.turn_off: relay
      - delay: 1000ms
      - switch.turn_on: relay
      - delay: 250ms
      - switch.turn_off: relay
      - delay: 1000ms
      - switch.turn_on: relay
      - delay: 250ms
      - switch.turn_off: relay
      - delay: 1000ms
      - switch.turn_on: relay
      - delay: 250ms
      - switch.turn_off: relay

      - delay: 1000ms
      - switch.turn_on: relay
```

## Credits

The original idea and the main part of the code was created by [Erelen](https://melda.ru).
