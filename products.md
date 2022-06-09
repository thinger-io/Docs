---
description: >-
  Have an IoT product and want to simplify its management at scale? Configure
  data to be stored, create dashboard templates, set custom data processors, or
  build REST APIs for MQTT devices.
---

# PRODUCTS

A product inside Thinger.io is a way to define behaviors for a set of devices of the same type. Suppose you have a fleet of hundred or thousand of devices, and want to configure them at scale, for example, storing certain device data to a data bucket. With a product, it is possible to specify the resource name/interval, i.e., temperature every 1 minute, and all the devices associated to the product will start to store the information automatically. But this is not the only option, as it can be used for update device properties, define device APIs (even for MQTT), add custom data processors, and design dashboard templates.

These options are described in more detail in the following sections.

## Product Profile

The product profile is the main page where a product is configured. It can be used for setting device properties, data buckets, api resources, and custom product scripts. It looks like in the following picture:

![Product Profile Overview](<.gitbook/assets/image (456).png>)

Next sections describe the different configurable options inside the product profile. For describing the products feature, it is used a real device for the examples. In this case, it is used a Shelly Plug S device, that is a WiFi Smart Plug with power metering, that can be easily integrated on the platform. As it supports MQTT protocol by default, it is not required to re-flash the device, keeping the device warranty.

### Properties

A property in a device is a way to store arbitrary data, i.e., store device configuration, latest device state, or any other data required for the use case. Defining a property inside a Product Profile allow:

* Automatic device property update from MQTT topic or IOTMP device resource for devices associated to the product
* Setting a default value for devices that did not updated the value yet
* Data preprocessing before its use for the property, i.e., modify measurement units, filter undesired data, etc.

For the Shelly Plug S device example, we will define two different properties to store the latest device state. Shelly Plug S can connect to a MQTT broker, and once connected, it periodically send information like temperature, relay state, real-time power consumption, available updates, etc. In this example, we will use the current power consumption and the rely sate, so, <mark style="color:purple;">power</mark> and <mark style="color:green;">relay</mark>, are defined as device properties, as shown in the following picture:

![Properties definition for Shelly Plug S SmartPlug](<.gitbook/assets/image (452).png>)

Both properties are updated from the following device topics:

* <mark style="color:blue;">power</mark> is updated from topic `shellies/{{device}}/relay/0/power`
* <mark style="color:green;">relay</mark> is updated from topic `shellies/{{device}}/relay/0`

To configure the properties in the product profile, just click on `Add` on the properties section. On the dialog, enter the property identifier (i.e., power or relay), select the `Device Topic` source, fill the `Topic`, and configure the payload, as shown in the following picture:

![](<.gitbook/assets/image (463).png>)

By default, the payload established on the property is`{{payload}}`, which is a placeholder that will be replaced with the contents received from the configured source. In this case, from the MQTT topic configured. So, any information that is received there is saved automatically in each device property. Cool!

The information about topics and payloads, can be usually obtained from the vendor documentation. In Thinger.io, we can even use the device inspector feature, where it is possible to sniff any MQTT packet sent by the device. For example, the captured packet for the relay topic is as the following:

![MQTT message captured from device event inspector](<.gitbook/assets/image (467).png>)

Notice here that publish topic from the device is `shellies/shellyplug-s-0C5F11/relay/0` while the topic used in the configuration is `shellies/`<mark style="color:purple;">`{{device}}`</mark>`/relay/0`. In this case, it is used a placeholder with the <mark style="color:purple;">`device`</mark> name, so it can capture any device identifier, but it is possible to use the standard single-level MQTT wildcard '+' like `shellies/+/relay/0.`

{% hint style="info" %}
`Use topic placeholders like {{device}} or single-level wildcards (+) to capture data from multiple devices.`
{% endhint %}

Moreover, the payload sent by the device to report the relay state is just a raw 'on' or 'off'.  The bytes 111 and 110 are indicating that the relay is 'on'. This payload is not a valid JSON that we can use easily on dashboards or REST APIs, so, we need to convert them to a valid JSON representation, i.e., to a boolean.

This is where Product Scripts come into play. Product scripts will be described further in following sections, but basically, it is possible to write custom Javascript/NodeJS code to process or transform payloads. Taking into account the 'on' 'off' raw payload sent by the device, it is possible to create a function that converts it to a boolean, for example:

```javascript
function toBoolean(value){
    return value == "on" ? true : false; 
}
```

This code can be easily modified inside the Product Script section on the Product Profile.

![Product script example for transforming payloads](<.gitbook/assets/image (466).png>)

To use this function it is only required to reference it when configuring the payload. In this case, it is used `:` to indicate that the payload must be replaced with the contents returned from the function name specified in the following.&#x20;

```javascript
{{payload:toBoolean}}
```

This way, the `toBoolean` function is called with the payload contents, and the final payload will be replaced with the value returned by the function. This configuration must be placed inside the `Payload` configuration on the Property dialog, as shown in the following picture:

![](<.gitbook/assets/image (449).png>)

If your device transmits its payload directly on JSON, then, it is not required to create processing functions, unless you want to customize the fields, change measurement units, etc. In the Shelly device used in the example, the power consumption is sent just as a number, i.e., '6.6', that it is a valid JSON value. So, it will display normally on the device inspector. In this case, the payload does not require processing.

![MQTT message captured from device event inspector (no processing required)](<.gitbook/assets/image (451).png>)

After this configuration is done, any device attached to the product will start to receive updates on its properties automatically. For example, the Shelly product is updating both <mark style="color:purple;">power</mark> and <mark style="color:green;">relay</mark> of a connected device. Moreover, our <mark style="color:green;">relay</mark> property is keeping `true`/`false` after processing its payload with the `toBoolean` function described above.&#x20;

![Example of device properties updated from Product configuration.](<.gitbook/assets/image (454).png>)

### Buckets

A data bucket in Thinger is a way to store time series data, i.e., data measured from devices over time like temperature, power consumption, or any other parameter that can evolve over time. Defining a bucket inside a Product Profile allow:

* Automatic bucket provisioning and configuration
* Automatic bucket ingestion from MQTT topic or IOTMP device resource for devices associated to the product
* Automatic data tagging inside the bucket. Use just one bucket for multiple devices. Each measurement is tagged by device and group, which can be easily filtered when using dashboards or querying data
* Data preprocessing before its insertion, i.e., modify measurement units, filter undesired data, etc.

For the Shelly Plug S device example, we will define 3 different buckets to store some device information, like device status (firmware version, mac, ip, updates), device temperature (and overheating), and total energy and current power consumption. These scenarios are described on the following sections

#### Storing device temperature and overheating

As before, we should rely on the device inspector or the vendor documentation to detect what is the available information and how it is sent by the device.&#x20;

{% hint style="info" %}
Use the device inspector to detect the messages sent by connected MQTT devices
{% endhint %}

In this case, we will use the inspector to detect the messages and its pattern. For example, regarding the device temperature or overheating, each device is publishing to two different topics:

* shellies/shellyplug-s-0CCF6B/temperature
* shellies/shellyplug-s-0CCF6B/overtemperature

![MQTT capture of messages sent by Shelly Plug S regarding temperature and overheating](<.gitbook/assets/image (461).png>)

With this information we could configure one bucket for temperature and one bucket for overheating. One bucket for each publish topic. This configuration is straightforward, and it is quite similar to the power property defined in the previous section.

However, it could be better to just merge the information from both topics in a single bucket, so, related data remains together. This approach can save some resources on data storage and its maintenance.

The regular topic configuration for listening to both of them would be the following:

```
shellies/{{device}}/temperature
shellies/{{device}}/overtemperature
```

In this case, we can merge both topics with the following notation: &#x20;

```
shellies/{{device}}/{{field=temperature|overtemperature}}
```

In this example it is defined a placeholder with the notation \{{<mark style="color:orange;">field</mark>=<mark style="color:blue;">temperature</mark>|<mark style="color:green;">overtemperature</mark>\}} which specifies two measurement fields: <mark style="color:blue;">temperature</mark> and <mark style="color:green;">overtemperature</mark>. This means that the payload from the topics `shellies/{{device}}/temperature` and `shellies/{{device}}/overtemperature` will be stored together in the same bucket, using the corresponding field name. The following image illustrates how a topic configuration works when storing data in a bucket.

![Topic Configuration to bucket data example](<.gitbook/assets/image (464).png>)

The configuration for this bucket will look like the following capture:

![](<.gitbook/assets/image (455).png>)

#### Storing device energy and power

Using the device inspector it is easy to guess how the device transmits the data with the total energy consumption and power.&#x20;

![MQTT capture of messages sent by Shelly Plug S regarding power and energy](<.gitbook/assets/image (470).png>)

Using the notation described in the previous example, it is possible to define a topic to capture both of them and merge them in a single bucket:

```
shellies/{{device}}/relay/0/{{field=energy|power}}
```

In this case, we will introduce a payload processing calling a function that will convert from wat-min to kwH. The payload configuration will be:

```
{{payload:processData}}
```

And the code to process the information can be as the following:

```javascript
function processData(value){
    // convert energy from watt-min to kwH
    value.energy = value.energy/60/1000;
    return value;
}
```

Notice that the function still receives a single parameter that holds both fields captured in the topic configuration, so, the function input is like in the following example:

```javascript
{
    energy: 1386640,
    power: 6.55
}
```

After the processing is done, the data is stored in the bucket with the desired unit (kwH) instead of the default wat-min provided by the vendor.&#x20;

![Data bucket example for storing Shelly Plug S energy and power](<.gitbook/assets/image (450).png>)

It is worth to mention how the protocol implemented by the vendor could support different relays, as the topic contains a `/relay/0`, indicating the relay number 0.&#x20;

```
shellies/{{device}}/relay/0/{{field=energy|power}}
```

This way, this topic could represent different data sources. If we need to process them independently (plot or calculate averages from a single relay), it is possible to define a bucket tag that will hold the relay identifier, following the same notation used with `{{device}}`. This way, the topic configuration can be changed to the following notation:

```
shellies/{{device}}/relay/{{relay}}/{{field=energy|power}}
```

{% hint style="info" %}
Use a placeholder in the topic, i.e., \{{relay\}} to define a bucket tag that identifies or categorize the data source.
{% endhint %}

Using this topic in the configuration will include the relay identifier as a bucket tag, so, it is possible to apply filtering both on device identifier and relay index.

![Data Bucket example with additional 'relay' bucket tag.](<.gitbook/assets/image (471).png>)

{% hint style="warning" %}
Bucket tags can be only defined on bucket creation. Remove the bucket and let the product recreate it again if any tag configuration is modified on the topic.
{% endhint %}

#### Storing device state

Shelly devices auto announce themselves periodically or after requesting them to 'announce' over the `sellies/command` topic. It is possible to see the reported information directly on the inspector. In this case, the device is sending a JSON payload with information like firmware version, identifier, local ip address, mac, or a flag to indicate if there are software updates available. In the following picture there is an example of the event captured on MQTT inspector:

![Shelly announce with information about the device](<.gitbook/assets/image (447).png>)

Storing this information is quite straightforward. It is only required to create a new bucket in product profile, pointing to the following source MQTT topic:

```
shellies/announce
```

Then, store the message payload using:

```
{{payload}}
```

Which will store all the information related to the announcement in the bucket:

![MQTT Data sample captured in a bucket for Shelly Announce information](<.gitbook/assets/image (469).png>)

In addition, it is possible to define a custom payload, i.e., using different field names without using functions. For example, suppose that we want to store only the `new_fw`, and `ip` fields, and change the `new_fw` field to `update`. It is possible to create a JSON using the required placeholders:

```javascript
{
    "update" : {{payload.new_fw}},
    "ip" : "{{payload.ip}}"
}
```

That will store the information according to our definition:

![MQTT Data sample modified before bucket insert](<.gitbook/assets/image (462).png>)

### API Resources

TODO
