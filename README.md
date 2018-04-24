Welcome to our BAE 305 Final Project

Built by: Daniel Barton, Juma Baryaa, Noah Cornett, Gina DeGraves, Cory Radcliff, and Julia Wallin
# Background
For our initial project we were tasked with monitoring the power of the Solar House. Unfortunately due to the architects spending all the money, we did not have the budget for the parts we deemed necessary for this project which are listed below. 
![bae 305 - parts list pic](https://user-images.githubusercontent.com/38705148/39218117-fc6560e8-47f0-11e8-8cd9-c97e4d51c323.PNG)
Next we were going to use transformers to monitor power from a basic circuit from Dr. Colliver but that would require too much time that we didn't have. It was then decided that our group would monitor the current output from a circuit.  

# Summary
The purpose of the Peltier Live Well cooler was to allow for the control of the temperature of a live well while fishing on a lake. Without the ability to control the temperature of the live well, particularly on hot summer days, captured fish risk overheating as the temperature of the water in the live well climbs much higher than the deeper lake water they were caught from. With the Peltier Live Well cooler, the water temperature in the live well is able to be controlled without the need for heavy or expensive traditional compressors and refrigerant. This system could be useful in other applications as well, such as maintaining the temperature of water samples taken from the field.

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

# Assembly Procedures
The theory behind initial assembly was to get each electrical component working and communicating with the Arduino before adding the next.

The Temperature sensor and LED were wired to the Arduino using a temporary breadboard. A 4.7KOhm resistor was added between the white data line and red power line of the waterproof temperature probe.

The OLED display was wired according to the datasheet, with SCL and SDA going to pins A5 and A4 respectively while 3.3V of power was fed from the Arduino.



Now that the temperature probe and LED are all communicating with the Arduino, the pushbuttons and code to allow the selection of desired temperature are added.



The last electrical component controlled by the Arduino is added, the 10A relay. The relay allows the Arduino to use the information from the temperature sensor to turn on or off the other electrical assembly, the combination of the water pump, fans, and peltier chips.

![](https://github.com/joedvorak/BAE305Example/blob/master/Design%20File%20Images/RelayAlso.jpg)

The fan and heat sink are attached to the hot side of the Peltier chips with thermal paste and super glue. The water cooler is attached to the cold side of the Peltier chip with thermal paste and super glue. The entire assembly is then glued to the bottom of the container and ventilation holes are cut in the side of the tub. Process repeated for other water cooler assembly.

![](https://github.com/joedvorak/BAE305Example/blob/master/Design%20File%20Images/peltierheatsinkassembly.jpg)

The 30A, 12V power supply is connected to the project first through a switch and then on to the Arduino directly for external power, and to the Peltier/Fan/Water pump assemblies by way of the relay.

![](https://github.com/joedvorak/BAE305Example/blob/master/Design%20File%20Images/tundra3000.jpg)

## Schematics

<img width="330" alt="sensor pic" src="https://user-images.githubusercontent.com/38290976/39218278-d37a4bf2-47f1-11e8-9dc4-234df089c22a.PNG">

<img width="529" alt="sensor schematic" src="https://user-images.githubusercontent.com/38290976/39218202-7e71d1fc-47f1-11e8-971e-94e82dc78ffe.PNG">

<img width="522" alt="sensor info" src="https://user-images.githubusercontent.com/38290976/39218227-945800ea-47f1-11e8-831d-8b7645097c1f.PNG">

<img width="501" alt="esp 32 thing schematic" src="https://user-images.githubusercontent.com/38290976/39217570-a71edc6a-47ee-11e8-8f37-8b6907e3dd7e.PNG">



## Programming Code
See this repository for the Arduino Code.
This code reads the temperature sensor on the OneWire interface and converts it to Fahrenheit and Celsius
```C
for ( i = 0; i < 9; i++) {           // we need 9 bytes
    data[i] = ds.read();
    Serial.print(" ");
  }
  Serial.println();
  // Convert the data to actual temperature

  int16_t raw = (data[1] << 8) | data[0];
  if (type_s) {
    raw = raw << 3; // 9 bit resolution default
    if (data[7] == 0x10) {
      // "count remain" gives full 12 bit resolution
      raw = (raw & 0xFFF0) + 12 - data[6];
    }
  } else {
    byte cfg = (data[4] & 0x60);
    // at lower res, the low bits are undefined, so let's zero them
    if (cfg == 0x00) raw = raw & ~7;  // 9 bit resolution, 93.75 ms
    else if (cfg == 0x20) raw = raw & ~3; // 10 bit res, 187.5 ms
    else if (cfg == 0x40) raw = raw & ~1; // 11 bit res, 375 ms
    //// default is 12 bit resolution, 750 ms conversion time
  }
  celsius = (float)raw / 16.0;
  fahrenheit = celsius * 1.8 + 32.0;
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
The Current Transducer and spec sheet we used can be found [here](https://learn.sparkfun.com/tutorials/esp32-thing-hookup-guide)

The Sparkfun Breadboard can be found here: (https://www.sparkfun.com/products/13032) and the Sparkfun ESP 32 Thing along with information can be found here (https://learn.sparkfun.com/tutorials/esp32-thing-hookup-guide)




