---
description: Streamlined Management and Analysis of Large IoT Fleets
cover: >-
  https://images.unsplash.com/photo-1545259741-2ea3ebf61fa3?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw1fHxpb3R8ZW58MHx8fHwxNjc1ODY3Nzky&ixlib=rb-4.0.3&q=80
coverY: 0
---

# PRODUCTS

Have an IoT product and want to simplify its management at scale? Configure data to be stored, create dashboard templates, set custom data processors, or build custom REST APIs for them, including MQTT devices.

## Features

A product inside Thinger.io is a way to define behaviors for a set of devices of the same type. At this moment it is able to offer different capabilities:

* [**Product Profile**](product-profile/)**:** The product profile in Thinger.io serves as the central hub for configuring a product, providing access to various tools such as setting device properties, time series data storage, custom API creation, and custom scripts for tailored payloads and functionalities.
  * [**Properties**](product-profile/properties.md): Store and manage various metadata related to the devices in a fleet, including the latest device state, owner information, device information, location, custom configuration, and more. This helps organizations to keep track of their devices and make quick updates when needed.
  * [**Buckets**](product-profile/buckets.md): Automatic storage of time-series data for a specified resource name/interval or MQTT topic. This helps organizations effectively manage and analyze data from large numbers of IoT devices.
  * [**API Resources**](product-profile/api-resources.md): Unified API access for various devices and protocols, payload processing capabilities, and compatibility with MQTT and the Device API Explorer. These features simplify the integration of different devices and systems, making it easier to test and manage them.
  * [**Scripts**](product-profile/scripts.md): All data related to a Product can be handled and processed over Scripts for processing payloads in different formats, converting units, creating virtual functions to generate calculated data, etc. There are endless possibilities.
* [**Product Dashboard**](product-dashboard.md): Create a single dashboard layout for each Product. Each device will display this dashboard automatically with its own set of data from properties, and data buckets, and will be able to interact with the device using the unified API Resources. Example of a Shelly Plug S dashboard:

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption><p>Product Dashboard example over a Shelly Plug S device</p></figcaption></figure>

* [**Product Services**](product-dashboard.md): Devices using the IOTMP protocol can provide extended features aside from remote sensing and actuation. At this moment, it includes the possibility to access remotely to web services (using Linux Clients). For example, it will allow controlling the router/gateway admin panel; 3D Printer monitoring page; or any other Industrial product which includes a web frontend for its management. All without using any VPN. Here is an example of remote PLCs management, using in this case PiCtory from [Revolution Pi](https://revolutionpi.com/).

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption><p>Product Remote Web Services - Kunbus Revolutiion Pi Example</p></figcaption></figure>

## Examples

Don´t want to read the whole documentation? Take a look at some of the available examples:

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><strong>Shelly Plug S</strong></td><td>Wi-Fi smart plug with power metering</td><td><a href="examples/shelly-plug-s.md">shelly-plug-s.md</a></td><td><a href="../../.gitbook/assets/Shelly Plug Plus S IoT.jpg">Shelly Plug Plus S IoT.jpg</a></td></tr><tr><td><strong>Kunbus RevPi</strong></td><td>Open Source IPC based on Raspberry Pi</td><td><a href="examples/kunbus-revpi.md">kunbus-revpi.md</a></td><td><a href="../../.gitbook/assets/kunbus revpi iot.jpg">kunbus revpi iot.jpg</a></td></tr><tr><td><strong>Shelly Plus 1 PM</strong></td><td>Wi-Fi-operated smart relay with Power Metering</td><td><a href="examples/shelly-plus-1-pm.md">shelly-plus-1-pm.md</a></td><td><a href="../../.gitbook/assets/ShellyPlus1PM_9_1051x700.webp">ShellyPlus1PM_9_1051x700.webp</a></td></tr></tbody></table>

## Marketplace

Want to integrate your Product into our Plugin Marketplace? Just [**become a Partner**](https://thinger.io/become-a-partner)!

Some of its benefits are:

* The product is directly available as a Plugin from the Thinger.io Marketplace.&#x20;
* Zero-pain integration for your customers. Just a few clicks to have hundreds of devices managed at scale.
* Your products and brand are featured on our web and marketplace. It provides broad visibility in one of the largest IoT Communities.
* Best-in-class integration for your products. On-demand training and integration development by Thinger.io experts.
* Private Cloud for demos and POCs with your customers.

{% embed url="https://thinger.io/become-a-partner" %}
