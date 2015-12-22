Overview
========

This documentation is related with the Arduino client library version of the Thinger.io platform. With this library you will be able to connect almost any Arduino board using Ethernet, Wifi, GSM, or other supported boards like ESP8266, NodeMCU, and TI CC3200.

The client library allows connecting your IoT devices to the [Thinger.io](http://thinger.io "thinger.io IoT Cloud Platform") cloud platform. This is a library specifically designed for the Arduino IDE, so you can easily program your devices and connect them within minutes.

It supports multiple network interfaces and boards, like Ethernet Shield, Wifi Shield, and GSM. It also supports other boards like ESP8266 (or NodeMCU), Texas Instruments CC3200 Launchpad, and Adafruit CC3000 board. It requires a modern Arduino IDE version, starting at 1.6.3.

Installation
============

The first step to start building thinger.io devices is to install the required libraries in the Arduino IDE to support exposing device resources like sensor values, lights, relays, and so on.

If you do not have the Arduino IDE installed yet, then it is a good moment to start, here are also some advices to choose the right version.

## Arduino IDE 

It is required a modern version of Arduino supporting `Library Manager` and some other features. Please install a version starting form **1.6.3** from the official Arduino download page. This step is not required if you already have a modern version.

[Download Arduino IDE >](https://www.arduino.cc/en/Main/Software)

There are two ways of installing the library. The preferred way is by using the Arduino `Library Manager`, which simplifies searching and installing new libraries. It also supports updating libraries when new versions are released. So use this method when possible. The other way to install the library is by using the traditional method of download and import the `zip` library. 

## Library Manager

The most easy way to install new libraries is by using the `Library Manager` available in the Arduino IDE. For installing the thinger.io library please follow the following steps:

Open the **Library Manager**

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/30a5f56c8917f8a26b03efb2438bfa444d531b2f.png" width="100%"> 

> Open the **Library Manager** in the Arduino menu in `Sketch` >
    `Include Library` > `Manage Libraries`

**Search** and install the thinger.io library

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/0e8bc7c86b5aff26aea7649741b592c8157cae11.png" width="100%">  

> Search the library with name **thinger.io** and then click `Install`. You can update the library also from this manager when it is updated.

Now the library should be be available with some default examples.

## Manual Import

If the preferred way using the Library Manager is not working or you prefer to manage the libraries yourself, you can also install the library by the traditional way. 

**Download** the latest version from the Github repository by clicking in the link below. This will download a file called `Arduino-Library-master.zip`.

[Download Library >](https://github.com/thinger-io/Arduino-Library/archive/master.zip)

Now **rename** the `Arduino-Library-master.zip` file to something more relevant like `thinger.zip`. 

The final step is to **import** the `zip` library using the Arduino IDE. This step will uncompress and copy the `zip` library in the Arduino libraries folder. Which is usually under your Documents folder.

<p align="center">
<img src="assets/add-zip-library.png" width="100%">
</p>

> Sketch > Include Library > Add .ZIP libraries

Now the library should be be available with some default examples.

Supported Hardware
==================

The thinger.io platform is designed to support almost any microcontroller or device with communication capabilities. No matter if it has Ethernet, Wifi, GSM, or the chip is from some vendor or not. Almost any device can be integrated in the cloud. So you can choose the hardware you want to connect, as this platform does not force you to purchase some compatible vendor hardware. This is a crucial when designing your IoT projects. Here you are free to choose the hardware you want!

In the following sections there are some of the devices that are compatible trough the Arduino IDE. For other device like Raspberry Pi, Intel Edison, BeagleBone Black, or any other device running a Linux distribution, please refer to the Linux Documentation.

## Arduino + Ethernet

The Arduino Ethernet Shield connects your Arduino to the internet in mere minutes. Just plug this module onto your Arduino board, connect it to your network with an RJ45 cable, and you are almost done to start controlling your world through the internet.

The following example will allow connecting your device to the cloud platform in a few lines. Just replace the sketch **username**, **deviceId**, and **deviceCredential** with your own credentials.

<p align="center">
<img src="assets/arduino-ethernet.png" width="300px">
</p>

``` cpp
#include <SPI.h>
#include <Ethernet.h>
#include <ThingerEthernet.h>

ThingerEthernet thing("username", "deviceId", "deviceCredential");

void setup() {
}

void loop() {
  thing.handle();
}
```

Want to add some device resources (led, sensors, etc.) to interact with them from the Internet?. Check the [Add Resources](#coding-adding-resources) section.

## Arduino + Wifi

The Arduino Wifi Shield is a poweful IoT shield that connects your Arduino board to the internet wirelessly. Connecting it to a WiFi network is simple, no further configuration in addition to the SSID and the password are required. The WiFi Shield comes with an easy-to-use library that allows to connect your Arduino board to the internet with few instructions. This is also applied to the Thinger client, so you can connect your Arduino + Wifi Shield to the platform in a few lines of code.

The following example will allow connecting your device to the cloud platform in a few lines. Just replace the sketch **username**, **deviceId**, and **deviceCredential** with your own credentials, and the **wifi_ssid**, **wifi_password** with the WiFi credentials. 

<p align="center">
<img src="assets/arduino-wifi-shield.png" width="300px">
</p>


``` cpp
#include <SPI.h>
#include <WiFi.h>
#include <ThingerWifi.h>

ThingerWifi thing("username", "deviceId", "deviceCredential");

void setup() {
    thing.add_wifi("wifi_ssid", "wifi_password");
}

void loop() {
  thing.handle();
}
```

Want to add some device resources (led, sensors, etc.) to interact with them from the Internet?. Check the [Add Resources](#coding-adding-resources) section.


## Arduino + CC3000

The CC3000 chip from Texas Instruments was one of the first low-cost WiFi chips that revolutionized the IoT maker ecosystem. In contrary to the other available WiFi alternatives, like the WiFi shield, the CC3000 appeared at a low cost (about 10$) for their time. It is a powerful chip as it integrates the whole TCP/IP stack and many other protocols. Some vendors like Adadruit started to build modules and libraries for integrating this chip with the Arduino ecosystem. Thanks to the libraries provided by Adafruit is then possible to build connected device with a few lines of code.

So for this module is required to have installed the **Adafruit CC3000 Libraries**, as they are directly used by the thinger client. You can download it here.

[Adafruit CC3000 Libraries >](https://github.com/adafruit/Adafruit_CC3000_Library/archive/master.zip)

The following example will allow connecting your device to the cloud platform in a few lines. Just replace the sketch **username**, **deviceId**, and **deviceCredential** with your own credentials, and the **wifi_ssid**, **wifi_password** with the WiFi credentials.

<p align="center">
<img src="assets/adafruit-cc3000.png" width="250px">
</p>

``` cpp
#include <Adafruit_CC3000.h>
#include <SPI.h>
#include <ccspi.h>
#include <ThingerCC3000.h>

ThingerCC3000 thing("username", "deviceId", "deviceCredential");

void setup() {
    thing.add_wifi("wifi_ssid", "wifi_password");
}

void loop() {
  thing.handle();
}
```

Want to add some device resources (led, sensors, etc.) to interact with them from the Internet?. Check the [Add Resources](#coding-adding-resources) section.

## Arduino Yun

The Arduino Yún is a microcontroller board based on the ATmega32u4 and the Atheros AR9331. The Atheros processor supports a Linux distribution based on OpenWrt named OpenWrt-Yun. The board has built-in Ethernet and WiFi support, a USB-A port, micro-SD card slot, 20 digital input/output pins (of which 7 can be used as PWM outputs and 12 as analog inputs), a 16 MHz crystal oscillator, a micro USB connection, an ICSP header, and 3 reset buttons. This board let the programmable ATmega32u4 communicate with Internet by using the Bridge Library that expose some functions running in the Linux distribution.

The following example will allow connecting the Yun to the cloud platform in a few lines. Just replace the sketch **username**, **deviceId**, and **deviceCredential** with your own credentials. Notice that it is not required to configure any network parameter in the code, as this managed by the running Linux distribution. However you many need to connect with your Arduino Yun via WiFi to connect it some local network.

<p align="center">
<img src="assets/arduino-yun.png" width="300px">
</p>

``` cpp
#include <YunClient.h>
#include <ThingerYun.h>

ThingerYun thing("username", "deviceId", "deviceCredential");

void setup() {
    Bridge.begin();
}

void loop() {
    thing.handle();
}
```

Want to add some device resources (led, sensors, etc.) to interact with them from the Internet?. Check the [Add Resources](#coding-adding-resources) section.

## ESP8266 / NodeMCU

The ESP8266 chip from Espressif was the new generation of low-cost WiFi chips after the TI CC3000/CC3200. This small chip not only integrates the whole WiFi features, but also a powerful programmable MCU. Depending on the board layout (ESP-01, ESP-03, ESP-07, ESP12, etc) it is attached to a programmable flash, ranging from 512K to 4M. This increases the available user code space, and make possible other cool features like a small file system, or OTA updates.

This devices can be directly programmed from the Arduino IDE. You can follow the following steps if you did not programmed this boards with the Arduino IDE. The only requirement is to install the board via the Arduino Boards Manager.

For this step, just put http://arduino.esp8266.com/stable/package_esp8266com_index.json into **Additional Board Manager URLs** field in the **Arduino v1.6.4+** preferences. If this URL is not working, maybe you may need to check the Github project that supports the library: [ESP8266 Github](https://github.com/esp8266/Arduino).

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/b9ef9df0c95c1bff0e9d7db258a355bb44374b06.png" width="100%"> 

> In the Arduino preferences, enter http://arduino.esp8266.com/stable/package_esp8266com_index.json in **Additional Boards Manager URLs**

Next, go to the Boards manager to install the ESP8266 package. Search for the esp8266 and install the package **esp8266 by ESP8266 Community**

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/efdec170e35cb296b895dd92b9868f8e0a9d3cd9.png" width="100%"> 

> **Tools** > **Boards** > **Board manager...** Then search and install the esp8266 package.

Now you can program almost any ESP8266 directly from the Arduino IDE. From the **Tools** > **Boards** you should see now the new ESP8266 boards installed. Select your board to be able to compile code for the ESP8266.

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/4db23ae2b7121cbf6702f5d55c3c931de6be5f33.png" width="250px"> 

> Select the ESP8266 based board you will program from **Tools** > **Boards**

You can find additional information for the ESP8266 package in the [ESP8266 Github Repository](https://github.com/esp8266/Arduino). The easiest board to program is the Node MCU, which does not require pressing Flash + Reset buttons for uploading the sketch. For other boards you will need to use a USB to Serial converter (3v3!) and flash the sketch by setting some GPIOs to GND. Please search in Google for this step if you are not sure how to make it for your board. For our example we will be using the NodeMCU, that already converts the 5v from USB to 3v3, and provides the USB to Serial embedded in the board.

The following example will allow connecting your device to the cloud platform in a few lines. Just replace the sketch **username**, **deviceId**, and **deviceCredential** with your own credentials, and the **wifi_ssid**, **wifi_password** with the WiFi credentials.

<p align="center">
<img src="assets/nodemcu.png" width="325px">
</p>

``` cpp
#include <SPI.h>
#include <ESP8266WiFi.h>
#include <ThingerWifi.h>

ThingerWifi thing("username", "deviceId", "deviceCredential");

void setup() {
  thing.add_wifi("wifi_ssid", "wifi_credentials");
}

void loop() {
  thing.handle();
}
```

Want to add some device resources (led, sensors, etc.) to interact with them from the Internet?. Check the [Add Resources](#coding-adding-resources) section.

## TI Launchpad CC3200

The TI CC3200 was the natural evolution of the CC3000/CC3100 chip. Instead on providing a single chip for managing the WiFi communications, it also integrates a powerful programmable MCU, in the same way the ESP8266 is doing. So you can program your code and have WiFi capabilities right out of the box. The easiest way to start with this chip is by using the TI CC3200 Launchpad, which integrates the chip, as well as some sensors, leds, and the USB to serial so you can program the board right from the USB.

To program this board it is possible to use an Arduino-based IDE that is called [Energia](http://energia.nu/download/). So, download and install it before continue. Checkout also the required [instructions](http://energia.nu/pin-maps/guide_cc3200launchpad/) for programming the CC3200, as you need to make a short between two pins.

Once the environment is available and you can program the board examples, then you should install the Thinger Arduino Client Libraries also in the Energia IDE. Check the [Manual Import](#installation-manual-import) for reference.

The following example will allow connecting your device to the cloud platform in a few lines. Just replace the sketch **username**, **deviceId**, and **deviceCredential** with your own credentials, and the **wifi_ssid**, **wifi_password** with the WiFi credentials.

<p align="center">
<img src="assets/ti-cc3200.png" width="325px">
</p>

``` cpp
#include <WiFi.h>
#include <ThingerWifi.h>

ThingerWifi thing("username", "deviceId", "deviceCredential");

void setup() {
    thing.add_wifi("wifi_ssid", "wifi_password");
}

void loop() {
  thing.handle();
}
```

Want to add some device resources (led, sensors, etc.) to interact with them from the Internet?, check the [Add Resources](#coding-adding-resources) section.

Coding
======

This section will cover how to add different functionality to your devices for exposing resources, calling endpoints, or streaming data to real-time websockets.

## Sketch Overview

Almost all Arduino Sketches looks the same. There is a setup method, and there is a loop method. Nothing changes here while integrating with Thinger.io. However you must know where you should define your device resources, or where it is possible to interact with external services. In general terms, any device resource (led, relay, sensor, servo, etc.) must be defined inside the `setup()` method. As well as you initialize your devices, set the input/output direction of a digital pin, or initialize the Serial port speed, you also need to initialize here your resources. This basically consists on configuring what values or resources you want to expose over the Internet.<br/><br/>The `loop()` is the place to always call to the `thing.handle()` method, so the thinger libraries can handle the connection with the platform. This is the place also for calling your endpoints, or streaming real-time data to an open WebSocket. Please, take into account to **do not add any delay inside the `loop()`** except if you know what you are doing, like working with deep sleep modes or so in your device. Any other delay will condition the proper functioning of Thinger in your device. Also it can be bad to read a sensor value in every loop if the sensor takes too much time to complete a read. This will result in a device with a noticeable lag while attending to our commands. 

``` cpp
// add required headers according to your device
#include <SPI.h>
#include <Ethernet.h>
#include <ThingerEthernet.h>

// initialize Thinger instance (type can change depending on your device)
ThingerEthernet thing("username", "deviceId", "deviceCredential");

void setup() {
    // initialize your sensors and pins
    
    // initialize wifi (see examples for your device)

    // add resources here, like sensors, lights, etc.
}

void loop() {
  // call always the thing handle in the loop and avoid any delay here
  thing.handle(); 
  // here you can call endpoints
  // and also you can stream resources
}

```

You can easily start with some available example for your device after you install the client libraries.

<p align="center">
<img src="assets/arduino-examples.png" width="325">
</p>

> It is recommended to start with some of the examples available in the Arduino IDE when you install the librarires

## Setting Credentials

All the devices connected to the platform needs to be authenticated against the server. When you create a [device in the console](https://link_to_console) you are basically creating a new device identifier and setting a device credential. Therefore, you need to setup this credentials also in your Arduino code so the device can be recognized and associated to your account. This is normally done while initializing the Thinger instance in the code. That is, when you define the `thing` instance. Replace here your `username`, `deviceId`, and `deviceCredential` with the values you have registered in the cloud.

```cpp
ThingerWifi thing("username", "deviceId", "deviceCredential");
```
## Adding Resources

In the Thinger.io platform, each device can define several resources. You can think that a resource is anything you can sense or actuate. For example, a typical resource will be a sensor value like temperature or humidity, or a relay that turns on and off a light. This way, you should define the resources you need to expose over the Internet.

All resources must be defined inside the `setup()` method of the Arduino sketch. This way the resources are configured at the beginning, but can be accessed later as necessary.

There are three different types of resources, which are explained in the following sections.

### Input Resources

If you need to control or actuate your IoT device, it is necessary to define an input resource. In this way, an input resource is anything that can provide information to your device. For example, it can be a resource for turning on and off a light or a relay, change a servo position, adjust a device parameter, etc.

To define an input resource it is used the operator `<<` pointing to the resource name, and it uses a C++11 Lambda function to define the function.

The input resource function takes one parameter of type `pson` that is a variable type that can contain booleans, numbers, floats, strings, or even structured information like in a JSON document.
 
The following subsections will show how to define different input resources for typical use cases.

*Turn on/off a led, a relay, etc*

This kind of resources only requires an on/off state so it can be enabled or disabled as required. As the `pson` type han hold multiple data types, we can think that the `pson` parameter of the input function is like a boolean. 
 
So, inside the `setup` function you can place a resource called `led` (but you can use any other name), of input type (using the operator `<<`), that takes a reference to a `pson` parameter. This example will turn on/off the digital pin 10 using a ternary operator over the `in` parameter.

``` cpp
thing["led"] << [](pson& in){
  digitalWrite(10, in ? HIGH : LOW);
};
```

*Modify a servo position*

Modifying a servo position is quite similar to turning on/off a led. In this case, however, it is necessary to use an integer value. As the `pson` type can hold multiple data types, we can still use the `pson` type as an integer value. 

``` cpp
thing["servo"] << [](pson& in){
    myServo.write(in);
};
```

*Update sketch variables*

You can use the input resources also for updating your sketch variables, so you can change your device behaviour dynamically. This is quite useful in some situations where you want to temporary disable an alarm, change the reporting intervals, update an hysteresis value, and so on. In this way, you can define additional resources to change your variables.  

``` cpp
float hysteresis = 0; // defined as a global variable
thing["hysteresis"] << [](pson& in){
    hysteresis = in;
};
```

*Pass multiple data*

The `pson` data type can hold not only different data types, but also is fully compatible with JSON documents. So you can use the pson data type to receive multiple values at the same time. This example will receive two different floats that are stored with the `lat` and `lon` keys.

``` cpp
thing["location"] << [](pson& in){
    float lat = in["lat"];
    float lon = in["lon"];
};
```


### Output Resources

Output resources should be used in general when you need to sense or read a sensor value, like temperature, humidity, etc. So the output resources are quite useful for extracting information from the device. 

To define an output resource it is used the operator `>>` pointing out of the resource name, and it uses a C++11 Lambda function to define the output function.

The output resource function takes one parameter of `pson` type that is a variable type that can contain booleans, numbers, floats, strings, or even structured information like in a JSON document.
 
The following subsections will show how to define different output resources for typical use cases.

*Read a sensor value*

Defining an output resource is quite similar to defining an input resource, but in this case it is used the operator `>>`. In the callback function we can fill the out value with any value we want, like in this case the output from a sensor reading. 

``` cpp
thing["temperature"] >> [](pson& out){
      out = dht.readTemperature();
};
```

*Read multiple data*

In the same way the input resources can receive multiple values at the same time, the output resources can also provide multiple data. This is an example for providing both latitude and longitude from a GPS.

``` cpp
thing["location"] >> [](pson& out){
      out["lat"] = gps.getLatitude();
      out["lon"] = gps.getLongitude();
};
```

*Read sketch variables*

If your sketch cannot provide a single sensor reading, as it is doing some kind of data integration, an output resource can be used also for reading your sketch variables, where the computed result is updated frequently.

``` cpp
float yaw = 0; // defined as a global variable
thing["yaw"] >> [](pson& out){
      out = yaw;
};
```

### Input/Output Resources

The last resource type is a resource that not only takes an input or an output, but takes both parameters. This is quite useful when you want to read an output that depends on a input, i.e., when you need to provide a changing reference value to a sensor.

This kind of resources are defined with the operator `=`. In this case the function takes two different `pson` parameters. One for input data and another one for output data. This example provides an altitude reading using the BMP180 Sensor. It takes the reference altitude as input, and provides the current altitude as output.

``` cpp
thing["altitude"] = [](pson& in, pson& out){
    out = bmp.readAltitude(in);
};
```
  
You can also define more complex input/output resources, that takes several input values, to provide also multiple output values, like in this example that takes `value1` and `value2` to provide the `sum` and `mult` values.

``` cpp
thing["in_out"] = [](pson& in, pson& out){
      out["sum"] = (long)in["value1"] + (long)in["value2"];
      out["mult"] = (long)in["value1"] * (long)in["value2"];
};
```

## Using Endpoints

In Thinger.io, an endpoint is defined as some kind of external resource that can be accessed by the device. With the endpoints feature, devices can easily send emails, SMS, push data to external WebServices, interact with IFTTT, and any general action that can be made by using WebHooks (Calling HTTP/HTTPS URLs).

Calling an endpoint is so easy from the Arduino sketch, as it is only required to call the `call_endpoint` method over the `thing` variable.

```cpp
thing.call_endpoint("endpoint_id");
```

You can simply call an endpoint to make some action like sending a predefined email, or also call the endpoint with some data, which is specially useful when you are using third party services that consume your devices data.

**You should take extra attention while calling resources, and call them at an appropriate rate. Otherwise you can consume easily your available data, receive hundred of emails, or consume your API calls in third-party services.**

### Calling Endpoints

In this case we will see a simple example to send an email alert based on a temperature value. For this example, we have configured an email endpoint called `high_temp_email` that contains some warning text about the temperature. For this case we do not want to check the temperature every millisecond, so we are introducing some variables to control the sensing and warning frequency. In this example, the temperature is checked every hour, and if it is above 30ºC, it will call the endpoint called `high_temp_email` which will send us an email with the predefined text. It is important here to **do not add delays** inside the loop method, as it will prevent the required execution of the `thing.handle()` method, so we are using here a non-blocking delay based on the `millis()` function.

```cpp
unsigned long lastCheck = 0;
loop(){
    thing.handle(); // required thing handle
    unsigned long currentTs = millis();
    if(currentTs-lastCheck>=60*60*1000){
        lastCheck = currentTs;
        if(dht.readTemperature()>30){
            thing.call_endpoint("high_temp_email");
        }
    }
}
```

You can be so creative here and call your endpoints when the presence sensor makes a detection, when your humidity sensor reports that there is no water in your plants, when the location of a device is not as expected, and many other stuff. Other interesting way of using endpoints is by its integration with IFTTT, so you can interact with multiple third-party services!

### Sending Data to Endpoints

Sending data to an endpoint (in JSON format) is also quite easy. We need to call also the `call_endpoint` method, but in this case adding some information based on the `pson` data format, which will be automatically converted to JSON. For example, if we want to report data to a third party service like Keen.io, we can create such kind of endpoints in the console. Once configured, we can call the endpoint with our readings, for example with humidity and temperature values from a DHT sensor.

```cpp
// be careful of sending data at an appropriate rate!
pson data;
data["temperature"] = dht.readTemperature();
data["humidity"] = dht.readHumidity();
thing.call_endpoint("keen_endpoint", data);
```

## Streaming Resources

In Thinger.io you can open WebSockets connections against your devices, so you can receive sensor values, events, or any other information in real-time. The WebSockets are mainly used in the Dashboard feature of the Console, and are normally used for streaming resources at a fixed configurable interval. This functionality is available right out of the box when you define an output resource. However, if you want to transmit the information right when it is required, like when your device detects a movement, presence, etc., you must program some code, that is quite similar to calling an endpoint.

In this case, you must detect when you want to stream the event, like the accelerometer value is over some threshold, your presence sensor is making a detection, or the compass heading is changing. This is up to you when it is necessary to stream new data. Streaming resources also requires that another endpoint is connected listening for them (i.e., from a WebSocket connection), so if there is no one listening for this data, the data is not sent. This is handled automatically by the client library and the server, therefore it is safe to stream data always, as the device will transmit the information only when there is a destination. 

The following example will report the compass heading in real-time if the heading value changes more than 1 degree.

<p align="center">
<img src="assets/esp8266-real-time-websockets.gif" width="100%">
</p>

```cpp
void setup(){
  thing["heading"] >> [](pson& out){     
    out = getHeading();
  };
}

float previousHeading = 0;
void loop() {
  thing.handle();
  float currentHeading = getHeading();
  if(abs(currentHeading-previousHeading)>=1.0f){
    thing.stream(thing["heading"]);
    previousHeading=currentHeading;
  }
}
```

Interacting
===========

It is possible to easily interact with your devices within minutes once you have defined your resources and the device is connected to the platform. There are several ways of interaction, like creating dashboards, accessing with a mobile application (currently only in Android), accessing from the API explorer, or calling the cloud API directly.

## Android

One of the easiest ways to access your devices is to simply issue a device token to grant access to the device. When you generate a device token you can restrict the shared resources, and the token expiration time, so you can share some device functionality with other people safely. The idea is that you can use this device token and scan it as a QR code in the Android application.

<p align="center">
<img src="assets/token.png" width="325px">
</p>

> Generate a device token in your cloud console, so you can easily interact from your phone within minutes.

### Mobile APP

The current version of the [Android APP](https://play.google.com/store/apps/details?id=io.thinger.thinger) does not require any kind of login to interact with your devices. Simply scan your token as a QR code and your phone will be able to interact with your device for reading sensor values, changing led or relays states, and so on.

<p align="center">
<img src="assets/phone.png" width="325px">
</p>

> In the current version of the Android APP, you can interact with your device resources out of the box. Just scan the QR Code and start interacting with your resources directly from the Internet.

### Android Wear

Coming soon!

## Cloud Console
In progress... Refer to the Cloud Console documentation


### Dashboard

<p align="center">
<img src="assets/dashboards.gif" width="100%">
</p>


### API Explorer


## Server API

In progress... Refer to the Server API documentation
