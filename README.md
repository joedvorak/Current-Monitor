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


<img width="529" alt="sensor schematic" src="https://user-images.githubusercontent.com/38290976/39218202-7e71d1fc-47f1-11e8-971e-94e82dc78ffe.PNG">

<img width="522" alt="sensor info" src="https://user-images.githubusercontent.com/38290976/39218227-945800ea-47f1-11e8-831d-8b7645097c1f.PNG">

<img width="501" alt="esp 32 thing schematic" src="https://user-images.githubusercontent.com/38290976/39217570-a71edc6a-47ee-11e8-8f37-8b6907e3dd7e.PNG">



## Programming Code
See this repository for the Arduino Code. All of our code can be found [here](https://raw.githubusercontent.com/radcory15/Current-Monitor/master/1OURPROJECT.ino).
This code retireves the data that the current sensor is measuring and transmits it to a monitroing website. 
```/*
  WriteVoltage
  
  Reads an analog voltage from pin 0, and writes it to a channel on ThingSpeak every 20 seconds.
  
  ThingSpeak ( https://www.thingspeak.com ) is an analytic IoT platform service that allows you to aggregate, visualize and 
  analyze live data streams in the cloud.
  
  Copyright 2017, The MathWorks, Inc.
  
  Documentation for the ThingSpeak Communication Library for Arduino is in the extras/documentation folder where the library was installed.
  See the accompaning licence file for licensing information.
*/

#include "ThingSpeak.h"

// ***********************************************************************************************************
// This example selects the correct library to use based on the board selected under the Tools menu in the IDE.
// Yun, Ethernet shield, WiFi101 shield, esp8266 and MKR1000 are all supported.  
// EPS32 is only partially compatible. ADC analogRead() for ESP32 has not yet been implemented in the SparkFun library.  It will always return 0.  
// With Yun, the default is that you're using the Ethernet connection.
// If you're using a wi-fi 101 or ethernet shield (http://www.arduino.cc/en/Main/ArduinoWiFiShield), uncomment the corresponding line below
// ***********************************************************************************************************

//#define USE_WIFI101_SHIELD
//#define USE_ETHERNET_SHIELD

#if !defined(USE_WIFI101_SHIELD) && !defined(USE_ETHERNET_SHIELD) && !defined(ARDUINO_SAMD_MKR1000) && !defined(ARDUINO_AVR_YUN) && !defined(ARDUINO_ARCH_ESP8266) && !defined(ARDUINO_ARCH_ESP32)
  #error "Uncomment the #define for either USE_WIFI101_SHIELD or USE_ETHERNET_SHIELD"
#endif

#if defined(ARDUINO_AVR_YUN)
    #include "YunClient.h"
    YunClient client;
#else
  #if defined(USE_WIFI101_SHIELD) || defined(ARDUINO_SAMD_MKR1000) || defined(ARDUINO_ARCH_ESP8266) || defined(ARDUINO_ARCH_ESP32)
    // Use WiFi
    #ifdef ARDUINO_ARCH_ESP8266
      #include <ESP8266WiFi.h>
    #elif defined(ARDUINO_ARCH_ESP32)
      #include <WiFi.h>
	#else
      #include <SPI.h>
      #include <WiFi101.h>
    #endif
    char ssid[] = "The Floo Network";    //  your network SSID (name) 
    char pass[] = "Connecto-patronum";   // your network password
    int status = WL_IDLE_STATUS;
    WiFiClient  client;
  #elif defined(USE_ETHERNET_SHIELD)
    // Use wired ethernet shield
    #include <SPI.h>
    #include <Ethernet.h>
    byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED};
    EthernetClient client;
  #endif
#endif

#ifdef ARDUINO_ARCH_AVR
  // On Arduino:  0 - 1023 maps to 0 - 5 volts
  #define VOLTAGE_MAX 5.0
  #define VOLTAGE_MAXCOUNTS 1023.0
#elif ARDUINO_SAMD_MKR1000
  // On MKR1000:  0 - 1023 maps to 0 - 3.3 volts
  #define VOLTAGE_MAX 3.3
  #define VOLTAGE_MAXCOUNTS 1023.0
#elif ARDUINO_SAM_DUE
  // On Due:  0 - 1023 maps to 0 - 3.3 volts
  #define VOLTAGE_MAX 3.3
  #define VOLTAGE_MAXCOUNTS 1023.0  
#elif ARDUINO_ARCH_ESP8266
  // On ESP8266:  0 - 1023 maps to 0 - 1 volts
  #define VOLTAGE_MAX 1.0
  #define VOLTAGE_MAXCOUNTS 1023.0
#elif ARDUINO_ARCH_ESP32
  // On ESP32:  0 - 4096 maps to 0 - 1 volts
  #define VOLTAGE_MAX 3.3
  #define VOLTAGE_MAXCOUNTS 4095.0
#endif

/*
  *****************************************************************************************
  **** Visit https://www.thingspeak.com to sign up for a free account and create
  **** a channel.  The video tutorial http://community.thingspeak.com/tutorials/thingspeak-channels/ 
  **** has more information. You need to change this to your channel, and your write API key
  **** IF YOU SHARE YOUR CODE WITH OTHERS, MAKE SURE YOU REMOVE YOUR WRITE API KEY!!
  *****************************************************************************************/
unsigned long myChannelNumber = 481699;
const char * myWriteAPIKey = "F7SY9GB9O9LYBSSL";

void setup() {
  
  #ifdef ARDUINO_AVR_YUN
    Bridge.begin();
  #else   
    #if defined(ARDUINO_ARCH_ESP8266) || defined(USE_WIFI101_SHIELD) || defined(ARDUINO_SAMD_MKR1000) || defined(ARDUINO_ARCH_ESP32)
      WiFi.begin(ssid, pass);
    #else
      Ethernet.begin(mac);
    #endif
  #endif
  Serial.begin(115200);
 Serial.println("Starting ThingSpeak");
 ThingSpeak.begin(client);
}

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
  
 
 

  // Write to ThingSpeak. There are up to 8 fields in a channel, allowing you to store up to 8 different
  // pieces of information in a channel.  Here, we write to field 1.
  ThingSpeak.writeField(myChannelNumber, 1, current, myWriteAPIKey);
  delay(20000); // ThingSpeak will only accept updates every 15 seconds.
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




