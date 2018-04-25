Welcome to our BAE 305 Final Project

Built by: Daniel Barton, Juma Baryaa, Noah Cornett, Gina DeGraves, Cory Radcliff, and Julia Wallin
# Background
For our initial project we were tasked with installing new transformers to create a power monitoring system for the Solar House. Unfortunately due to a tight budget, the architects could not allot us enough money to order the transformers or other parts we required. See the table below to find the estimated parts we intially needed along with the total cost.
![bae 305 - parts list pic](https://user-images.githubusercontent.com/38705148/39218117-fc6560e8-47f0-11e8-8cd9-c97e4d51c323.PNG)
Since the Solar House monitoring project was not going to work we had to switch focus. In order to keep with our intial focus of monitoring power, we decided to contact Dr. Colliver and ask about transformers to monitor power on a smaller scale. After several weeks of back and forth contact, we determined that this solution would take more time than we had availbile to us. So once again we decided to shift focus. Finally we decided the best move for the project was to focus on a much smaller scale. We chose to monitor power and current going through a DC circuit. 

# Summary
The purpose of our current monitoring system is to allow a user to insert any type of wire through the sensor and in return get a live current reading. This sytem is useful when wanting to measure current from DC circuit elements. Our simple design only consists of three parts. First, a current transducer that was used to measure current running through differnet elements. Next a Sparkfun Breadboard Power Supply Stick was used to power the sensor. Lastly, a ESP 32 Thing. This was programmed through Arduino to transform the current data onto a monitoring website. 

# Video

# Materials
* Sparkfun ESP 32 Thing
* Breadboard
* 10K Ohm Resistor
* HASS (50-600)-S Current Transducer
* Various Male-Male Jumper Wires
* 9V Battery Pack
* Sparkfun Breadboard Power Supply Stick 5V/3.3V
* Micro-USB Cable
* Arduino Programming Language

# Assembly Procedures



## Schematics


<img width="529" alt="sensor schematic" src="https://user-images.githubusercontent.com/38290976/39218202-7e71d1fc-47f1-11e8-971e-94e82dc78ffe.PNG">

<img width="522" alt="sensor info" src="https://user-images.githubusercontent.com/38290976/39218227-945800ea-47f1-11e8-831d-8b7645097c1f.PNG">

<img width="501" alt="esp 32 thing schematic" src="https://user-images.githubusercontent.com/38290976/39217570-a71edc6a-47ee-11e8-8f37-8b6907e3dd7e.PNG">



## Programming Code
The complete Arduino code for our project can be found [here](https://raw.githubusercontent.com/radcory15/Current-Monitor/master/1OURPROJECT.ino).

The code section below reads the input coming from the sensor and caluclautes the current based on equations within the spec sheet of our specific transducer. 
```

void loop() {
  // read the input on analog pin 0:
  int sensorValue = analogRead(36);
  Serial.print(sensorValue);
  Serial.print("\t");
  // Convert the analog reading 
  // On Uno,Mega,YunArduino:  0 - 1023 maps to 0 - 5 volts
  // On ESP8266:  0 - 1023 maps to 0 - 1 volts
  // On ESP32:  0 - 4095 maps to 0 - 1 volts, but will always return 0 as analogRead() has not been implemented yet
  // On MKR1000,Due: 0 - 4095 maps to 0 - 3.3 volts
  float voltage = sensorValue * ((VOLTAGE_MAX / VOLTAGE_MAXCOUNTS))+0.2;
  float current = (((voltage-2.5)* 300)/.625);
  Serial.print(voltage);
  Serial.print("V\t");
  Serial.println(current);  
  

```
# Test Equipment
Not Provided.
# Test Procedures
Not Provided.
# Test Results
No text provided

![](https://github.com/joedvorak/BAE305Example/blob/master/Design%20File%20Images/Graph.png)

# Discussion
## Design Decisions
Not Provided
## Test Results Discussion
The Peltier Live Well Cooler allows the user to select the desired maintenance temperature of the water in the live well using 2 pushbuttons and will then automatically cycle the system on and off while giving the user a real time readout of the current live well water temperature
# References
The Current Transducer and spec sheet we used can be found [here](https://docs-emea.rs-online.com/webdocs/146d/0900766b8146d0a8.pdf)

The Sparkfun Breadboard can be found [here](https://www.sparkfun.com/products/13032) and the Sparkfun ESP 32 Thing along with information and spec sheets can be found [here](https://www.sparkfun.com/products/13907)

We monitored the current through the use of a MATLAB application called [ThingSpeak](https://thingspeak.com/). The libraries that we used in order to write the current monitoring code can be found [here](https://www.arduinolibraries.info/libraries/thing-speak).




