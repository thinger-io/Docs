---
description: >-
  In this section it is explained how to connect your devices using Thinger.io
  platform.
---

# COMPATIBLE DEVICES

As there are many different IoT hardware available nowadays, this section is divided in different categories: 

**Arduino compatible devices** such as Arduino MKR,  Ethernet hats + Arduino classic boards, ESP8266, NodeMCU, TI CC3200, etc. 

{% page-ref page="arduino.md" %}

**Linux** **devices**, including **Raspberry Pi**, Intel Edison and any other Linux computer running Linux or MacOs

{% page-ref page="linux.md" %}

**Low-Power Devices** or "Edge devices" such as **Sigfox**, **LoraWAN**, **TheThingsNetwork** or any other infrastructure with gateways:

{% page-ref page="sigfox.md" %}

**Any other device or third-party Platform** that can't be furnished with Thinger.io software client, can be also integrated using our open API

{% page-ref page="http-devices.md" %}

## 

<table>
  <thead>
    <tr>
      <th style="text-align:left"><a href="arduino.md">Arduino</a>
      </th>
      <th style="text-align:left"></th>
      <th style="text-align:left"><a href="linux.md">Linux</a>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>&lt;b&gt;&lt;/b&gt;<a href="arduino.md"><b> </b></a>
        </p>
        <p><a href="arduino.md"><img src="../.gitbook/assets/arduino-logo.png" alt/></a>
        </p>
        <p></p>
        <p><a href="arduino.md"> </a>
        </p>
        <p></p>
        <p><a href="arduino.md">This category is related with any hardware that can be programmed over the Arduino IDE and its libraries. So it is not exclusively for Arduino Devices, as you can also program different boards like the ESP8266, ESP32, </a>Arduino
          MRK, etc.</p>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left">
        <p><a href="linux.md"><img src="../.gitbook/assets/imagen1.png" alt/></a>
        </p>
        <p>For <a href="linux.md">Linux OS based devices, such as  Raspberry Pi, Intel Edison, and many other computer running a Linux distribution, including a Mac OS computer.</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>Edge Devices</b>
        </p>
        <p>&lt;b&gt;&lt;/b&gt;<a href="sigfox.md">&lt;b&gt;&lt;/b&gt;<img src="../.gitbook/assets/edge-devices-thinger.io (1).png" alt/><b> </b></a>&lt;b&gt;&lt;/b&gt;</p>
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
          <img src="../.gitbook/assets/mbed-enabled-logo.png"
          alt/>
        </p>
        <p>ARM is building its own IoT ecosystem in the cloud, mainly to simplify
          the development process when using this kind of hardware.</p>
      </td>
    </tr>
  </tbody>
</table>However, Thinger.io can be adapted to many other hardware devices running our C++ source code libraries or third party platforms by using the HTTP device integration. 

