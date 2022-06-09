# Buckets

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

![MQTT capture of messages sent by Shelly Plug S regarding temperature and overheating](<../.gitbook/assets/image (461).png>)

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

![Topic Configuration to bucket data example](<../.gitbook/assets/image (464).png>)

The configuration for this bucket will look like the following capture:

![](<../.gitbook/assets/image (455) (1).png>)

#### Storing device energy and power

Using the device inspector it is easy to guess how the device transmits the data with the total energy consumption and power.&#x20;

![MQTT capture of messages sent by Shelly Plug S regarding power and energy](<../.gitbook/assets/image (470).png>)

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

![Data bucket example for storing Shelly Plug S energy and power](<../.gitbook/assets/image (450).png>)

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

![Data Bucket example with additional 'relay' bucket tag.](<../.gitbook/assets/image (471).png>)

{% hint style="warning" %}
Bucket tags can be only defined on bucket creation. Remove the bucket and let the product recreate it again if any tag configuration is modified on the topic.
{% endhint %}

#### Storing device state

Shelly devices auto announce themselves periodically or after requesting them to 'announce' over the `sellies/command` topic. It is possible to see the reported information directly on the inspector. In this case, the device is sending a JSON payload with information like firmware version, identifier, local ip address, mac, or a flag to indicate if there are software updates available. In the following picture there is an example of the event captured on MQTT inspector:

![Shelly announce with information about the device](<../.gitbook/assets/image (447).png>)

Storing this information is quite straightforward. It is only required to create a new bucket in product profile, pointing to the following source MQTT topic:

```
shellies/announce
```

Then, store the message payload using:

```
{{payload}}
```

Which will store all the information related to the announcement in the bucket:

![MQTT Data sample captured in a bucket for Shelly Announce information](<../.gitbook/assets/image (469).png>)

In addition, it is possible to define a custom payload, i.e., using different field names without using functions. For example, suppose that we want to store only the `new_fw`, and `ip` fields, and change the `new_fw` field to `update`. It is possible to create a JSON using the required placeholders:

```javascript
{
    "update" : {{payload.new_fw}},
    "ip" : "{{payload.ip}}"
}
```

That will store the information according to our definition:

![MQTT Data sample modified before bucket insert](<../.gitbook/assets/image (462) (1) (1).png>)

###
