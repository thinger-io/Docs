# ESPRESSIF ESP32

## Introduction

ESP32 is a series of low-cost, low-power system on a chip microcontrollers with integrated Wi-Fi and dual-mode Bluetooth. There are multiple modules based on this microcontroller that includes different kinds of antennas, pinouts and memory extensions. It is the successor to the ESP8266 microcontroller and is designed to be one of the most relevant IoT impulsor during the next years and there is a great diversity of variants that exploit its capacities together with other peripherals, integrating LoRa communication, audio amplifiers, LCD screens, etc.

![ESP32 WROOM DEV MODULE](../.gitbook/assets/ESP32.png)

## Install On Arduino IDE

This device can be programmed directly from the Arduino IDE by including the ESP32 core libraries with Arduino Boards Manager. For this step, you will need first to include [https://dl.espressif.com/dl/package\_esp32\_index.json](https://dl.espressif.com/dl/package\_esp32\_index.json) into `Additional Board Manager URLs` field in the Arduino preferences.

![](../.gitbook/assets/ESP32\_Preferences.PNG)

Next, go to the Boards manager to install the ESP32 package. Search for the `esp32` and install the package **esp32 by Espressif Systems**

![](../.gitbook/assets/esp32\_boardsManager.PNG)

After this process you should be able to select this board on your Arduino IDE and start creating your IoT projects with Thinger.io.&#x20;

## ESP32 WiFi

The following example will allow connecting your device to the cloud platform in a few lines via WiFi interface. Just modify the `arduino_secrets.h` file with your own information.

{% tabs %}
{% tab title="ESP32.ino" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerESP32.h>
#include "arduino_secrets.h"

ThingerESP32 thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

void setup() {
  // open serial for debugging
  Serial.begin(115200);

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

## ESP32 Ethernet

{% tabs %}
{% tab title="ESP32Eth.ino" %}
```cpp
// It may be required to define this according to your specific board
// Example RMII LAN8720 (Olimex, etc.)
#ifndef ETH_PHY_TYPE
#define ETH_PHY_TYPE  ETH_PHY_LAN8720
#define ETH_PHY_ADDR  0
#define ETH_PHY_MDC   23
#define ETH_PHY_MDIO  18
#define ETH_PHY_POWER -1
#define ETH_CLK_MODE  ETH_CLOCK_GPIO0_IN
#endif

// enable debug output over serial
#define THINGER_SERIAL_DEBUG

// disable certificate validation
// #define THINGER_INSECURE_SSL

// define private server instance
// #define THINGER_SERVER "acme.aws.thinger.io"

#include <ThingerESP32Eth.h>
#include <ThingerESP32OTA.h>
#include "arduino_secrets.h"

// initialize thinger instance
ThingerESP32Eth thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

// Initialize ESP32OTA OTA
// use Thinger.io VSCode Studio extension + Platformio to upgrade the device remotely
ThingerESP32OTA ota(thing);

void setup() {

    // enable serial for debugging
    Serial.begin(115200);

    // example of fixed ip address (dhcp is used by default)
    //thing.set_address("192.168.1.55", "192.168.1.1", "255.255.255.0", "8.8.8.8", "8.8.4.4");

    // set desired hostname
    thing.set_hostname("ESP32Eth");

    // resource output example (i.e. reading a sensor value)
    thing["eth"] >> [](pson& out){
        out["hostname"] = ETH.getHostname();
        out["mac"] = ETH.macAddress();
        out["ip"] = ETH.localIP().toString();
        out["link"] = ETH.linkSpeed();
    };

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

## ESP32 WiFi WebConfig

It is possible to configure all parameters required for connection via a web Interface (captive portal). The device will create an access point where the user can connect to establish required information, like username, device identifier, credentials, and access point to connect.

{% tabs %}
{% tab title="ESP32WebConfig.ino" %}
```cpp
#define THINGER_SERIAL_DEBUG

// Requires WifiManager from Library Manager or https://github.com/tzapu/WiFiManager
#include <ThingerESP32WebConfig.h>

ThingerESP32WebConfig thing;

void setup() {
  // open serial for debugging
  Serial.begin(115200);

  pinMode(27, OUTPUT);

  // digital pin control example (i.e. turning on/off a light, a relay, configuring a parameter, etc)
  thing["relay"] << digitalPin(27);

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
3. Configure the wifi where the ESP32 will be connected, and your thinger.io device credentials
4. Your device should be now connected to the platform.

The WebConfig interface includes different methods to control the captive portal:

* **clean\_credentials**: It clean all credentials from the device (WiFi/user parameters). This way, the next time the device is booted will create the captive portal again to request the WiFi configuration. It can be executed after a long press on a button.&#x20;
* **set\_user**: Initializes the default user for connecting the device to the platform (if set, this parameter is not requested to the user in the captive portal).
* **set\_device**: Initializes the default device for connecting the device to the platform (if set, this parameter is not requested to the user in the captive portal).
* **set\_password**: Initializes the default device password for connecting the device to the platform (if set, this parameter is not requested to the user in the captive portal).
* **add\_setup\_parameter**: Add additional parameters to be requested in the captive portal, for example, any other configuration required for the execution: sampling intervals, meta-data, thresholds, etc.
* **set\_on\_config\_callback**: Set a callback to receive configuration provided by the user in the captive portal, i.e., user, device, password, or any additional parameter configured.
* **set\_on\_wifi\_config**: Set a callback to receive the result of the WiFi configuration. If the connection did not succeed, it can be used to clean credentials, so, the captive portal runs again.
* **set\_on\_captive\_portal\_run**: Set a callback to receive the WiFiManager instance before the captive portal is shown. It can be used to add any other customization over the WebConfig interface.

## ESP32 WiFi SmartConfig

Coming soon



