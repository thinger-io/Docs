# ARDUINO ETHERNET

## Introduction

Using Arduino with Ethernet is a great option for connecting the Arduino board to the Internet in a few minutes. It provides the fastest and reliable connectivity to the IoT devices. There are Ethernet shields that can extend Arduino features, like the Arduino Ethernet Shield for standard Arduinos, or the Arduino MKR ETH Shield for MKR devices. There are also external modules that can be plugged into any microcontroller, like the ENC28J60 module.&#x20;

In this documentation, we cover how to connect devices over Ethernet by using both approaches, the default Arduino Ethernet Shields, and the external ENC28J60 module.

## Arduino Portenta H7 Ethernet

Portenta H7 simultaneously runs high-level code along with real-time tasks. The design includes two processors that can run tasks in parallel. For example, it is possible to execute Arduino compiled code along with MicroPython code, and have both cores communicate with one another. The Portenta functionality is two-fold, it can either be running like any other embedded microcontroller board or as the main processor of an embedded computer.&#x20;

H7's main processor is the dual-core STM32H747, including a Cortex® M7 running at 480 MHz and a Cortex® M4 running at 240 MHz. The two cores communicate via a _Remote Procedure Call_ mechanism that allows calling functions on the other processor seamlessly.

![Arduino Portenta H7](<../.gitbook/assets/image (313).png>)

{% tabs %}
{% tab title="ArduinoPortentaH7Eth.ino" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerMbedEth.h>
#include <ThingerPortentaOTA.h>
#include "arduino_secrets.h"

ThingerMbedEth thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);
ThingerPortentaOTA ota(thing);

void setup() {
    // configure LED_BUILTIN for output
    pinMode(LED_BUILTIN, OUTPUT);

    // open serial for debugging
    Serial.begin(115200);

    // pin control example (i.e. turning on/off a light, a relay, etc)
    thing["led"] << digitalPin(LED_BUILTIN);

    // resource output example (i.e. reading a sensor value, a variable, etc)
    thing["millis"] >> outputValue(millis());

    // start thinger on its own task
    thing.start();

    // more details at http://docs.thinger.io/arduino/
}

void loop() {
    // use loop as in normal Arduino Sketch
    // use thing.lock() thing.unlock() when using/modifying variables exposed on thinger resources
    delay(1000);
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

{% hint style="success" %}
In case of problems when connecting over secure TLS connections, try updating the WiFi firmware by flashing the WiFiFirmwareUpdater example sketch.

<img src="../.gitbook/assets/image (623) (1).png" alt="" data-size="original">
{% endhint %}

## Arduino Opta Ethernet

The Arduino Opta is designed for industrial automation, offering robust performance and reliability. It features a dual-core STM32H747 microcontroller, which includes a Cortex® M7 running at 480 MHz and a Cortex® M4 running at 240 MHz. This configuration enables Opta to handle complex real-time tasks and high-level code execution concurrently.

With its versatile architecture, the Opta supports running Arduino sketches alongside MicroPython, allowing developers to leverage the strengths of both programming environments. The dual-core setup facilitates inter-core communication via Remote Procedure Call, ensuring smooth and efficient coordination between the two processors. This capability makes the Arduino Opta ideal for advanced automation systems, where precise control and rapid response are crucial.

Additionally, the Arduino Opta is equipped with industrial-grade features, such as enhanced I/O capabilities and robust connectivity options. It can be seamlessly integrated into existing industrial networks, providing a reliable solution for monitoring and control applications. Whether used as a standalone microcontroller or as part of a larger embedded system, the Opta's performance and versatility make it a valuable asset in any industrial setting.

<figure><img src="../.gitbook/assets/image (630).png" alt=""><figcaption><p>Arduino Opta Wifi</p></figcaption></figure>

{% tabs %}
{% tab title="ArduinoOptaEth.ino" %}
```cpp
// enable debug output over serial
#define THINGER_SERIAL_DEBUG

// define private server instance
#define THINGER_SERVER "acme.aws.thinger.io"

#include <ThingerMbedEth.h>
#include <ThingerPortentaOTA.h>
#include "arduino_secrets.h"

ThingerMbedEth thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);
ThingerPortentaOTA ota(thing);

void setup() {
    // open serial for debugging
    Serial.begin(115200);

    // configure leds for output
    pinMode(LED_D0, OUTPUT);
    pinMode(LED_D1, OUTPUT);
    pinMode(LED_D2, OUTPUT);
    pinMode(LED_D3, OUTPUT);
    pinMode(LEDR, OUTPUT);
    pinMode(LED_BUILTIN, OUTPUT);

    // configure relays for output
    pinMode(D0, OUTPUT);
    pinMode(D1, OUTPUT);
    pinMode(D2, OUTPUT);
    pinMode(D3, OUTPUT);

    // example for controlling relays and status LED
    thing["relay_d0"] << [](pson& in){
        if(in.is_empty()){
            in = (bool) digitalRead(D0);
        }else{
            digitalWrite(D0, in ? HIGH : LOW);
            digitalWrite(LED_D0, in ? HIGH : LOW);
        }
    };

    thing["relay_d1"] << [](pson& in){
        if(in.is_empty()){
            in = (bool) digitalRead(D1);
        }else{
            digitalWrite(D1, in ? HIGH : LOW);
            digitalWrite(LED_D1, in ? HIGH : LOW);
        }
    };

    thing["relay_d2"] << [](pson& in){
        if(in.is_empty()){
            in = (bool) digitalRead(D2);
        }else{
            digitalWrite(D2, in ? HIGH : LOW);
            digitalWrite(LED_D2, in ? HIGH : LOW);
        }
    };

    thing["relay_d3"] << [](pson& in){
        if(in.is_empty()){
            in = (bool) digitalRead(D3);
        }else{
            digitalWrite(D3, in ? HIGH : LOW);
            digitalWrite(LED_D3, in ? HIGH : LOW);
        }
    };

    // example for controlling the LED
    thing["led"] << digitalPin(LED_BUILTIN);
    thing["led_r"] << digitalPin(LEDR);

    // resource output example (i.e. reading a sensor value, a variable, etc)
    thing["millis"] >> outputValue(millis());

    // start thinger on its own task
    thing.start();

    // more details at http://docs.thinger.io/arduino/
}

void loop() {
    // use loop as in normal Arduino Sketch
    // use thing.lock() thing.unlock() when using/modifying variables exposed on thinger resources
    delay(1000);
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

{% hint style="success" %}
In case of problems when connecting over secure TLS connections, try updating the WiFi firmware by flashing the WiFiFirmwareUpdater example sketch.

<img src="../.gitbook/assets/image (623) (1).png" alt="" data-size="original">
{% endhint %}

## Arduino with Ethernet Shield

![Arduino Ethernet Shield](../.gitbook/assets/arduino-ethernet.png)

This example will allow connecting the Arduino device with the Ethernet Shield to the cloud platform in a few lines. The `arduino_secrets.h` file just needs to be modified with the relevant information.

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

{% tab title="arduino_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"
```
{% endtab %}
{% endtabs %}

## Arduino with ENC28J60

The ENC28J60 is a very cheap Ethernet controller that can be used with our Arduinos to extend their connectivity. The main advantage of this controller is that it is inexpensive, as this module costs a few dollars. The bad news is that all the TCP/IP stack, DNS features, and so on, must run in the microcontroller itself, so there is not enough space in stock Arduinos for building our program. This way, for integrating the thinger.io libraries in the sketch, it would be necessary to disable the DHCP protocol (which uses UDP under the hood) and assign a manual IP address. If this is suitable for a project, or if a compatible microcontroller with more resources (such as ESP8266, Teensy, STM32F, etc.) is available, then this module can be a great option.

![ENC28J60 Ethernet Module](../.gitbook/assets/ENC28J60.jpg)

There are some libraries for managing these boards, but we will use [UIPEthernet](https://github.com/ntruchsess/arduino_uip), as it provides a standard interface that is compatible with the stock Thinger libraries.

This example will allow connecting a device using the ENC28J60 interface to the cloud platform in a few lines using the WiFi interface. The `arduino_secrets.h` file just needs to be modified with the relevant information.

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

{% tab title="arduino_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"
```
{% endtab %}
{% endtabs %}
