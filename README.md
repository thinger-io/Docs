---
description: >-
  Thinger.io is an Open-Source Platform for the Internet of Things. This
  documentation helps learning about how to use each component to create awesome
  projects within minutes.
---

# OVERVIEW

## What is Thinger.io?

Thinger.io is a cloud IoT Platform that provides every needed tool to prototype, scale and manage connected  products in a very simple way. Our goal is democratize the use of IoT making it accessible to the hole world, and streamlining the development of big IoT projects.

* **Free IoT platform**: Thinger.io provides a lifetime freemium account with only few limitations to start learning and prototyping, when your product becomes ready to scale, you can deploy a Premium Server with full capacities within minutes.
* **Simple but Powerful**: Just a couple code lines to connect a device and start retrieving data or controlling it's functionalities with our web based Console, able to connect and manage thousands of devices in a simple way.
* **Hardware agnostic:** Any device from any manufacturer can be easily integrated with Thinger.io's infrastructure.
* **Open-Source**: most of the platform modules, libraries and APP source code are available in our github repository to be downloaded and modified with MIT license. 
* **Customizable**: Fully white-labelable frontend allows customizing Thinger.io Platform with your brand colors, logotype and web domain.

### Thinger.io Main Features

![](.gitbook/assets/thinger.io-platform-feature.png)

* **Connect devices:** Any kind of device can be integrated, no matter the processor, the network or the manufacturer. Thinger.io is ready to create bidirectional communications with all of them even with edge devices like Sigfox, or LoRaWAN ones. 
* **Bidirectional Efficient Communications:** Allows retrieving Real-time data from thousands of devices, but also to send data to them with extremely low latency. Perfect for smart-autonomous projects o remote controlling systems. 
* **Store Device Data:** Just a couple clicks to create a Data Bucket a store IoT data in an scalable, efficient and affordable way, that also allows real-time data aggregation. 
* **Show Real-time or Stored Data:** Using our awesome dashboards, it is possible to create used-friendly data visualization interfaces and share it in minutes with your customers. 
* **Cloud Computing:** Using the scheduler and other Plugins it is possible to process device data to extract useful information or create automatic behaviors. 
* **Integrate with third parties:** Our Open API allows retrieving data and share it with third party Internet Platforms and custom programs.

Are you ready to start creating IoT projects? [**Create here your free account**](https://console.thinger.io/#/signup) and learn below how to use all this technology.

## Compatible Devices

This section will cover how to program your hardware to connect and use the Thinger.io platform. As there are many IoT hardware available nowadays, this section is divided in different categories.

There is a category related with Arduino compatible hardware, which is any board you can program with the Arduino IDE \(Arduino + Ethernet, Arduino + Wifi, ESP8266, NodeMCU, TI CC3200, etc\). There is other general category related with Linux powered devices like the Raspberry Pi, Intel Edison, or any other Linux computer running Ubuntu or MacOS. Finally, there is a section that will cover the integration with the ARM Mbed platform and compatible devices.

So, to start using the platform select the hardware platform you want to use.

<table>
  <thead>
    <tr>
      <th style="text-align:left"><a href="hardware-devices/arduino.md">Arduino</a>
      </th>
      <th style="text-align:left"></th>
      <th style="text-align:left"><a href="hardware-devices/linux.md">Linux</a>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>&lt;b&gt;&lt;/b&gt;<a href="hardware-devices/arduino.md"><b> </b></a>
        </p>
        <p><a href="hardware-devices/arduino.md"><img src=".gitbook/assets/arduino-logo.png" alt/></a>
        </p>
        <p></p>
        <p><a href="hardware-devices/arduino.md"> </a>
        </p>
        <p></p>
        <p><a href="hardware-devices/arduino.md">This category is related with any hardware that can be programmed over the Arduino IDE and its libraries. So it is not exclusively for Arduino Devices, as you can also program different boards like the ESP8266, ESP32, </a>Arduino
          MRK, etc.</p>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p><a href="hardware-devices/linux.md"><img src=".gitbook/assets/imagen1.png" alt/></a>
        </p>
        <p>For <a href="hardware-devices/linux.md">Linux OS based devices, such as  Raspberry Pi, Intel Edison, and many other computer running a Linux distribution, including a Mac OS computer.</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>Edge Devices</b>
        </p>
        <p></p>
        <p></p>
        <p></p>
        <p></p>
        <p>
          <img src=".gitbook/assets/sigfox-logo.jpg" alt/>
        </p>
        <p></p>
        <p></p>
        <p></p>
        <p></p>
        <p>In this category is covered the Sigfox integration with the platform,
          where the user can configure callbacks for transmitting data from the devices
          to the cloud for creating real-time dashboards.</p>
      </td>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;</td>
      <td style="text-align:left">
        <p><b>ARM mbed</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;
          <img src=".gitbook/assets/mbed-enabled-logo.png" alt/>
        </p>
        <p>ARM is building its own IoT ecosystem in the cloud, mainly to simplify
          the development process when using this kind of hardware. So, to integrate
          an ARM mBed compatible board, this is your place.</p>
      </td>
    </tr>
  </tbody>
</table>However, Thinger.io can be adapted to many other hardware devices running our C++ source code libraries or third party platforms by using the HTTP device integration. 

## [Software Client Coding](coding.md)

[ ![](.gitbook/assets/coding.png) ](coding.md)

This section will cover how to add different functionality to your devices for exposing resources, calling endpoints, or streaming data to real-time websockets.

## [Cloud Console](console.md)

[![](.gitbook/assets/console.png) ](console.md)

The Cloud Console is related with the management front-end designed to easily manage your devices and visualize its information in the cloud. In this section you will learn how to register devices, create real-time dashboards, access the devices API, and other management operations.

## Plugins System

![](.gitbook/assets/imagen1dddcc.png) 

Plugins are extensions that allows complementing Thinger.io Platform with specific features or functionalities. 

## [Mobile A](mobile.md)[pp](mobile.md)

[![](.gitbook/assets/mobile-app.png) ](mobile.md)

This section is related with the use of the Android and iOS Mobile App. You will learn how to scan devices, point them to another server or visualize and manage its information.

## [Server API](api.md)

[![](.gitbook/assets/api.png)](api.md)

The Thinger.io Cloud is supported by a REST API designed to easily integrate your IoT projects with other applications like Web Services, mobile phones, or desktop applications. So if you want to integrate your device with your own application or service, then this is the starting point.

## Plugins

## [Thinger.io Hardware](climastick/)

[![](.gitbook/assets/climastick.jpg) ](climastick/)

This section is related to the Thinger.io hardware boards, like the ClimaStick. A small powerful device plenty of sensors, WiFi, ready for prototyping, learning, or developing products.

## [Server Deployment](deployment.md)

[![](.gitbook/assets/docker-logo.png) ](deployment.md)

This section is related to the Cloud configuration and deployment in your own infrastructure, both in local machines or in the cloud.

