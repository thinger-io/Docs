# OTA PROGRAMMING

OTA \(Over The Air\) Programing is a method that allows to remotely send new firmware to IoT devices no matter where they are as it works with Thinger.io Server to deliver the new executable file through the Internet network.

This feature allow us to keep the devices updated with new security patches and code improvements in a fast and scalable way, it is an essential tool for the maintenance of IoT products that will prevent us from traveling to the location of the devices.

## Configure the IDE

Before working with this tool it is necessary to install and configure Visual Studio Code and the Platformio extension as explained on the SDK SETUP section of this documentation. Then Install [Thinger.io ](http://thinger.io/)VSCode extension directly from the Extension manager. Search for “[Thinger.io 25](http://thinger.io/)” or [Checkout on Microsoft Marketplace 6](https://marketplace.visualstudio.com/items?itemName=thinger-io.thinger-io).

![](../.gitbook/assets/image%20%28435%29.png)

This extension will manage the OTA processes with some interesting features such as: 

* Device switcher to select the target device for the update
* Device connection status
* Compatible with multiple PlatformIO configuration environments inside a Project
* Automatic build and upload in one click
* Support for compressed OTA images \(zlib on ESP32, and gzip on ESP8266\)
* MD5 Checksum verification for firmware binaries

### Configure the extension

Before running the OTA it is necessary to configure the extension by accessing the VScode Extenions Manager and selecting the Extensions Settings option.

![](../.gitbook/assets/image%20%28439%29.png)

* Thinger.io Host: Place here the URL of the Thinger.io Instance you are working with 
* Thinger.io Port: Specifies the connection port \(typically 443\)
* Thinger.io Secure: Allows enable / disable SSL/TLS connection 
* Thinger.io Token: Place here an access token created to manage the devices with the next privileges:
  * ListDevices
  * AccessDeviceResources
  * ReadDeviceStatistic

## Upload firmware via OTA

The OTA system requires a library to be introduced in the software client of the device. Currently the library for the ardunino framework is available for the following microcontrollers:

* Espressif ESP32
* Espressif ESP8266 & 8285

This library must be included in the sketch in the traditional way, however, once the firmware is loaded and the VSCode extension is set up, you should be able to find the device\_ID in the deployable devices list \(roket icon\) and pressing the `"play"` button the new firmware will be remotely updated as shown in the Gif below

![](../.gitbook/assets/image%20%28441%29.png)

### Example for ESP32

* Create or use an exsiting project on VSCode with Platformio for ESP32 boards
* Add latest [Arduino Library 6](https://github.com/thinger-io/Arduino-Library) from Github on your `platformio.ini` configuration, for example:

```text
[env:ota-test]
platform = espressif32
board = esp32dev
framework = arduino
monitor_speed = 115200
lib_deps = https://github.com/thinger-io/Arduino-Library.git
```

* Using the serial port, flash the following example for ESP32 \(with your device credentials and WiFi Config\) with PlatformIO upload command:

```text
#include <ThingerESP32.h>
#include <ThingerESP32OTA.h>

#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"

ThingerESP32 thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);
ThingerESP32OTA ota(thing);

void setup() {
  pinMode(16, OUTPUT);

  thing.add_wifi(SSID, SSID_PASSWORD);

  // digital pin control example (i.e. turning on/off a light, a relay, configuring a parameter, etc)
  thing["GPIO_16"] << digitalPin(16);

  // resource output example (i.e. reading a sensor value)
  thing["millis"] >> outputValue(millis());

  // more details at http://docs.thinger.io/arduino/
}

void loop() {
  thing.handle();
}
```

* Now the device is ready to receive new firmwares via OTA, by pressing the "Play" button on the bottom bar of VSCode.

### Example for ESP8266



* Create or use an exsiting project on VSCode with Platformio for ESP8266 boards
* Add latest [Arduino Library 6](https://github.com/thinger-io/Arduino-Library) from Github on your `platformio.ini` configuration, for example:

```text
[env:ota-test]
platform = espressif8266
board = esp8266dev
framework = arduino
monitor_speed = 115200
lib_deps = https://github.com/thinger-io/Arduino-Library.git
```

* Using the serial port, flash the following example for ESP32 \(with your device credentials and WiFi Config\) with PlatformIO upload command:

```text
#define THINGER_SERIAL_DEBUG

#include <ThingerESP8266.h>
#include <ThingerESP8266OTA.h>
#include "arduino_secrets.h"

ThingerESP8266 thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

// Initialize ESP8266 OTA
// use Thinger.io VSCode Studio extension + Platformio to upgrade the device remotelly
ThingerESP8266OTA ota(thing);

void setup() {
  // open serial for monitoring
  Serial.begin(115200);

  // set builtin led as output
  pinMode(LED_BUILTIN, OUTPUT);

  // add WiFi credentials
  thing.add_wifi(SSID, SSID_PASSWORD);

  // digital pin control example (i.e. turning on/off a light, a relay, configuring a parameter, etc)
  thing["led"] << digitalPin(LED_BUILTIN);

  // resource output example (i.e. reading a sensor value)
  thing["millis"] >> outputValue(millis());

  // more details at http://docs.thinger.io/arduino/
}

void loop() {
  thing.handle();
}
```

* Now the device is ready to receive new firmwares via OTA, by pressing the "Play" button on the bottom bar of VSCode.



