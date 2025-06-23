---
description: Thinger.io documentation for Arduino based on GSM connectivity
---

# ARDUINO GSM

## Introduction

Using Arduino with GSM connectivity is a great option for connecting the Arduino board wirelessly to the Internet in a few minutes. Connecting a device to a GSM network is simple, and it only requires a module with GSM connectivity and a SIM card. There are some boards with GSM connectivity on the Arduino ecosystem, like the MKR GSM 1400 that uses the 3G network for Internet connectivity, and the MKR NB 1500, using a narrowband solution, allowing the use of LTE Cat M1 or NB LTE-M.

In this documentation, we cover how to connect devices over GSM by using different approaches, like using Arduino MKR GSM 1400 or Arduino MKR NB 1500.

## Arduino MKR GSM 1400

The Arduino MKR GSM 1400 takes advantage of the cellular network as a means to communicate. The GSM / 3G network is the one that covers the highest percentage of the world's surface, making this connectivity option very attractive when no other connectivity options exist. Whether for building a gateway to a remote sensor network or for a single device sending a text message when an event occurs across the country, the MKR GSM 1400 assists in quickly implementing a solution to accommodate such needs.

The board's main processor is a low-power Arm® Cortex®-M0 32-bit SAMD21, like in the other boards within the Arduino MKR family. The GSM / 3G connectivity is performed with a module from u-blox, the SARA-U201, a low power chipset operating in the de different bands of the cellular range (GSM 850 MHz, E-GSM 1900 MHz, DCS 1800 MHz, PCS 1900 MHz). On top of those, secure communication is ensured through the Microchip® ECC508 crypto chip. Besides that, a battery charger and a connector for an external antenna can be found.

![Arduino MKR GSM 1400](<../.gitbook/assets/image (306).png>)

This example will allow connecting the Arduino GSM 1400 to the cloud platform in a few lines. The `arduino_secrets.h` file just needs to be modified with the relevant information.

{% tabs %}
{% tab title="ArduinoGSM1400.ino" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerMKRGSM.h>
#include "arduino_secrets.h"

ThingerMKRGSM thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

void setup() {
  // open serial for debugging
  Serial.begin(115200);

  // optional set pin number
  thing.set_pin(PIN_NUMBER);

  // set APN
  thing.set_apn(GPRS_APN, GPRS_LOGIN, GPRS_PASSWORD);

  // set builtin led to output
  pinMode(LED_BUILTIN, OUTPUT);

  // pin control example over the Internet (i.e. turning on/off a light, a relay, etc)
  thing["led"] << digitalPin(LED_BUILTIN);

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

#define PIN_NUMBER "your_pin"

#define GPRS_APN "your_apn_name"
#define GPRS_LOGIN "your_gprs_login"
#define GPRS_PASSWORD "your_gprs_password"
```
{% endtab %}
{% endtabs %}

## Arduino MKR NB 1500

Add Narrowband communication to the project with the MKR NB 1500. It's the perfect choice for devices in remote locations without an Internet connection, or in situations in which power isn't available, like on-field deployments, remote metering systems, solar-powered devices, or other extreme scenarios.

The board's main processor is a low-power Arm® Cortex®-M0 32-bit SAMD21, like in the other boards within the Arduino MKR family. The Narrowband connectivity is performed with a module from u-blox, the SARA-R410M-02B, a low power chipset operating in the de different bands of the IoT LTE cellular range. On top of those, secure communication is ensured through the Microchip® ECC508 crypto chip. Besides that, the PCB includes a battery charger and a connector for an external antenna.

This board is designed for global use, providing connectivity on LTE's Cat M1/NB1 bands 1, 2, 3, 4, 5, 8, 12, 13, 18, 19, 20, 25, 26, 28. Operators offering service in that part of the spectrum include: Vodafone, AT\&T, T-Mobile USA, Telstra, and Verizon, among others.

![Arduino MRK NB 1500](<../.gitbook/assets/image (278).png>)

This example will allow connecting the Arduino MKR NB 1500 to the cloud platform in a few lines. The `arduino_secrets.h` file just needs to be modified with the relevant information.

{% tabs %}
{% tab title="ArduinoMKRNB1500.ino" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerMKRNB.h>
#include "arduino_secrets.h"

ThingerMKRNB thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

void setup() {
  // enable serial for debugging
  Serial.begin(115200);

  // optional set pin number
  thing.set_pin(PIN_NUMBER);

  // set APN
  thing.set_apn(GPRS_APN, GPRS_LOGIN, GPRS_PASSWORD);

  // set builtin led to output
  pinMode(LED_BUILTIN, OUTPUT);

  // pin control example over the Iinternet (i.e. turning on/off a light, a relay, etc)
  thing["led"] << digitalPin(LED_BUILTIN);

  // resource output example (i.e. reading a sensor value, a variable, etc)
  thing["time"] >> [&](pson& out){
      out = thing.getNB().getTime();
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

#define PIN_NUMBER ""

#define GPRS_APN "your_apn_name"
#define GPRS_LOGIN "your_gprs_login"
#define GPRS_PASSWORD "your_gprs_password"
```
{% endtab %}
{% endtabs %}

