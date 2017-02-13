# Thinger.io ClimaStick Quick Start Gide

This board is a complete Internet of Things development kit, that integrates a full programable WiFi transmissor and all environmetal and inertial sensors you will need to create tons of connected projects easily than ever, thanks to thinger.io libraries and infraestructure. 

it could be user to:
- Meassure environmetral variables such as Temperature, relative humidity, barometric pressure or luminic radiation.
- Take inertial datas like vibrations, linear and angular accelerations. 
- Export all data for bucket storage or monitoricing process.

## WARNINGS
##### GENERAL ADVERTISEMENT

-  It should be powered with a 5V and 250 to 1000mah USB power supply.
-  This board has a low heat dissipation capacity, so it is normal that it keeps hot on high transmission processes. The temperature sensor could takes values up to 70ºC on full duplex communication process.
-  If you use the VIN power header, be careful to connect in the correct position, as it is showed on image2. Do not follow this directive could damage the protection diod.

<img src="https://github.com/jtrinc266/myThinger/blob/master/Images/i1.JPG?raw=true"   width="200" height="180" />
-  This device is developed like a software testing platform and it is not protected to support hard weather conditions without the appropriate cover case.
- Do not touch the components Surfaces to grab the device, it will sport the electrical contacts, producing shortcuts and wrong working. keep holding from lateral edges like in the illustration below:

<img src="https://github.com/jtrinc266/myThinger/blob/master/Images/i2.PNG?raw=true"   width="200" height="300" />
- If it is necessary, clean the circuit using a non-damaging contact cleaner like Isopropyl alcohol and soft brush. 
-  Store in a cool, dry place. Protected from dust.

#### BATTERY POWERING
- Using BAT power header, be careful to wire up correctly, as it is showed on image below:

<img src="https://github.com/jtrinc266/myThinger/blob/master/Images/i3.PNG?raw=true"   width="200" height="180" />
 - BAT header is connected to a lithium battery charger that can manage 3.7Vdc, 500mah Li-Po / li-ion batteries charge and discharge process. 

&#9888; if you are ussing different battery, plug it on VIN connector.

&#9888; if cell voltage flows under 3.6V, an authomatic battery protection circuit will power off the system. 

 - To load a battery, connect it on BAT heather and power on the ClimaStick through USB / VIN connectors. The battery charger will manage the charging voltage to increase live battery and stops charging cycle when voltage ups to 4.2Vdc.

 - Never use a computer USB port to battery charging, it could damage your computer’s circuitry. 


#### EXENCION OF RESPONSABILITY
- This device its commercialized by Thinger.io platform like a software development kit, so it is not subject to commerce homologation rules. The owner is liable for all injuries to third parties and damage to their properties. 

## GETTING STARTED
#### RUNNING STOCK EXAMPLE CODE:
> 1. Power up the board with a micro USB-B 5V supply, like a cell phone charger. 
2. Sign-up on thinger.io
3. Using a Smartphone, look for and connect to “Thinger device” LAN.
4. Open your web browser and enter your Thinger.io account name, and your WiFi credentials.
5. Get out of “thinger device” WiFi Access point and connect to your home WiFi.
6. Open your browser on thinger.io > dashboards > ClimaStick_Example

#### START PROGRAMING
1- Download and install the working environements:
 - CP2102 drivers from silicon labs web site:
http://www.silabs.com/products/mcu/pages/usbtouartbridgevcpdrivers.aspx
 - Arduino IDE v1.6.13 or newer:
 https://www.arduino.cc/en/main/software
 
2- Configure Arduino IDE, opening next interfaces:
  > 1. On File>Preferences>Additional_Boards_URL_Manager to include the "ESP8266 boards manager link" that you can retrieve from Github community project: https://github.com/esp8266/Arduino
2. On Tools>Board>Board_manager and look for ESP8266 community board firmware, and install the last version.
3. On Tools>Board, Select **NODE_MCU V1.0 (ESP-12E)** board
4. On Edit>include_libraries>libraries_manager, look for the last version of **Thinger.io** libraries. 
5. Connect the ClimaStick to your computer and select its serial communication port number on: Tools>port
6. Now you can start developing with Thinger.io ClimaStic. Check out the source code examples opening Arduino IDE: File>Examples>Thinger.io>ClimaStick Basic

#### UPLOADING FIRMWARE 
- Be sure that your USB wire allows data transmission.
- Verify that your operative system recognised the CP2102 serial port interface.
- Check out the selected serial COM number on Arduino IDE: Tools>port
- On Arduino IDE main menu, press &#9658; button to start compiling and flashing de firmware.

&#9888; **Flash boot mode:** If you follow "uploading firmware steps", and there is any problem to stablish the communication with the board, you can force a flash boot up keeping pressed USR button and making reset (pressing RST button).
 
## CLIMASTICK LIBRARY
#### EASY FUNCTIONS
Clima_move.h library is included on Thinger.io libraries. It contains all sensor integration code that you will need to read variables. next list shows all function names that retrieves real time read values:
```cpp
 void getMotion();
 void getCompass();
 void getMagnet();
 void getClima();
 void getTime();
 float getBatteryVoltage();
 float getBatteryLoad();
```
After execute this functions, the retrieve variables will be ready to work with readed values. Next list shows all this variables:
 - Accelerometer: ax, ay, az.
 - Gyroscope: gx, gy, gz.
 - Magnetometer:  mx, my, mz.
 - Compass: heading, headingDegrees.
 - Environmental: temperature, humidity, altitude, pressure, vissibleSpectum, IRSpectum;
 - Time: hour, minute, second
 
#### EMBEDED RGB LED


```cpp
 rgb(String "colorName");
 rgb(int red, int green, int blue);
```

#### ENVIRONMENTAL SAMPLING PROTOCOL
Due to the low power dissipation capacity, climaSticks gets hot after a few seconds of working, so it is not possible to make high-rate real time temperature reads. For reading temperature process, it is necessary to hibernate the processor, and wait a few minutes before to execute a high precision temperature read. This function is available using ESP.deepsleep(<-milliseconds->). during specified time period, the running process will be stopped,  and processor will not respond to any inquiry or WiFi transmissions. When the countdown is over, the device make a hard reset and the process will be start from new.


```cpp
#include <Clima_Move.h>

#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"

void setup() {
  thing.add_wifi(SSID, SSID_PASSWORD);
  climaMove(thing);   
} 
 
void loop(){
  thing.handle();
  
  thing.stream(thing["environment"]);
  
  if(sleepMode==1){
      ESP.deepSleep(5*60*1000000);      //Sleeping 5 minutes until reset 
  }

}
```

>**&#9888; DEEPSLEEP CONSIDERATIONS:** 

> - During the deepSleep mode, it is not possible to flash code. To change the program you will need to make a forced flash mode boot up.
> - Note that, when the processor makes a hard reset, all dynamic variables will lost its values. 
