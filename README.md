# esp-doorbell-detection
A simple schematic to detect a doorbell signal, with [ESPHome](https://esphome.io/) (esp8266) based software to send the doorbell status to homeassistant. 

## Hardware

The circuit in the schematic is made to detect a ding-dong doorbell signal. The doorbell to detect is powered by 8v AC through an 230V to 8V transformer. The doorbell is connected to 8V transformer to in series with a push-button.
The detection circuit is connected in parallel over the doorbell.

![drawing](https://raw.githubusercontent.com/Bonusbartus/esp-doorbell-detection/master/hw/drawing.png "Schematic Drawing")
When the doorbell button is pressed, an opto-coupler pulls its output low. This could have been used to directly connect to a microcontroller, but I prefer an active-high and filtered signal.
Therefore the opto-coupler output connects to a BC547 transistor. When the opto output goes low, the transistor stops conducting and its emitter is pulled high. This active-high output is filtered by a simple RC filter, to only generate one pulse on the microcontroller input.

![waveform](https://raw.githubusercontent.com/Bonusbartus/esp-doorbell-detection/master/hw/wave.png "Simulated waveform of filtered output on 250ms button press")
