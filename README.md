Welcome to the BAE 305 Final Project for the Solar House Group
# Tundra 3000
Built by: Students in BAE 305 in Spring 2017
# Summary
The purpose of the Peltier Live Well cooler was to allow for the control of the temperature of a live well while fishing on a lake. Without the ability to control the temperature of the live well, particularly on hot summer days, captured fish risk overheating as the temperature of the water in the live well climbs much higher than the deeper lake water they were caught from. With the Peltier Live Well cooler, the water temperature in the live well is able to be controlled without the need for heavy or expensive traditional compressors and refrigerant. This system could be useful in other applications as well, such as maintaining the temperature of water samples taken from the field.
# Materials
## Materials
* Arduino Uno
* 2x Peltier Chips
* Waterproof Temperature Probe
* DC Water Pump
* 2x 12v Fans w/heat sinks
* 10 Amp Relay
* 4.7KOhm Resistor
* OLED Screen
* 2x Pushbuttons
* Pushbutton Resistors
* Wire
* 3/8" Inside Diameter Tubing
* Large Plastic Container
* Super Glue
* Thermal Paste
## Tools and Supplies
* 30 Amp, 12V Power Supply
* Solder
* Soldering Iron
* Box Knife
* Wire Strippers
* Small Flathead Screwdriver
# Assembly Procedures
The theory behind initial assembly was to get each electrical component working and communicating with the Arduino before adding the next.

The Temperature sensor and LED were wired to the Arduino using a temporary breadboard. A 4.7KOhm resistor was added between the white data line and red power line of the waterproof temperature probe.

The OLED display was wired according to the datasheet, with SCL and SDA going to pins A5 and A4 respectively while 3.3V of power was fed from the Arduino.

![](https://github.com/joedvorak/BAE305Example/blob/master/Design%20File%20Images/SensorandLED6.jpg)

Now that the temperature probe and LED are all communicating with the Arduino, the pushbuttons and code to allow the selection of desired temperature are added.

![](https://github.com/joedvorak/BAE305Example/blob/master/Design%20File%20Images/ButtonsToo.jpg)

The last electrical component controlled by the Arduino is added, the 10A relay. The relay allows the Arduino to use the information from the temperature sensor to turn on or off the other electrical assembly, the combination of the water pump, fans, and peltier chips.

![](https://github.com/joedvorak/BAE305Example/blob/master/Design%20File%20Images/RelayAlso.jpg)

The fan and heat sink are attached to the hot side of the Peltier chips with thermal paste and super glue. The water cooler is attached to the cold side of the Peltier chip with thermal paste and super glue. The entire assembly is then glued to the bottom of the container and ventilation holes are cut in the side of the tub. Process repeated for other water cooler assembly.

![](https://github.com/joedvorak/BAE305Example/blob/master/Design%20File%20Images/peltierheatsinkassembly.jpg)

The 30A, 12V power supply is connected to the project first through a switch and then on to the Arduino directly for external power, and to the Peltier/Fan/Water pump assemblies by way of the relay.

![](https://github.com/joedvorak/BAE305Example/blob/master/Design%20File%20Images/tundra3000.jpg)

## Schematics
Red wire= positive, Green wire= negative, Blue wire= analog/digital outputs

![](https://github.com/joedvorak/BAE305Example/blob/master/Design%20File%20Images/Tundra%203000_schem.gif)

## Engineering Drawings
Not Provided.
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
The OLED was connected with help from this instructable: http://www.instructables.com/id/Monochrome-096-i2c-OLED-display-with-arduino-SSD13/
After finding the correct i2c address for the display using the i2c scanner, an example sketch from the Adafruit SSD1306 library was uploaded to the Arduino. After verification that the OLED was working, code from the example sketch was reworked into the code needed to display our data.
Similarly, code to run the temperature probe was borrowed from the OneWire library example sketch and modified to meet the needs of the project.
