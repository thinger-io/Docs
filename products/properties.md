# Properties

A property in a device is a way to store arbitrary data, i.e., store device configuration, latest device state, or any other data required for the use case. Defining a property inside a Product Profile allow:

* Automatic device property update from MQTT topic or IOTMP device resource for devices associated to the product
* Setting a default value for devices that did not updated the value yet
* Data preprocessing before its use for the property, i.e., modify measurement units, filter undesired data, etc.

For the Shelly Plug S device example, we will define two different properties to store the latest device state. Shelly Plug S can connect to a MQTT broker, and once connected, it periodically send information like temperature, relay state, real-time power consumption, available updates, etc. In this example, we will use the current power consumption and the rely sate, so, <mark style="color:purple;">power</mark> and <mark style="color:green;">relay</mark>, are defined as device properties, as shown in the following picture:

![Properties definition for Shelly Plug S SmartPlug](<../.gitbook/assets/image (452).png>)

Both properties are updated from the following device topics:

* <mark style="color:blue;">power</mark> is updated from topic `shellies/{{device}}/relay/0/power`
* <mark style="color:green;">relay</mark> is updated from topic `shellies/{{device}}/relay/0`

To configure the properties in the product profile, just click on `Add` on the properties section. On the dialog, enter the property identifier (i.e., power or relay), select the `Device Topic` source, fill the `Topic`, and configure the payload, as shown in the following picture:

![](<../.gitbook/assets/image (463).png>)

By default, the payload established on the property is`{{payload}}`, which is a placeholder that will be replaced with the contents received from the configured source. In this case, from the MQTT topic configured. So, any information that is received there is saved automatically in each device property. Cool!

The information about topics and payloads, can be usually obtained from the vendor documentation. In Thinger.io, we can even use the device inspector feature, where it is possible to sniff any MQTT packet sent by the device. For example, the captured packet for the relay topic is as the following:

![MQTT message captured from device event inspector](<../.gitbook/assets/image (467).png>)

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

![Product script example for transforming payloads](<../.gitbook/assets/image (466).png>)

To use this function it is only required to reference it when configuring the payload. In this case, it is used `:` to indicate that the payload must be replaced with the contents returned from the function name specified in the following.&#x20;

```javascript
{{payload:toBoolean}}
```

This way, the `toBoolean` function is called with the payload contents, and the final payload will be replaced with the value returned by the function. This configuration must be placed inside the `Payload` configuration on the Property dialog, as shown in the following picture:

![](<../.gitbook/assets/image (449).png>)

If your device transmits its payload directly on JSON, then, it is not required to create processing functions, unless you want to customize the fields, change measurement units, etc. In the Shelly device used in the example, the power consumption is sent just as a number, i.e., '6.6', that it is a valid JSON value. So, it will display normally on the device inspector. In this case, the payload does not require processing.

![MQTT message captured from device event inspector (no processing required)](<../.gitbook/assets/image (451).png>)

After this configuration is done, any device attached to the product will start to receive updates on its properties automatically. For example, the Shelly product is updating both <mark style="color:purple;">power</mark> and <mark style="color:green;">relay</mark> of a connected device. Moreover, our <mark style="color:green;">relay</mark> property is keeping `true`/`false` after processing its payload with the `toBoolean` function described above.&#x20;

![Example of device properties updated from Product configuration.](<../.gitbook/assets/image (454) (1).png>)

###
