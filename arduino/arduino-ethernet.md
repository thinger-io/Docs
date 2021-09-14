# ARDUINO ETHERNET

## Introduction

Using Arduino with Ethernet is a great option for connecting your Arduino board to the Internet in a few minutes. It provides a fastest and reliable connectivity to your IoT devices. There are Ethernet shields that can extend Arduino features, like the Arduino Ethernet Shield for standard Arduinos, or the Arduino MKR ETH Shield for MKR devices. There are also external modules that can be plugged to any microcontroller, like the ENC28J60 module. 

In this documentation we cover how to connect devices over Ethernet by using both approaches, the default Arduino Ethernet Shields, and the external ENC28J60 module.

## Arduino with Ethernet Shield

![Arduino Ethernet Shield](../.gitbook/assets/arduino-ethernet.png)

The following example will allow connecting your Arduino device with the Ethernet Shield to the cloud platform in a few lines. Just modify the `arduino_secrets.h` file with your own information.

{% tabs %}
{% tab title="ArduinoEthernet.ino" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerEthernet.h>
#include "arduino_secrets.h"

ThingerEthernet thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

void setup() {
  // open serial for debugging
  Serial.begin(115200);

  pinMode(2, OUTPUT);

  // pin control example (i.e. turning on/off a light, a relay, etc)
  thing["led"] << digitalPin(2);

  // resource output example (i.e. reading a sensor value, a variable, etc)
  thing["millis"] >> outputValue(millis());

  // more details at http://docs.thinger.io/arduino/
}

void loop() {
  thing.handle();
}
```
{% endtab %}

{% tab title="arduino\_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"
```
{% endtab %}
{% endtabs %}

## Arduino with ENC28J60

The ENC28J60 is a very cheap Ethernet controller that can be used with our Arduinos to extend its connectivity. The main advantage of this controller is that it is inexpensive, as you can find this module for a few dollars. The bad news is that all the TCP/IP stack, DNS features, and so on, must run in the microcontroller itself, so there is no enough space in stock Arduinos for building our program. This way, for integrating the thinger.io libraries in the sketch, it would be necessary to disable the DHCP protocol \(that uses UDP under the hood\), and assign a manual IP address. If this is OK for your project, or you have a compatible microcontroller with more resources \(like ESP8266, Teensy, STM32F, etc\), then this module can be a great option.

![ENC28J60 Ethernet Module](../.gitbook/assets/enc28j60.jpg)

There are some libraries for managing this boards, but we will use [UIPEthernet](https://github.com/ntruchsess/arduino_uip), as it provides an standard interface that is compatible with the stock Thinger libraries.

The following example will allow connecting a device using the ENC28J60 interface to the cloud platform in a few lines using the WiFi interface. Just modify the `arduino_secrets.h` file with your own information.

{% tabs %}
{% tab title="ArduinoENC28J60.ino" %}
```cpp
// Install UIPEthernet for ENC28J60
// https://github.com/UIPEthernet/UIPEthernet
#define THINGER_SERIAL_DEBUG
  
#include <ThingerENC28J60.h>
#include "arduino_secrets.h"

ThingerENC28J60 thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

void setup() {
  // open serial for debugging
  Serial.begin(115200);

  // ENC28J60 using fixed IP Address. DHCP is too big for the sketch.
  uint8_t mac[6] = {0x00, 0x01, 0x02, 0x03, 0x04, 0x05};
  Ethernet.begin(mac, IPAddress(192, 168, 1, 125));

  pinMode(LED_BUILTIN, OUTPUT);

  // pin control example (i.e. turning on/off a light, a relay, etc)
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

{% tab title="arduino\_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"
```
{% endtab %}
{% endtabs %}

