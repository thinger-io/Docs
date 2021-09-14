---
description: This section describes how to integrate the Things Stack with Thinger.io
---

# LoRaWAN DEVICES

## Integrating TTN devices

When using our TTN or TTN Stack \(v3\) platform, it is possible to integrate LoRaWAN devices with Thinger.io using our "the things stack integration plugin", which simplifies the connection of both platform and also provides some interesting features such as devices auto-provisioning, payload data processing and gateway data filtering**, f**[**ollow this link to read the how to guide**](../plugins/the-things-stack.md) ****\(this is the option we recommend\). 

{% embed url="https://www.youtube.com/watch?v=8jn7ACmjnvM" %}

## Other LoRaWAN devices

Working with other LoRaWAN servers it is also possible to integrate devices with thinger.io platform by forwarding data from the LoRaWAN server, but note that in this case, RAW data will be received including the gateway information and the hexadecimal payload value that needs the be processed. The next steps explain how to use Node-RED to make this integration and process the devices data in order to make a proper integration with Thinger.io: 

