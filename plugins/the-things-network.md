---
description: Plugin for automatically integrating TTN devices in Thinger.io platform.
---

# The Things Network Plugin

![](../.gitbook/assets/image%20%283%29.png)

The Things Network \(TTN\) plugin is a solution for using The Things Network HTTP integration in an optimized way, providing features like both uplink and downlink processing, or automatic device and data storage provisioning, so it is not required to configure TTN devices in both places. 

{% hint style="info" %}
[Note: Plugins are only available for premium Thinger.io servers. Check **this link** to create your own instance within minutes](https://pricing.thinger.io)
{% endhint %}

## Plugin Features

* [x] Automatic device and bucket provisioning for new devices included in a TTN application. It is no required additional configuration to start working with TTN data inside Thinger.io. 
* [x] Store device metadata based on TTN information, like device location, signal quality, hardware serial, etc. 
* [x] Store device data automatically in data buckets so it can be easily used from the console.
* [x] Provide support por defining custom uplink callbacks on NodeJS  to process `payload_raw` or `payload_fields` provided by the TNT Integration.
* [x] Provide support for defining custom downlink callbacks on NodeJS, so it is possible to configure donwlink data in an user-friendly format \(JSON\), and then convert it to `payload_raw` or `payload_fields`, as required by TTN network.

## TTN Concepts

For a better understanding of the following sections, here is described some basic TTN concepts:

* Device: It is a hardware device with a LoRa interface.
* Gateway: It is a hardware device with both LoRa and Internet connectivity. It basically receives LoRa messages from multiple devices, and push them to TTN network over the Internet.
* Uplink: It is a data flow which represents messages sent from a device to the TTN network
* Downlink: It is a data flow which represents messages sent from the TTN network to a device.
* Application: It is a concept that defines a group of devices of the same type, normally sending the same kind of data both in uplink and downlink\). 
* Integration: It is a way of push device data outside TTN network, i.e., sending messages to other platforms like Thinger.io.

## Plugin Configuration

In this section it is described the different interfaces that can be used to configure the TTN plugin.

### Applications

Every TTN application that is integrated over this plugin, should define a new application with the same application identifier as defined in TTN. Each application defined in this way will allow to customize the plugin behaviour based on the application type.

![TTN aplication configuration ](../.gitbook/assets/image%20%2843%29.png)

It is possible to create as many applications as required. To configure a particular application, just select the application id from the `Application Id` dropdown, and then navigate to the other plugin sections.

{% hint style="warning" %}
Always create applications with the same application identifier as defined in TTN.
{% endhint %}

### Uplink Behaviour

The uplink behaviour allows to configure how the plugin will react on new information received from TTN. 

![Uplink behaviour configuration](../.gitbook/assets/captura-de-pantalla-2019-10-02-a-las-10.58.12.png)

The configurable parameters are the following:

* **Auto provision resources:** Enable or disable automatic resource provisioning while receiving messages for non created devices.
* **Device connection timeout:** When creating a new device, establish the device connection timeout in minutes, so the platform can consider the device as disconnected after a fixed time without receiving a message. 
* **Device identifier prefix:** When creating a new device, create it with a custom prefix + the original device id.
* **Bucket identifier prefix:** When creating a new data bucket associated to the device, create it with a custom prefix + the original device id.
* **Update device location:** Use the location provided in the gateways information to update de current device location.
* **Save metadata:** Store the metadata information provided by TTN as a device property that can be retrieved later.
* **Initialize downlink data:** When creating a new device, initialize a custom downlink data, that can be modified and processed in further downlink requests.

### Payload Processing

In this section it is possible to configure the payload processors that will transformate the payload received from TTN in an uplink message, or the payload being sent to TTN in a downlink message.

The interface provides a code editor for NodeJS, where it is possible to define the `uplink` and `downlink`processors. It is also possible to test the code by providing a sample input data both for `uplink` and `downlink`.

![TTN Payload processing configuration](../.gitbook/assets/image%20%2857%29.png)

In the following, there is information about the uplink and downlink methods.

{% tabs %}
{% tab title="Uplink" %}
The uplink method will be called after a gateway sends a new message over the TTN network. Depending on the configuration done in the TTN application, this function will receive different inputs:

* **Base64 String**: If the TTN application defines `Custom`payload format but does not provide a `decoder` function, this method will receive the raw payload encoded in base64. In this case, it will be necessary to write a function to transform this base64 data to a JSON object.
* **JSON Object from Cayene LPP**: If the TTN application defines a `Cayene LPP` payload format, TTN will automatically convert the binary data to a JSON object that can be used directly by the platform. In this case, it is not necessary to define custom uplink method unless you want to do some extra processing like incorporate calculated fields.
* **JSON Object from custom decoder:** If the TTN application defines `Custom`payload format and provides a `decoder` function, this function will receive the output from TTN function. In this case, creating a custom uplink method will be redundant, so create the funtion in TTN, or in the plugin.

The output of this method must be always a JSON object containing the information that is necessary to be used by the platform. In the following, there is an uplink method that converts base64 data into a JSON object with `temperature` and `humidity` parsed from the binary data.

```javascript
/* convert a base64 payload to a JSON object that can be used 
   by Thinger.io */
module.exports.uplink = function(payload){
    const buffer = Buffer.from(payload, 'base64');
    let processed = {};
    processed.temperature = buffer.readInt16LE(0)/100.0;
    processed.humidity = buffer.readInt16LE(2)/100.0;
    return processed;
};
```

{% hint style="success" %}
The uplink method must always return a JSON object.
{% endhint %}
{% endtab %}

{% tab title="Downlink" %}
The downlink method will be called before the plugin issues a downlink request to TTN. To issue a downlink request to TTN, this plugin must receive an HTTP POST call, indicating the Thinger.io device identifier, and it will automatically issue the request to the required TTN endpoint and its specific protocol. Check out the next sections for more details.

The This function will receive different inputs depending on how the plugin is called over its REST API.

* **JSON Object**: If the downlink call is done for a Thinger.io device that defines a `downlink` property \(that is automatically initialized if `Initialize Downlink Data` is configured in the plugin\), this method will receive the JSON content of this property. It usually consists on a user-friendly device configuration that should be later encoded to binary in base64.
* **JSON Object**: If the plugin downlink request contains a JSON payload in the POST call, this function will receive this payload instead of the one configured in the device `downlink` property. 

The output of this method should be one of the following:

* **Base64 String**: With binary information that can be sent directly to the TTN network. It is required if your TTN application is not defining a `converter`.
* **JSON Object**: If the TTN application provides a `converter`for your payloads, this method can return a JSON object that will be accesible in the `converter` method. In this case, creating a custom downlink method will be redundant, so create the funtion in TTN, or in the plugin.

Example of a downlink method converting a JSON device configuration into base64 as required by TTN:

```javascript
/* convert a JSON object with the device configuration in a base64
   string expected by TTN */
module.exports.downlink = function(payload){
    let bytes = [];
    bytes[0] = payload.enabled ? 1 : 0;
    bytes[1] = payload.frequency;
    bytes[2] = payload.threshold;
    return Buffer.from(bytes).toString('base64');
};
```

{% hint style="success" %}
The downlink method should return a base64 string if the TTN application does not define a converter.
{% endhint %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Use the interface tester to see if your code is correctly procesing the payloads. 
{% endhint %}

### TTN Uplink Integration

### TTN Downlink Integration

## Plugin Development Details

![](../.gitbook/assets/image%20%28181%29.png)[Github source code](https://github.com/thinger-io/ttn-plugin)

### Uplink Data Flow

In this section it is described how the uplink data flow works, from its source in the TTN network, to its final destination in Thinger.io.

![](../.gitbook/assets/image%20%2847%29.png)

In the following subsections are described the elements shown in the figure.

#### TTN Uplink Callback

When TTN receives a message from a given gateway, it automatically checks its configured integrations to forward them the information received. This plugin is integrated over HTTP, so, the TTN network will issue an HTTP request to the Thinger.io plugin on new messages.

#### TTN Plugin

The Thinger.io plugin receives data from the TTN network in a JSON format. The callback includes several fields of information, like `app_id`, `dev_id`, `donwlink_url`, `metadata`, or the actual payload information sent by LoRa devices on `payload_fields` or `payload_raw` fields, depending on the Payload formats configured in the TTN application.

Here is an example of the raw information received by the plugin:

```javascript
{
   "app_id":"jfmateos_thingerior",
   "counter":2845.0,
   "dev_id":"jfmateos_thingerio_01",
   "downlink_url":"https://integrations.thethingsnetwork.org/ttn-eu/api/v2/down/jfmateos_thingerior/thinger?key=ttn-account-v2.KAKwU66ozqojOX8ygDZoc4QZB4Is7M2kSqVlaGepubI",
   "hardware_serial":"0045687763982140",
   "metadata":{
      "coding_rate":"4/5",
      "data_rate":"SF7BW125",
      "frequency":868.1,
      "gateways":[
         {
            "channel":0.0,
            "gtw_id":"eui-dc4f22ffff583935",
            "latitude":40.41237,
            "longitude":-3.71809,
            "rf_chain":0.0,
            "rssi":-25.0,
            "snr":9.0,
            "time":null,
            "timestamp":2957948451.0
         }
      ],
      "modulation":"LORA",
      "time":"2019-03-15T12:50:22.080690209Z"
   },
   "payload_fields":{
      "analog_in_3":3.3
   },
   "payload_raw":"AwIBSg==",
   "port":1.0
}
```

{% hint style="info" %}
More information about TTN HTTP Integration [here](https://www.thethingsnetwork.org/docs/applications/http).
{% endhint %}

Once this information is received by the plugin, it is processed in order to execute the following actions in Thinger.io:

1. Auto provision new device and its associated data bucket if the device does not exists on the platform. It is based on the `dev_id` field. 
2. Store updated device information in a device property called `device`, It saves information like `app_id`, `dev_id`, `hardware_serial`, `port`, `counter`, and `downlink_url`.
3. Store other device metadata as configured in the plugin, i.e, updating device location based on gateway location, or saving the `metadata` field for further analysis.
4. Use the configured uplink processor \(if any\) to transform the information provided from TTN network in the fields `payload_raw` or `payload_fields` before its use in the platform.
5. Call device callback that will actually push processed data to its associated data bucket, but could do any other action like forwarding data to other endpoints.

#### TTN Device information

This plugin stores some basic information about the device that is sending data. This information is mainly used for keeping some information necessary for the downlink, like the original device identifier used in TTN; the associated application identifier to handle its downlink processor properly; and the most important, the downlink url the platform should use to issue a downlink request to the TTN network.

![Sample device information stored in Thinger.io for every TTN device](../.gitbook/assets/image%20%2841%29.png)

#### Uplink Processor

This plugin allows to configure custom code for processing incoming data. The information sent by LoRaWan devices is normally encoded in small binary payloads that cannot be directly used for representation, as they should not contain tags, JSON, ascii text, etc., in order to minimize transmission time. So, it is required to process the data sent by devices in some point of the cloud.

{% hint style="info" %}
[Information aobut LoRaWan limitations](https://www.thethingsnetwork.org/docs/lorawan/limitations.html)
{% endhint %}

TTN is already able to process binary payload from devices and convert it in something representative by using a `decoder`, that is a custom function that can be configured for each application. It can also parse data encoded by the Cayenne LPP binary format easily.

This plugin also allows to create custom decoders if necessary. The advantage of using the Thinger.io payload processing \(if necessary\), is that it is using NodeJS runtime instead of plain Javascript, so it is possible to use NodeJS modules like Buffer, that simplifies the condig of the processing functions.

Internaly, payload processors are precompiled after its configuration in the plugin, and executed with the payload data received from TTN. The output from this function \(if it gets executed\), is then transmitted to the next final step, which is the device callback.

#### Device Callback

The last step of this plugin is to call the device callback in Thinger.io. This plugin auto provision new TTN devices as HTTP devices. HTTP devices inside Thinger.io are generic devices that can push data over REST API methods. Thinger.io will be responsable of taking input data and perform different configurable actions with it, like change the device state to connected/disconnected; write provided data to a configured data bucket; send this information to other services over an endpoint; store the provided information as a device property; or return data from one of the device properties.

In this case, the plugin interacts with the platform over such REST interface, pushing data received from TTN, and processed by the custom uplink method. By default, the plugin initializes an HTTP device to write to a data bucket that is also automatically created. So, every message sent by a TTN device, will write finally write to a specific data bucket. As shown in the following picture:

![HTTP Device created by the TTN Plugin](../.gitbook/assets/image%20%28121%29.png)

After the device callback is done, it will appear as a connected device, showing also its location if it was configured in the plugin options.

### Downlink Data Flow

![The Things Network Downlink Flow Overview](../.gitbook/assets/image%20%28118%29.png)

