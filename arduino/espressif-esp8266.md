# ESPRESSIF ESP8266

## Introduction

The ESP8266 chip from Espressif is the new generation of low-cost WiFi chips after the TI CC3000/CC3200. This small chip not only integrates the whole WiFi features, but also a powerful programmable MCU. Depending on the board layout (ESP-01, ESP-03, ESP-07, ESP12, etc) it is attached to a programmable flash, ranging from 512K to 4M. This increases the available user code space, and make possible other cool features like a small file system, or OTA updates.

![ESP8266 Dev Module. Also known as NodeMCU.](../.gitbook/assets/nodemcu.png)

## Install On Arduino IDE

This device can be directly programmed from the Arduino IDE. You can follow the following steps if you did not programmed this boards with the Arduino IDE. The only requirement is to install the board via the Arduino Boards Manager.

> In the Arduino preferences, enter [http://arduino.esp8266.com/stable/package\_esp8266com\_index.json](http://arduino.esp8266.com/stable/package\_esp8266com\_index.json) in **Additional Boards Manager URLs**

![Arduino Preferences - Additional Boards Manager](https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/b9ef9df0c95c1bff0e9d7db258a355bb44374b06.png)

> Next, go to the Boards manager to install the ESP8266 package.** Tools** > **Boards** > **Board manager...** Then search and install the esp8266 package.

![](https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/efdec170e35cb296b895dd92b9868f8e0a9d3cd9.png)

> Now you can program almost any ESP8266 directly from the Arduino IDE. From the **Tools** > **Boards** you should see now the new ESP8266 boards installed. Select your board to be able to compile code for the ESP8266.

You can find additional information for the ESP8266 package in the [ESP8266 Github Repository](https://github.com/esp8266/Arduino). The easiest board to program is the NodeMCU, which does not require pressing Flash + Reset buttons for uploading the sketch. For other boards you will need to use a USB to Serial converter (3v3!) and flash the sketch by setting some GPIOs to GND. Please search in Google for this step if you are not sure how to make it for your board.&#x20;

## ESP8266 WiFi

The following example will allow connecting an ESP8266 device to the cloud platform in a few lines using the WiFi interface. Just modify the `arduino_secrets.h` file with your own information.

{% tabs %}
{% tab title="ESP8266.ino" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerESP8266.h>
#include "arduino_secrets.h"

ThingerESP8266 thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

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
{% endtab %}

{% tab title="arduino_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"
```
{% endtab %}
{% endtabs %}

## ESP8266 WiFi WebConfig

It is possible to configure all parameters required for connection via a web Interface. The device will create an access point where the user can connect to establish required information, like username, device identifier, credentials, and access point to connect.

{% tabs %}
{% tab title="ESP8266WebConfig.ino" %}
```cpp
#define THINGER_SERIAL_DEBUG

// Requires WifiManager from Library Manager or https://github.com/tzapu/WiFiManager
#include <ThingerWebConfig.h>

ThingerWebConfig thing;

void setup() {
  // open serial for debugging
  Serial.begin(115200);

  pinMode(LED_BUILTIN, OUTPUT);

  // digital pin control example (i.e. turning on/off a light, a relay, configuring a parameter, etc)
  thing["led"] << digitalPin(LED_BUILTIN);

  // resource output example (i.e. reading a sensor value)
  thing["millis"] >> outputValue(millis());
}

void loop() {
  thing.handle();
}
```
{% endtab %}
{% endtabs %}

Once this sketch is loaded in the device, it is possible to follow the next steps to connect it to the platform:

1. Connect to Thinger-Device WiFi with your computer or phone, using `thinger.io` as WiFi password
2. Wait for the configuration window, or navigate to [http://192.168.4.1](http://192.168.4.1) if it does not appear
3. Configure the wifi where the ESP8266 will be connected, and your thinger.io device credentials
4. Your device should be now connected to the platform.

## ESP8266 WiFi SmartConfig

The SmartConfig (or recently named [ESP-Touch](https://www.espressif.com/en/products/software/esp-touch/overview)) is a provisioning technology developed by TI to connect a new Wi-Fi device to a Wi-Fi network. It uses a mobile app to broadcast the network credentials from a smartphone, or a tablet, to an un-provisioned Wi-Fi device.

The advantage of this technology is that the device does not need to directly know SSID or password of an Access Point (AP). This information is provided using the smartphone. This is particularly important to headless device and systems, due to their lack of a user interface.

To use this Wi-Fi provisioning technology, just use an example like the following:

{% tabs %}
{% tab title="ESP8266SmartConfig.ino" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerSmartConfig.h>
#include "arduino_secrets.h"

ThingerSmartConfig thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

void setup() {
  // open serial for debugging
  Serial.begin(115200);

  pinMode(LED_BUILTIN, OUTPUT);

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
{% endtab %}

{% tab title="arduino_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"
```
{% endtab %}
{% endtabs %}

Once this sketch is loaded in the device, it is possible to setup WiFi credentials using example applications from Espressif. Just download and install ESP Touch application from Espressif for your mobile phone and follow the onscreen instructions. Reference applications can be downloaded from here: [https://www.espressif.com/en/products/software/esp-touch/resources](https://www.espressif.com/en/products/software/esp-touch/resources)
