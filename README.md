# esp-doorbell-detection
A simple schematic to detect a doorbell signal, with [ESPHome](https://esphome.io/) (esp8266) based software to send the doorbell status to homeassistant. 

## Hardware

The circuit in the schematic is made to detect a ding-dong doorbell signal. The doorbell to detect is powered by 8v AC through an 230V to 8V transformer. The doorbell is connected to 8V transformer to in series with a push-button.
The detection circuit is connected in parallel over the doorbell.

![drawing] (https://raw.githubusercontent.com/Bonusbartus/esp-doorbell-detection/master/hw/drawing.png "Schematic Drawing")
To detect the button press on the AC doorbell, an opto-coupler is used. As the AC signal is 8V, a 4.7V Zener diode is used to keep the opto within spec.
When the doorbell button is pressed, the opto-coupler pulls its output low. This could have been used to directly connect to a microcontroller, but I prefer an active-high and filtered signal.
Therefore the opto-coupler output connects to a BC547 transistor. When the opto output goes low, the transistor stops conducting and its emitter is pulled high. This active-high output is filtered by a simple RC filter, to only generate one pulse on the microcontroller input.

![waveform] (https://raw.githubusercontent.com/Bonusbartus/esp-doorbell-detection/master/hw/wave.png "Simulated waveform of filtered output on 250ms button press")

## Software

The GPIO pin of the circuit is to be connected to a microcontroller. For this project I used a WeMos D1 mini, esp8266 based module in combination with [ESPHome](https://esphome.io/) to connect it to my [Home Assistant](https://www.home-assistant.io/) instance and send a push notification to the connected android companion apps.
ESPHome uses simple yaml files to generate code automatically. In this case the esp8266 is called esphome_doorbell. It is connected to my wifi AP, uses the ESPHome native api, OTA update api and a status LED. GPIO pin D7 is used to sense the doorbell signal and send a notification.
The mqtt broker is only enabled to be able to also get a message outside of Home Assistant.
Settings for the Wifi AP, API, OTA and MQTT are stored in a secrets.yaml [file](https://www.home-assistant.io/docs/configuration/secrets/).
