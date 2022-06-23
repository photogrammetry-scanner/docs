# C-code Feeder Arduino CNC shield

A simple G-code feeder for the Arduino UNO CNC shield that is accessed wirelessly. Commands can be sent manually by the http:// api or automatically from G-code file.

**Description**

The G-code feeder (Wemos D1 mini) communicates by serial connection with the CNC controller (Arduino UNO + GRBL firmware).
The communication stream consits of standard G-codes (from feeder to CNC controller) and GRBL responses (from CNC controller to feeder).
The feeder provides a simplistic user interface by http://. 
G-code commands can be sent either by http:// API calls or from G-code file. 
In case of file, it is read line by line from the controller's file system and sent to the CNC controller.

<img width=50% src="https://github.com/photogrammetry-scanner/docs/blob/main/images/overview.png" />

**Prerequisites**

1. Wemos D1 mini
1. Wemos Oled display
1. Arduino UNO
1. Arduino UNO CNC shield
2. stepper drivers (for example A4988)
3. stepper motors
4. servo
5. some resistors 

**Wiring**

Note: the transmission from Arduino is 5V compatible whereas the Wemos D1 mini reception is 3.3V compatible. 
Here the level shift is done by 1k/2.2k voltage divider.

<img width=50% src="https://github.com/photogrammetry-scanner/docs/blob/main/images/sketch.jpg" />
