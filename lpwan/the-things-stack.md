---
description: This section describes how to integrate the Things Stack with Thinger.io
---

# LoRaWAN

LoRaWAN (Long Range Wide Area Network) is a low-power, wide-area networking protocol designed to connect wireless battery-powered devices to the internet across regional, national, or even global deployments. It is optimized for low energy consumption while providing secure bi-directional communication, localization capabilities, and support for mobility.

As one of the most widely adopted technologies in the IoT market, LoRaWAN provides several advantages:

* **Long Range:** Coverage of several kilometers in rural environments and a few kilometers in dense urban areas.
* **Low Power Consumption:** Devices can operate for years using standard batteries.
* **High Capacity:** A single gateway can manage thousands of devices simultaneously.
* **Cost Efficiency:** Reduced infrastructure and operational costs compared to cellular alternatives.
* **Secure Communication:** End-to-end encryption ensures data confidentiality and integrity.

Device integration is usually achieved through a **LoRaWAN Network Server (LNS)**, which manages communication between gateways and applications.

## Compatible LNS

Thinger.io maximizes interoperability with LoRaWAN by offering plugins that integrate with the most widely used LNS providers in the market

<table data-view="cards"><thead><tr><th></th><th></th><th data-type="content-ref"></th><th data-hidden data-card-cover data-type="image">Cover image</th></tr></thead><tbody><tr><td><strong>The Things Stack</strong></td><td>The world’s largest community-driven LoRaWAN network, freely accessible and widely adopted for collaborative and educational projects. It enables quick onboarding and extensive global coverage.</td><td><a href="https://marketplace.thinger.io/announcements/category/ttn/">https://marketplace.thinger.io/announcements/category/ttn/</a></td><td><a href="https://alfaiot.com/wp-content/uploads/2024/03/The_Things_Network_logo.svg_-1.png">https://alfaiot.com/wp-content/uploads/2024/03/The_Things_Network_logo.svg_-1.png</a></td></tr><tr><td><strong>LORIOT LNS</strong></td><td>A professional-grade solution offering advanced enterprise services, including scalability, high availability, and SLA-backed support. It is commonly chosen for commercial and industrial deployments.</td><td><a href="https://marketplace.thinger.io/announcements/category/ttn/">https://marketplace.thinger.io/announcements/category/ttn/</a></td><td><a href="https://us1.loriot.io/assets/images/login-logo.png">https://us1.loriot.io/assets/images/login-logo.png</a></td></tr><tr><td><strong>ChirpStack</strong></td><td>An open-source LNS that must be self-hosted and maintained. It provides maximum control and customization for developers, but requires infrastructure management and operational expertise.</td><td><a href="https://marketplace.thinger.io/announcements/category/ttn/">https://marketplace.thinger.io/announcements/category/ttn/</a></td><td><a href="https://www.chirpstack.io/img/logo.png">https://www.chirpstack.io/img/logo.png</a></td></tr></tbody></table>

> **Note:** Some LoRaWAN gateways include built-in LNS functionality. Support for this type of integration is currently being explored.

Thinger.io does not endorse one LNS provider over another. Multiple integrations are provided to reach the broadest possible range of user communities. It remains the responsibility of the user to evaluate and decide which service best suits the requirements of a specific project and deployment scenario.

