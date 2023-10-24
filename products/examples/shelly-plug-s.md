---
description: WiFi Smart Plug with Power Metering
---

# Shelly Plug S

## Product Description

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption><p>Shelly Plug S</p></figcaption></figure>

Shelly Plug S is a WiFi Smart Plug with power metering, that can be easily integrated into the platform. As it supports the MQTT protocol by default, it is not required to re-flash the device, keeping the device warranty.

It can control a wide range of home appliances and office equipment (lights, power lines, security systems, space heaters, radiators, air conditioners, etc.) from anywhere.

* Allows you to manage and power monitor electrical supplies with power up to 2500W (10A)
* Easy control through the Shelly app, or various protocols, platforms, and voice assistants.
* Has an embedded web server and connects to your Wi-Fi network

## Integration

To Integrate Shelly Plug S into the platform it is not required to flash a custom firmware. By default, Shelly PLUG S supports connecting to external MQTT servers.

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption><p>Shelly Plug S Configuration Options</p></figcaption></figure>

Under the "Internet & Security" menu, it is possible to configure "Advanced - Developer Settings". The configuration parameters are the following:

* Username: The username of the Thinger.io account.
* Password: The password for the device
* Server: Thinger.io hostname + port. Example: `acme.aws.thinger.io:1883`
* The device identifier (MQTT client id) can be observed under the Will Topic. In this case, it is "`shellyplug-s-C18B12`"

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption><p>Shelly Plug S - Advanced MQTT server configuration</p></figcaption></figure>

## Product Profile

### Properties

For the Shelly Plug S device example, we will define two different properties to store the latest device state. Shelly Plug S can connect to an MQTT broker, and once connected, it periodically sends information like temperature, relay state, real-time power consumption, available updates, etc. In this example, we will use the current power consumption and the rely on state, so, <mark style="color:purple;">power</mark> and <mark style="color:green;">relay</mark>, are defined as device properties, as shown in the following picture:

![Properties definition for Shelly Plug S SmartPlug](<../../.gitbook/assets/image (582).png>)

Both properties are updated from the following device topics:

* <mark style="color:blue;">power</mark> is updated from topic `shellies/{{device}}/relay/0/power`
* <mark style="color:green;">relay</mark> is updated from topic `shellies/{{device}}/relay/0`

To configure the properties in the product profile, just click  `Add` on the properties section. On the dialog, enter the property identifier (i.e., power or relay), select the `Device Topic` source, fill the `Topic`, and configure the payload, as shown in the following picture:

<div align="left">

<figure><img src="../../.gitbook/assets/image (542).png" alt=""><figcaption><p>Shelly Plug S - Property configuration for storing current power</p></figcaption></figure>

</div>

By default, the payload established on the property is`{{payload}}`, which is a placeholder that will be replaced with the contents received from the configured source. In this case, from the MQTT topic configured. So, any information that is received there is saved automatically in each device property. Cool!

The information about topics and payloads can be usually obtained from vendor documentation. In Thinger.io, we can even use the device inspector feature, where it is possible to sniff any MQTT packet sent by the device. For example, the captured packet for the relay topic is as the following:

![MQTT message captured from device event inspector](<../../.gitbook/assets/image (534).png>)

Notice here that publish topic from the device is `shellies/shellyplug-s-0C5F11/relay/0` while the topic used in the configuration is `shellies/`<mark style="color:purple;">`{{device}}`</mark>`/relay/0`. In this case, it is used a placeholder with the <mark style="color:purple;">`device`</mark> name, so it can capture any device identifier, but it is possible to use the standard single-level MQTT wildcard '+' like `shellies/+/relay/0.`

{% hint style="info" %}
`Use topic placeholders like {{device}} or single-level wildcards (+) to capture data from multiple devices.`
{% endhint %}

Moreover, the payload sent by the device to report the relay state is just a raw 'on' or 'off'.  The bytes 111 and 110 are indicating that the relay is 'on'. This payload is not a valid JSON that we can use easily on dashboards or REST APIs, so, we need to convert them to a valid JSON representation, i.e., to a boolean.

This is where Product Scripts come into play. Product scripts will be described further in the following sections, but basically, it is possible to write custom Javascript/NodeJS code to process or transform payloads. Taking into account the 'on' and 'off' raw payload sent by the device, it is possible to create a function that converts it to a boolean, for example:

```javascript
function toBoolean(value){
    return value == "on" ? true : false; 
}
```

This code can be easily modified inside the Product Script section on the Product Profile.



![Product script example for transforming payloads](<../../.gitbook/assets/image (506).png>)

To use this function it is only required to reference it when configuring the payload. In this case, it is used `:` to indicate that the payload must be replaced with the contents returned from the function name specified in the following.&#x20;

```javascript
{{payload:toBoolean}}
```

This way, the `toBoolean` function is called with the payload contents, and the final payload will be replaced with the value returned by the function. This configuration must be placed inside the `Payload` configuration on the Property dialog, as shown in the following picture:

<figure><img src="../../.gitbook/assets/image (556).png" alt=""><figcaption><p>Shelly Plug S - Property configuration for storing relay state</p></figcaption></figure>

If your device transmits its payload directly on JSON, then, it is not required to create processing functions, unless you want to customize the fields, change measurement units, etc. In the Shelly device used in the example, the power consumption is sent just as a number, i.e., '6.6', it is a valid JSON value. So, it will display normally on the device inspector. In this case, the payload does not require processing.

![MQTT message captured from device event inspector (no processing required)](<../../.gitbook/assets/image (566).png>)

After this configuration is done, any device attached to the product will start to receive updates on its properties automatically. For example, the Shelly product is updating both <mark style="color:purple;">power</mark> and <mark style="color:green;">relay</mark> of a connected device. Moreover, our <mark style="color:green;">relay</mark> property is keeping `true`/`false` after processing its payload with the `toBoolean` function described above.&#x20;

![Example of device properties updated from Product configuration.](<../../.gitbook/assets/image (546).png>)

### Buckets

For the Shelly Plug S device example, we will define 3 different buckets to store some device information, like device status (firmware version, mac, IP, updates), device temperature (and overheating), and total energy and current power consumption. These scenarios are described in the following sections

#### Storing device temperature and overheating

As before, we should rely on the device inspector or the vendor documentation to detect what is the available information and how it is sent by the device.&#x20;

{% hint style="info" %}
Use the device inspector to detect the messages sent by connected MQTT devices
{% endhint %}

In this case, we will use the inspector to detect the messages and their pattern. For example, regarding the device temperature or overheating, each device is publishing on two different topics:

* shellies/shellyplug-s-0CCF6B/temperature
* shellies/shellyplug-s-0CCF6B/overtemperature

![MQTT capture of messages sent by Shelly Plug S regarding temperature and overheating](<../../.gitbook/assets/image (503).png>)

With this information, we could configure one bucket for temperature and one bucket for overheating. One bucket for each publish topic. This configuration is straightforward, and it is quite similar to the power property defined in the previous section.

However, it could be better to just merge the information from both topics in a single bucket, so, related data remain together. This approach can save some resources on data storage and maintenance.

The regular topic configuration for listening to both of them would be the following:

```
shellies/{{device}}/temperature
shellies/{{device}}/overtemperature
```

In this case, we can merge both topics with the following notation: &#x20;

```
shellies/{{device}}/{{field=temperature|overtemperature}}
```

In this example, it is defined a placeholder with the notation \{{<mark style="color:orange;">field</mark>=<mark style="color:blue;">temperature</mark>|<mark style="color:green;">overtemperature</mark>\}} which two specifies measurement fields: <mark style="color:blue;">temperature</mark> and <mark style="color:green;">overtemperature</mark>. This means that the payload from the topics `shellies/{{device}}/temperature` and `shellies/{{device}}/overtemperature` will be stored together in the same bucket, using the corresponding field name. The following image illustrates how a topic configuration works when storing data in a bucket.

![Topic Configuration to bucket data example](<../../.gitbook/assets/image (563).png>)

The configuration for this bucket will look like the following capture:

<figure><img src="../../.gitbook/assets/image (576).png" alt=""><figcaption><p>Shelly Plug S - Bucket Configuration for storing temperature</p></figcaption></figure>

#### Storing device energy and power

Using the device inspector it is easy to guess how the device transmits the data with the total energy consumption and power.&#x20;

![MQTT capture of messages sent by Shelly Plug S regarding power and energy](<../../.gitbook/assets/image (553).png>)

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

![Data bucket example for storing Shelly Plug S energy and power](<../../.gitbook/assets/image (502).png>)

It is worth mentioning how the protocol implemented by the vendor could support different relays, as the topic contains a `/relay/0`, indicating the relay number 0.&#x20;

```
shellies/{{device}}/relay/0/{{field=energy|power}}
```

This way, this topic could represent different data sources. If we need to process them independently (plot or calculate averages from a single relay), it is possible to define a bucket tag that will hold the relay identifier, following the same notation used with `{{device}}`. This way, the topic configuration can be changed to the following notation:

```
shellies/{{device}}/relay/{{relay}}/{{field=energy|power}}
```

{% hint style="info" %}
Use a placeholder in the topic, i.e., \{{relay\}} to define a bucket tag that identifies or categorizes the data source.
{% endhint %}

Using this topic in the configuration will include the relay identifier as a bucket tag, so, it is possible to apply filtering both on the device identifier and relay index.

![Data Bucket example with additional 'relay' bucket tag.](<../../.gitbook/assets/image (537).png>)

{% hint style="warning" %}
Bucket tags can be only defined on bucket creation. Remove the bucket and let the product recreate it again if any tag configuration is modified on the topic.
{% endhint %}

#### Storing device state

Shelly devices auto-announce themselves periodically or after requesting them to 'announce' over the `sellies/command` topic. It is possible to see the reported information directly on the inspector. In this case, the device is sending a JSON payload with information like firmware version, identifier, local IP address, mac, or a flag to indicate if there are software updates available. In the following picture there is an example of the event captured on the MQTT inspector:

![Shelly announce with information about the device](<../../.gitbook/assets/image (507).png>)

Storing this information is quite straightforward. It is only required to create a new bucket in the product profile, pointing to the following source MQTT topic:

```
shellies/announce
```

Then, store the message payload using:

```
{{payload}}
```

Which will store all the information related to the announcement in the bucket:

![MQTT Data sample captured in a bucket for Shelly Announce information](<../../.gitbook/assets/image (524).png>)

In addition, it is possible to define a custom payload, i.e., using different field names without using functions. For example, suppose that we want to store only the `new_fw`, and `ip` fields, and change the `new_fw` field to `update`. It is possible to create a JSON using the required placeholders:

```javascript
{
    "update" : {{payload.new_fw}},
    "ip" : "{{payload.ip}}"
}
```

That will store the information according to our definition:

![MQTT Data sample modified before bucket insert](<../../.gitbook/assets/image (535).png>)

### API Resources

Following our previous examples with the Shelly Smart Plug S, makes sense to create an endpoint to switch on/off the relay. The approach for building the API Resources is quite similar to the previous sections regarding properties and buckets. Just click on the 'Add' button on the API Resources section and start setting an identifier, like `relay`.

<figure><img src="../../.gitbook/assets/image (545).png" alt=""><figcaption><p>Shelly Plug S - API Resource to modify on/off state</p></figcaption></figure>

Then, there are two sections inside the API Resource. The first one is the `Request`, where it is configured the request that will be sent to the device, and the other one is the `Response`, that configures the response that will be sent to the API Rest client that originates the request.

In this case, we want to send a command to the Shelly Device, so, it is required to configure the Request section with `Device Topic` in Target field, as we are interacting with a MQTT device. Then, in the topic field, we can set the following topic, as described in the [vendor documentation](https://shelly-api-docs.shelly.cloud/gen1/#shelly-plug-plugs-overview):&#x20;

```
shellies/{{device}}/relay/0/command
```

Then we must set the message payload, that must be 'on' or 'off' depending on the desired state. If we configure `{{payload}}` as the Request payload, then, any body sent to the API will be transmitted to the device. The standard 'on'/'off' representation in JSON is true/false, so, we can configure a processing function to convert an input JSON boolean to the required values by the device. In the Product Script section can be coded a function function like this:

```javascript
function toOnOff(value){
    return value ? "on" : "off";
}
```

As described on [Properties](../product-profile/properties.md) and [Buckets](broken-reference) sections, it is possible to configure the API Resource payload to call this function to transform the incoming payload.

```
{{payload:toOnOff}}
```

After saving the new API Resource, we can go to the device API, and the new `relay`resource will appear.

![Device API for resource created over Product ](<../../.gitbook/assets/image (580).png>)

However, there seems to be something wrong there! The API can detect that the resource is expecting an input (`Resource Input` header is present), but nothing more appears. We are expecting here a true/false input, or just a switch to turn the relay on and off.&#x20;

This can be solved by assigning a default payload value on the API definition, which let the API explorer know the expected values by default. This can be done changing the API Resource payload to something like this:

```
{{payload:toOnOff=false}}
```

Now, refreshing the API explorer will display the relay resource expecting a boolean that can be easily changed over the GUI. At this moment, it is possible to turn on and off the switch button. It will call the API Resource on the product, that will convert the incoming payload to 'on' and 'off', and then will be transmitted to the required topic for the Shelly Plug S device. Cool! :sunglasses:

![Device API for resource created over Product ](<../../.gitbook/assets/image (560).png>)

However, this use case can be improved a little bit. For example, suppose that it is required to display the current relay state to avoid showing always the default 'false' value set before. For this purpose we can reference other properties, or even API responses. To reference a property, i.e., the 'relay' one configured on the [Properties](../product-profile/properties.md) section, we can set a default value with `=property.<property_name>` like in the following example:

```
{{payload:toOnOff=property.relay}}
```

Then, the next time the API explorer, or a dashboard, 'inspect' the device resource, will obtain the value stored on the property. In this case, the property is updated periodically with the current relay status, or even if the status is changed with the device psychical on/off button. Now we can use the API resource on a dashboard easily, just as any other device. Here are some sample dashboard widgets created for our MQTT Shelly Plug S device, allowing power switch and plotting some device information like real-time power consumption, total consumption, temperature, etc.&#x20;

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption><p>Shelly Plug S - Dashboard</p></figcaption></figure>
