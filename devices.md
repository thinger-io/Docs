---
description: >-
  Thinger.io provides comprehensive support for a variety of devices and
  protocols, enabling seamless integration from simple IoT applications to
  complex industrial systems.
cover: >-
  https://images.unsplash.com/photo-1524234107056-1c1f48f64ab8?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw0fHxlc3A4MjY2fGVufDB8fHx8MTcxNzQ5NzI5MXww&ixlib=rb-4.0.3&q=85
coverY: -72.00266666666667
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: false
---

# CONNECT A DEVICE

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Arduino Compatible Devices</strong></td><td></td><td></td><td><a href=".gitbook/assets/arduino logo.png">arduino logo.png</a></td><td><a href="overview.md">overview.md</a></td></tr><tr><td><strong>Linux and Raspberry Pi Devices</strong></td><td></td><td></td><td><a href=".gitbook/assets/Raspberry_Pi_Logo.jpg">Raspberry_Pi_Logo.jpg</a></td><td><a href="linux.md">linux.md</a></td></tr><tr><td><strong>MQTT Devices</strong></td><td></td><td></td><td><a href=".gitbook/assets/mqtt_devices.jpeg">mqtt_devices.jpeg</a></td><td><a href="mqtt.md">mqtt.md</a></td></tr><tr><td><strong>Sigfox Devices</strong></td><td></td><td></td><td><a href=".gitbook/assets/sigfox.jpeg">sigfox.jpeg</a></td><td><a href="lpwan/sigfox.md">sigfox.md</a></td></tr><tr><td><strong>LoraWan Devices</strong></td><td></td><td></td><td><a href=".gitbook/assets/lorawan.png">lorawan.png</a></td><td><a href="lpwan/the-things-stack.md">the-things-stack.md</a></td></tr><tr><td><strong>Generic HTTP Devices</strong></td><td></td><td></td><td><a href=".gitbook/assets/https.jpeg">https.jpeg</a></td><td><a href="http-devices.md">http-devices.md</a></td></tr></tbody></table>

To connect a device, choose the appropriate technology that matches the device's capabilities and communication protocol.

* **Arduino Compatible Devices**: Any Arduino with Internet connectivity, including the Espressif [ESP8266](arduino/espressif-esp8266.md) and [ESP32](arduino/espressif-esp32.md) devices. Connect them with the IOTMP protocol, which enables a Device API, includes[ Remote OTA](ota.md), and [Remote Console](remote-console.md).
* **Linux/Raspberry Pi Devices**: Any device running Linux, using IOTMP protocol, including Device API, Remote SSH, Sockets Proxy, and Remote Web Services.
* **MQTT Devices**: Connect any device over the MQTT protocol. MQTT (Message Queuing Telemetry Transport) is a messaging protocol for small sensors and mobile devices.
* **Sigfox Devices**: Connect any device over the Sigfox 0G network. Sigfox is a global IoT network operator that provides low-power, wide-area network (LPWAN) services, ideal for applications requiring long battery life and low data transmission rates.
* **LoraWAN Devices**: Connect any device using the LoraWAN protocol. LoraWAN (Long Range Wide Area Network) is a protocol for low-power, wide-area networks designed to wirelessly connect battery-operated things to the internet in regional, national, or global networks.
* **HTTP Devices**: Connect any device using HTTP requests to communicate with Thinger.io. HTTP (Hypertext Transfer Protocol) is the foundation of data communication for the World Wide Web, allowing devices to send and receive data over the Internet.

