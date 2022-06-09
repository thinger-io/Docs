---
description: >-
  Have an IoT product and want to simplify its management at scale? Configure
  data to be stored, create dashboard templates, set custom data processors, or
  build REST APIs for MQTT devices.
---

# PRODUCTS

A product inside Thinger.io is a way to define behaviors for a set of devices of the same type. Suppose you have a fleet of hundred or thousand of devices, and want to configure them at scale, for example, storing certain device data to a data bucket. With a product, it is possible to specify the resource name/interval, i.e., temperature every 1 minute, and all the devices associated to the product will start to store the information automatically. But this is not the only option, as it can be used for update device properties, define device APIs (even for MQTT), add custom data processors, and design dashboard templates.

These options are described in more detail in the following subsections.

## Product Profile

The product profile is the main page where a product is configured. It can be used for setting device properties, data buckets, api resources, and custom product scripts. It looks like in the following picture:

![Product Profile Overview](<../.gitbook/assets/image (456).png>)

Next subsections describe the different configurable options inside the product profile. For describing the products feature, it is used a real device for the examples. In this case, it is used a Shelly Plug S device, that is a WiFi Smart Plug with power metering, that can be easily integrated on the platform. As it supports MQTT protocol by default, it is not required to re-flash the device, keeping the device warranty.
