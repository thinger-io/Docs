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

<table data-view="cards"><thead><tr><th></th><th></th><th data-type="content-ref"></th><th data-hidden data-card-cover data-type="image">Cover image</th></tr></thead><tbody><tr><td><strong>The Things Stack</strong></td><td>The world’s largest community-driven LoRaWAN network, freely accessible and widely adopted for collaborative and educational projects. It enables quick onboarding and extensive global coverage.</td><td><a href="https://marketplace.thinger.io/plugins/ttn/">https://marketplace.thinger.io/plugins/ttn/</a></td><td><a href="https://iot.wifx.net/wp-content/uploads/2021/03/TTN_TTS.png">https://iot.wifx.net/wp-content/uploads/2021/03/TTN_TTS.png</a></td></tr><tr><td><strong>LORIOT LNS</strong></td><td>A professional-grade solution offering advanced enterprise services, including scalability, high availability, and SLA-backed support. It is commonly chosen for commercial and industrial deployments.</td><td><a href="https://marketplace.thinger.io/plugins/loriot/">https://marketplace.thinger.io/plugins/loriot/</a></td><td><a href="https://www.venturelab.swiss/demandit/files/M_BB941CC4DCEF687AD98/dms/Image/B08590E7-C129-F91B-01CDA7FE6BCAEEC4.jpg">https://www.venturelab.swiss/demandit/files/M_BB941CC4DCEF687AD98/dms/Image/B08590E7-C129-F91B-01CDA7FE6BCAEEC4.jpg</a></td></tr><tr><td><strong>ChirpStack</strong></td><td>An open-source LNS that must be self-hosted and maintained. It provides maximum control and customization for developers, but requires infrastructure management and operational expertise.</td><td></td><td><a href="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQU-IfBY3PQzSQ1ZoAAunH5tmimuVqf9Kw9wQ&#x26;s">https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQU-IfBY3PQzSQ1ZoAAunH5tmimuVqf9Kw9wQ&#x26;s</a></td></tr></tbody></table>

> **Note:** Some LoRaWAN gateways include built-in LNS functionality. Support for this type of integration is currently being explored.

Thinger.io does not endorse one LNS provider over another. Multiple integrations are provided to reach the broadest possible range of user communities. It remains the responsibility of the user to evaluate and decide which service best suits the requirements of a specific project and deployment scenario.

## Integrating LoRaWAN Devices

### The Challenge

When you build a LoRaWAN IoT product, your devices send data (uplinks) and receive commands (downlinks) through a LoRaWAN Network Server (LNS). Each LNS provider uses its own JSON schema for device uplinks and downlinks, which makes it impossible to create a generic product configuration. That is why each LNS plugin parses each LNS request into a Thinger.io standardized format.

Consider a temperature monitoring product initially configured with LORIOT. The product reads device properties, configures dashboard widgets, and defines data processing rules based on LORIOT's specific JSON structure. Six months later, you decide to switch to The Things Stack. **Without standardization**, you would need to:

* Rewrite data processing rules to match the new schema
* Update device property mappings
* Modify automation workflows and API integrations

### The Solution

**Thinger.io's LoRaWAN plugins implement a protocol adaptation layer that normalizes data from any LNS provider into a unified schema.** Each plugin acts as a bidirectional parser, translating between the LNS-specific format and Thinger.io's standard format.

#### The Standarized UpLink Schema

Every uplink message processed by a Thinger.io LoRaWAN plugin conforms to a canonical JSON schema, regardless of the originating LNS:

```json
{
  "deviceEui": "1234567890ABCDEF",
  "deviceId": "sensor-room-101",
  "source": "loriot",
  "appId": "building-monitoring",
  "fPort": 1,
  "fCnt": 42,
  "payload": "01A5CB1288A",
  "decodedPayload": null,
  "metadata": {
    "ack": false,
    "battery": 254,
    "offline": false,
    "seqNo": 12345
  }
}
```

<table><thead><tr><th width="162.51953125">Field</th><th width="155.359375">Type</th><th width="434.0234375">Definition</th></tr></thead><tbody><tr><td>deviceEui</td><td>string</td><td>Device Extended Unique Identifier</td></tr><tr><td>deviceId</td><td>string</td><td>Human-readable device identifier. This is usually used to autoprovision the device.</td></tr><tr><td>source</td><td>string</td><td>LNS provider identifier (lowercase: "loriot", "chirpstack", "ttn")</td></tr><tr><td>appId</td><td>string</td><td>Application identifier within the LNS</td></tr><tr><td>fPort</td><td>integer</td><td>LoRaWAN application port</td></tr><tr><td>fCnt</td><td>integer</td><td>Uplink frame counter</td></tr><tr><td>payload</td><td>string (hex) | null</td><td>Hex-encoded raw payload bytes (when LNS doesn't decode)</td></tr><tr><td>decodedPayload</td><td>object | nulll</td><td>Decoded uplink data (when LNS provides decoding)</td></tr><tr><td>metadata</td><td>object</td><td>Optional LNS-specific metadata</td></tr></tbody></table>

{% hint style="info" %}
Exactly one of `payload` or `decodedPayload` must be present
{% endhint %}

#### The Standarized Downlink Schema

For the same reason as Uplinks, Downlink data format must properly formatted and forwarded to Thinger.io LNS plugin. As an example;

```json
{
  "data": "FF0164",
  "port": 85,
  "priority": 3,
  "confirmed": false,
  "uplink": {
    "deviceEui": "1234567890ABCDEF",
    "fCnt": 42,
    ...
  }
}
```

<table><thead><tr><th width="167.75">Field</th><th width="232.7265625">Type</th><th>Definition</th></tr></thead><tbody><tr><td>data</td><td>string (hex)</td><td>Hex-encoded payload bytes</td></tr><tr><td>port</td><td>integer</td><td>LoRaWAN FPort</td></tr><tr><td>priority</td><td>integer (0, 1 ... 5, 6)</td><td>Queue priority (0=lowest, 6=highest)</td></tr><tr><td>confirmed</td><td>boolean</td><td>Request MAC-layer acknowledgment</td></tr><tr><td>uplink</td><td>object</td><td>Last Recieved Uplink</td></tr></tbody></table>

### Product Configuration Profile

The [LoRaWAN Product Template](https://marketplace.thinger.io/plugins/lorawan-product-template/) in the [Marketplace](https://marketplace.thinger.io/) has been specifically designed to be used as a generic template for any LoRa-driven device working through any LNS supported in Thinger.io. To start building a LoRaWAN device, this integration would be as simple as installing this generic template, clone it to change the device\_id and start working!

This generic template provides a product with the following basic configuration

#### Uplink Property

This Uplink Property stores the last uplink received from the device. It is used to recall essential information needed to send downlinks to the device or to visualize specific device metadata in dashboards.

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="415"><figcaption><p>Uplink Property</p></figcaption></figure>

#### Generic Data Bucket

A pre-configured data bucket is also created within this product template to provide a generic "end-of-pipeline" storage solution for the data. This bucket automatically stores time-series data from device uplinks.

<figure><img src="../.gitbook/assets/image (5).png" alt="" width="407"><figcaption><p>Generic Data Bucket</p></figcaption></figure>

{% hint style="info" %}
It is **recommended** (though not required) to perform **data decoding at the bucket stage** rather than at the API resource level. This approach ensures that the **raw uplink message** stored in the product property remains **unaltered**, preserving the exact payload format originally received from the LNS plugin.
{% endhint %}

#### API Resource&#x20;

Pre-configured uplink and downlink endpoints have been created within the product profile configuration, defining a unified data communication bridge with the LNS Thinger.io Plugin. These channels communicate directly with the LNS plugin, so all data incoming to and outgoing from these endpoints is expected to be correctly formatted according to the standardized schema.

<figure><img src="../.gitbook/assets/image (6).png" alt="" width="461"><figcaption><p>API resources</p></figcaption></figure>

{% hint style="info" %}
These API endpoints use the standardized format described in the plugin documentation. Do not modify the endpoint structure unless you understand the implications for cross-LNS compatibility.
{% endhint %}

#### Auto Provision

The Device autoprovisioning will need to be configured in the application that has been created in the LNS Thinger Plugin. This device ID comes in two parts:&#x20;

* The **prefix** needs to be specified in both the product autoprovision schema and in the LNS application plugin configuration. This ensures consistent device identification across both systems.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Autoprovision Configured in Product Profile</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (8).png" alt="" width="482"><figcaption><p>Device ID Prefix configured in LNS Plugin (in this case, The Things Stack)</p></figcaption></figure>

* The device ID **sufix**: This id sufix is the device EUI wich will be appended in the LNS Thinger Plugin.

