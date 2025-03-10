---
description: >-
  The Thinger32NB-IoT is a compact dev board that combines Quectel BC660 with an
  Espressif ESP32-PICO-D4 to provide a simple to use development board for IoT
  projects.
---

# Thinger32 NB-IoT

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Unlock the **next level of IoT development** with this **ESP32 + Quectel BC66 hybrid board**. Combining the **power and flexibility** of the **ESP32** with the **low-power, wide-area communication** of the **NB-IoT Quectel BC66**, this board is the **perfect solution for developing IoT projects that demand high efficiency, long-range connectivity, and seamless integration**.

* The BC660K-GL is a high-performance LTE Cat NB2 module which supports multiple frequency bands of B1/ 2/ 3/ 4/ 5/ 8/ 12/ 13/ 17/ 18/ 19/ 20/ 25/ 28/ 66/ 70/ 85 with extremely low power consumption, it provides a flexible and scalable platform for the migration from GSM/GPRS to NB-IoT networks.
* The ESP32-PICO-D4 is a powerful MCU module with WiFi, Bluetooth®, and BLE connectivity and comes integrated with 8MB SPI flash, 2MB SPI Pseudo static RAM (PSRAM), and a 40 MHz crystal oscillator. The ESP32 microcontroller itself features two CPU cores that can be individually controlled, with an adjustable clock frequency between 80 - 240MHz and a low-power co-processor for minor tasks, such as monitoring peripherals. It supports a range of peripherals, including an SD card interface, capacitive touch sensors, ADC, DAC, Two-Wire Automotive Interface (TWAI), Ethernet, high-speed SPI, UART, I2S, I2C, etc.

Designed by Thinger.io team, this board has been made for **developers, makers, and industry professionals** leveraging the best integration with Thinger.io platform. This board enables **Wi-Fi, Bluetooth, and NB-IoT** connectivity, all in a compact form factor with **USB 3.0 support and up to 10 versatile GPIO options**.

## **Technical Specifications**

<table data-header-hidden><thead><tr><th width="336">Feature</th><th>Specification</th></tr></thead><tbody><tr><td><strong>Feature</strong></td><td><strong>Specification</strong></td></tr><tr><td>NB-IoT bands</td><td>B1/ 2/ 3/ 4/ 5/ 8/ 12/ 13/ 17/ 18/ 19/ 20/ 25/ 28/ 66/ 70/ 85</td></tr><tr><td><strong>WiFi Connectivity</strong></td><td><ul><li>WiFi 802.11 b/g/n</li><li>150Mbps</li><li>2412 - 2484MHz</li></ul></td></tr><tr><td><strong>Bluetooth Connectivity</strong></td><td><ul><li>Bluetooth v4.2 Specification</li><li>BR/EDR and BLE</li><li>Transmitter Class: 1, 2, and 3</li></ul></td></tr><tr><td></td><td></td></tr><tr><td><strong>ESP32 specs</strong></td><td><p></p><p>Xtensa® Dual-Core 32-bit LX6 Microprocessor (up to 240MHz)</p><ul><li>448KB of ROM and 520KB SRAM</li><li>8MB SPI Flash</li><li>2MB PSRAM</li><li>16KB SRAM in RTC</li></ul></td></tr><tr><td></td><td></td></tr><tr><td><strong>GPIO</strong></td><td>10 configurable (UART, I2C, SPI)</td></tr><tr><td><strong>SIM Support</strong></td><td>NanoSIM 4FF slot</td></tr><tr><td><strong>Programming</strong></td><td>Arduino Framework, MicroPython, ESP-IDF</td></tr><tr><td><strong>Buttons</strong></td><td>ESP32 Reset<br>ESP32 Boot</td></tr><tr><td><strong>Connections</strong></td><td>USB 3.0 with CP2102 interface<br>ITX1 Antenna connector WiFi Blutetooth<br>ITX1 Antenna connector NB-IoT<br>2X16P 2.54mm pins</td></tr><tr><td><strong>LEDS</strong></td><td>BC PWR Red<br>BC NET  Amber<br>ESP32 UART Tx Green</td></tr><tr><td><strong>Supported peripherals</strong></td><td>SD card, UART, SPI, SDIO, I2C, LED PWM, Motor PWM, I2S, IR, pulse counter, GPIO, capacitive touch sensor, ADC, DAC, TWAI® (compatible with ISO 11898-1, i.e. CAN Specification 2.0), Ethernet MAC</td></tr></tbody></table>

### **Electrical Characteristics**

| **Power Source**   | **Voltage** | **Current**                                      |
| ------------------ | ----------- | ------------------------------------------------ |
| USB 3.0            | 5V          | Full Rf operation 600mAh  Normal operation 80mAh |
| GPIO Power Output  | 3.3V        |                                                  |
| NB-IoT Active Mode | 3.3V        |                                                  |

## **Pinout**

<figure><img src="../../.gitbook/assets/image (4).png" alt="" width="375"><figcaption><p>Thinger32 NB-IoT Pinout</p></figcaption></figure>

## **Getting Started**

**1. Install the Required Software**

* **Arduino IDE** (with ESP32 board manager) or **Visual Studio Code** with **Platformio** extension.&#x20;
* **USB Driver for CP2102** (if needed)
* Install Thinger.io library on your project

**2. Connect the Board**

* Use a **USB 3.0 cable** for fast communication, then check the data transference by opening the Serial Monitor.&#x20;
* Insert a **microSIM card** for NB-IoT connectivity.

**3. Upload Your First Sketch**

* Use the **Arduino Framework** and Thinger.io libraries for easy programming.
* Configure **Wi-Fi or NB-IoT** connectivity in a few lines of code.

## **Example code**



````
```cpp

//#define THINGER_SERVER "upm.aws.thinger.io"
#define THINGER_SERVER "acme.thinger.io"
#define HEXACORE_DEVELOPMENT
#define THINGER_SERIAL_DEBUG
#define _DISABLE_TLS_
#include <Arduino.h>
#include <EEPROM.h>

// Set serial for debug console (to the Serial Monitor, default speed 115200)
#define SerialMon Serial
#define SerialAT Serial1

// Select your modem:
#define TINY_GSM_MODEM_BC660
#define TINY_GSM_DEBUG SerialMon
//#define DUMP_AT_COMMANDS
#ifndef TINY_GSM_RX_BUFFER
#define TINY_GSM_RX_BUFFER 1024
#endif

//set hibernation parameters
#define uS_TO_S_FACTOR 1000000ULL  /* Conversion factor for micro seconds to seconds */
#define TIME_TO_SLEEP  10        /* Time ESP32 will go to sleep (in seconds) */

// Can be installed from Library Manager or https://github.com/vshymanskyy/TinyGSM
#include <TinyGsmClient.h>
#include <ThingerTinyGSM.h>
#include <ThingerESP32OTA.h>
#include "arduino_secrets.h"

//Sensor libraries

#ifdef DUMP_AT_COMMANDS
#include <StreamDebugger.h>
StreamDebugger debugger(Serial1, Serial);
ThingerTinyGSM thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL, debugger);
#else
ThingerTinyGSM thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL, SerialAT);
#endif

ThingerESP32OTA ota(thing);
String iccid;

// module config version
#define MODULE_CONFIG_VERSION 3


   #define PIN_MODEM_RESET 13
   #define PIN_MODEM_WAKEUP 12
   #define PIN_MODEM_PWR_KEY 19
   #define PIN_MODULE_LED 21
   #define PIN_MODEM_RX 32
   #define PIN_MODEM_TX 33

   #define RELAY_PIN 27
   #define DOOR_STATE_PIN 14

String id2 = "My_device";


void setup() {
   pinMode(PIN_MODEM_RESET,   OUTPUT);
   pinMode(PIN_MODEM_WAKEUP,  OUTPUT);
   pinMode(PIN_MODEM_PWR_KEY, OUTPUT);

   Serial.begin(115200);
   Serial1.begin(115200, SERIAL_8N1, PIN_MODEM_RX, PIN_MODEM_TX);
   delay(400);

  sensors_begin();

   thing["my_data"] >> [](pson & out){
       out["millis"] = millis();
   };

   thing["modem"] >> [](pson & out){
      out["modem"] =    thing.getTinyGsm().getModemInfo().c_str();
      out["IMEI"] =     thing.getTinyGsm().getIMEI().c_str();
      out["CCID"] =     thing.getTinyGsm().getSimCCID().c_str();
      out["operator"] = thing.getTinyGsm().getOperator().c_str();
   };

   //  set APN
   thing.setAPN(APN_NAME, APN_USER, APN_PSWD);

   // configure hardware module reset
   thing.setModuleReset([]{
      // modem reset
      digitalWrite(PIN_MODEM_RESET, 1);
      delay(100);
      digitalWrite(PIN_MODEM_RESET, 0);
   });

   thing.initModem([](TinyGsm& modem){
      // read SIM ICCID
      iccid = modem.getSimCCID();
      THINGER_DEBUG_VALUE("NB-IOT", "SIM ICCID: ", iccid.c_str());

      // disable power save mode
      modem.sendAT("+CPSMS=0");
      modem.waitResponse();

      // disable eDRX
      modem.sendAT("+CEDRXS=0");
      modem.waitResponse();

      // edRX and PTW -> disabled
      modem.sendAT("+QEDRXCFG=0");
      modem.waitResponse();

      // initialize module configuration for the first time
      EEPROM.begin(1);
      auto state = EEPROM.readByte(0);
      if(state!=MODULE_CONFIG_VERSION){
         THINGER_DEBUG_VALUE("NB-IOT", "Configuring module with version: ", MODULE_CONFIG_VERSION);

         // stop modem functionality
         modem.sendAT("+CFUN=0");
         modem.waitResponse();

         // configure APN
         modem.sendAT("+QCGDEFCONT=\"IP\",\"" APN_NAME "\"");
         modem.waitResponse();

         // preferred search bands (for Spain)
         modem.sendAT("+QBAND=3,20,8,3");
         modem.waitResponse();

         // set preferred operators
         modem.sendAT("+COPS=4,2,\"21407\"");  // movistar
         //modem.sendAT("+COPS=4,2,\"21401\"");  // vodafone
         modem.waitResponse();

         // enable net led
         modem.sendAT("+QLEDMODE=1");
         modem.waitResponse();

         // full functionality
         modem.sendAT("+CFUN=1");
         modem.waitResponse();

         EEPROM.writeByte(0, MODULE_CONFIG_VERSION);
         EEPROM.commit();
      }else{
         THINGER_DEBUG_VALUE("NB-IOT", "Module already configured with version: ", MODULE_CONFIG_VERSION);
      }
      EEPROM.end();
   });

   // configure ota block size to 
   ota.set_block_size(512);
   id2+=iccid.c_str();
   thing.set_credentials(USERNAME, id2.c_str(), id2.c_str());

}

void loop() {
  // iotmp handle, modem pwr on
  thing.handle();
}

```
````

## Documentation

{% file src="../../.gitbook/assets/esp32-pico_series_datasheet_en.pdf" %}

{% file src="../../.gitbook/assets/Quectel_BC660K_GL_NB_IoT_Specification_V1_4_1-3009753.pdf" %}

{% file src="../../.gitbook/assets/Schematic Thinger32NB-IoT.pdf" %}

{% file src="../../.gitbook/assets/Thinger32NB-IoT Measurements.pdf" %}

## **Use Cases**

🔹 **Smart Cities & Remote Monitoring** – Low-power sensors with NB-IoT for real-time monitoring\
🔹 **Industrial IoT (IIoT)** – Secure cloud connectivity for machinery and logistics tracking\
🔹 **Wearables & Health Tech** – Battery-optimized applications with Wi-Fi + NB-IoT\
🔹 **Home Automation** – Wi-Fi and cellular connectivity for smart devices\
🔹 **Agriculture & Environment** – Remote sensing with ultra-low power requirements

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

