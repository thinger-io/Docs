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

## Thinger.io Main Features

![](.gitbook/assets/thinger.io-platform-feature.png)

* **Connect devices:** Any kind of device can be integrated, no matter the processor, the network or the manufacturer. Thinger.io is ready to create bidirectional communications with all of them even with edge devices like Sigfox, or LoRaWAN ones. 
* **Bidirectional Efficient Communications:** Allows retrieving Real-time data from thousands of devices, but also to send data to them with extremely low latency. Perfect for smart-autonomous projects o remote controlling systems. 
* **Store Device Data:** Just a couple clicks to create a Data Bucket a store IoT data in an scalable, efficient and affordable way, that also allows real-time data aggregation. 
* **Show Real-time or Stored Data:** Using our awesome dashboards, it is possible to create used-friendly data visualization interfaces and share it in minutes with your customers. 
* **Cloud Computing:** Using the scheduler and other Plugins it is possible to process device data to extract useful information or create automatic behaviors. 
* **Integrate with third parties:** Our Open API allows retrieving data and share it with third party Internet Platforms and custom programs.

Are you ready to start creating IoT projects? [**Create here your free account**](https://console.thinger.io/#/signup) and learn below how to use all this technology.

## Quick Start Guide

### 1.Create Device

Using "Devices" menu tab, just click in "New device" button, and fill the form with the device ID, description and Credentials you prefer.

![](.gitbook/assets/image%20%289%29.png)

### 2.Connect Device

After provisioning the device at Thinger.io cloud, it is the moment to configure it in the Hardware device. there are many different hardware supports and communication technologies but Thinger.io allows using all of them: 

{% tabs %}
{% tab title="Arduino/Linux Devices" %}
1. Install Thinger.io libraries into your Arduino IDE
2. Going to "File&gt;Examples&gt;Thinger.io", open the example code that fix better for the PCB
3. Edit the example code to write Thinger.io and connection credentials as shown in the image below. Finally upload the sketch

![](.gitbook/assets/image%20%2867%29.png)

{% hint style="success" %}
Find additional documentation about Arduino or Linux devices connection at the **COMPATIBLE DEVICES** section 
{% endhint %}
{% endtab %}

{% tab title="HTTP devices" %}
1\) Create an HTTP device profile by selecting it in the "Device Type" when creating the device  
2\) Going to the device dashboard, create an HTTP device Callback  
3\) Create a device Access Tocken to authorize the device sending data to the platform  
4\) Introduce the HTTP request \(API+TOKEN\) into your device code or third party platform and start sending data to Thinger.io

{% hint style="success" %}
[Follow a detailed tutorial about HTTP devices connection **here!**](hardware-devices/http-devices.md)
{% endhint %}
{% endtab %}

{% tab title="Sigfox / LoRaWAN devices" %}
Any individual Sigfox or LoraWAN device can be integrated using our API as HTTP devices, just setting an HTTP device callback into their callback managers, but if a big network is going to be created using these technologies, it is better to use our integration plugins:

{% page-ref page="plugins/sigfox.md" %}

{% page-ref page="plugins/the-things-network.md" %}
{% endtab %}
{% endtabs %}

Going to "devices" menu tab, a complete device list will be shown, allowing to manage the devices and check its connection status. Just click over the device ID to access the device management dashboard

### 3.Retrieve & Send Data to Devices

Each device can be managed through the "Device Dashboard", an interface that shows connection data and also allows checking the "device API" with raw device data representation.

Thinger.io provides bidirectional communication, so it is possible to retrieve data into the server from devices "output resources" and send messages from server to devices "input resources". Both resources are represented in the "device API" viewer.

### 4.Store, Show & Share Data

Thinger.io provides three essential tools to work with devices data that are the basis for creating any IoT project, such as: 

| Data Buckets | Dashboards | Access Tokens |
| :--- | :--- | :--- |
| To **store** **device data** in an scalable way, programming different sampling intervals or catching streams | Panels with **customizable widgets** that can be created within minutes using drag'n drop technology, to show real time and stored data | Dashboards, Data buckets or Device resources can be easily shared with third parties using **Access Tokens** and our **API** |

### 5.Extend Thinger.io

Thinger.io platform can be complemented with many different Internet services using **Plugins** that can be found and deployed within seconds Just going to our marketplace and selecting it:

{% page-ref page="plugins/" %}

## Compatible Devices

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
        <p>&lt;b&gt;&lt;/b&gt;<a href="hardware-devices/sigfox/">&lt;b&gt;&lt;/b&gt;<img src=".gitbook/assets/edge-devices-thinger.io (1).png" alt/><b> </b></a>&lt;b&gt;&lt;/b&gt;</p>
        <p>In this category is covered the Sigfox integration with the platform,
          where the user can configure callbacks for transmitting data from the devices
          to the cloud for creating real-time dashboards.<b> </b>
        </p>
      </td>
      <td style="text-align:left">&lt;b&gt;&lt;/b&gt;</td>
      <td style="text-align:left">
        <p><b>ARM mbed</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;
          <img src=".gitbook/assets/mbed-enabled-logo.png" alt/>
        </p>
        <p>ARM is building its own IoT ecosystem in the cloud, mainly to simplify
          the development process when using this kind of hardware.</p>
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



