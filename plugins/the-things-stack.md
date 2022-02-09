# The Things Stack



![](<../.gitbook/assets/image (4).png>)

The Things Network is a LoRaWAN Network solution that simplifies the deployment of large IoT applications over a collaborative Internet of Things network that spans many countries around the world. From thinger.io we wanted to offer an improved integration to TTN users by providing easy to configure tools for the storage, analyze and show devices data in a simple way. This plugin allows to retrieve TTN webhook messages to enhance the integration with some intereting features such as:&#x20;

* Uplink data payload processing&#x20;
* Downlink data payload processing
* automatic device and data buckets provisioning.&#x20;

{% hint style="info" %}
[Note: Plugins are only available for premium Thinger.io servers. Check **this link** to create your own privated IoT instance within minutes](https://pricing.thinger.io)
{% endhint %}

## Plugin Features

* [x] Automatic device and bucket provisioning for new devices included in a TTN application. It is no required additional configuration to start working with TTN data inside Thinger.io.&#x20;
* [x] Store device metadata based on TTN information, like device location, signal quality, hardware serial, etc.&#x20;
* [x] Store device data automatically in data buckets so it can be easily used from the console.
* [x] Provide support por defining custom uplink callbacks on NodeJS  to process `payload_raw` or `payload_fields` provided by the TNT Integration.
* [x] Provide support for defining custom downlink callbacks on NodeJS, so it is possible to configure donwlink data in an user-friendly format (JSON), and then convert it to `payload_raw` or `payload_fields`, as required by TTN network.

## TTN Concepts

For a better understanding of the following sections, here is described some basic TTN concepts:

* **Device**: It is a hardware device with a LoRa interface.
* **Gateway**: It is a hardware device with both LoRa and Internet connectivity. It basically receives LoRa messages from multiple devices, and push them to TTN network over the Internet.
* **Uplink**: It is a data flow which represents messages sent from a device to the TTN network
* **Downlink**: It is a data flow which represents messages sent from the TTN network to a device.
* **Application**: It is a concept that defines a group of devices of the same type, normally sending the same kind of data both in uplink and downlink).&#x20;
* **Integration**: It is a way of push device data outside TTN network, i.e., sending messages to other platforms like Thinger.io.

## Plugin Configuration

This section describes the different interfaces that can be used to configure the TTN plugin.

### Integrating TTN Applications

El primer paso para realizar una integración es crear una nueva configuración del plugin. Es posible crear multiples perfiles de configuración with custom behavior para cada applicación desplegada en TTN Stack. To create a new application profile, just write the Application ID and press the `Add Application` green button. Note that the ID must be exactly the same identifier as defined in TTN application.

![](<../.gitbook/assets/image (407).png>)

The `Application Id` dropdown allows to select and configure a particular application profile, but if the "default" profile is selected, the configuration will be applied to all the applications integrated with the plugin.

{% hint style="warning" %}
Always create applications with the same application identifier as defined in TTN.
{% endhint %}

### Uplink Settings

![](<../.gitbook/assets/image (405).png>)

As shown in the image above, the parameters can be used to configure the plugin behavior:

* **Auto provision resources:** Enable or disable automatic resource provisioning while receiving messages for non-created devices.
* **Device connection timeout:** When creating a new device, establish the device connection timeout in minutes, so the platform can consider the device as disconnected after a fixed time without receiving a message.&#x20;
* **Device identifier prefix:** When creating a new device, create it with a custom prefix + the original device id.
* **Bucket identifier prefix:** When creating a new data bucket associated to the device, create it with a custom prefix + the original device id.
* **Update device location:** Use the location provided in the gateways information to update de current device location.
* **Save metadata:** Store the metadata information provided by TTN as a device property that can be retrieved later.
* **Initialize downlink data:** When creating a new device, initialize a custom downlink data, that can be modified and processed in further downlink requests.

### Downlink Settings

TTN stack Downlink processes can be configured from Thinger.io in order to select the behavior in some parameters as shown below:

![](<../.gitbook/assets/image (403).png>)

* **Confirmed downlink:** Set to enabled if downlink messages must be confirmed by the device.
* **Push To Downlink Queue:** Enable to push downlink messages instead of replace previous ones.
* **Downlink priority:** Specify the downlink priority

### Payload processing

This tab allows configuring the payload data treatment in order to will transform from binary payload received from TTN webhook into user-friendly variables and the Downlink JSON into a binary buffer that will be transmitted to TTN Stack.

The interface provides a code editor for Node.js scripts, where it is possible to define the codification / decodification processes and also provides a testing tool that allows verifying the behavior of both  `uplink` and `downlink` processes.

![](<../.gitbook/assets/image (410).png>)

The following sections provide additional information about how to configure the uplink and downlink methods.

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

* **JSON Object**: If the downlink call is done for a Thinger.io device that defines a `downlink` property (that is automatically initialized if `Initialize Downlink Data` is configured in the plugin), this method will receive the JSON content of this property. It usually consists on a user-friendly device configuration that should be later encoded to binary in base64.
* **JSON Object**: If the plugin downlink request contains a JSON payload in the POST call, this function will receive this payload instead of the one configured in the device `downlink` property.&#x20;

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

## TTN Console configuration

### Uplink Configuration&#x20;

The last tab of the plugin configuration interface is called "Webhook settings", it has been created to help the developers to complete the integration in TTN Console, by providing all the information required to setup the webhook profile.&#x20;

![](<../.gitbook/assets/image (398).png>)

{% hint style="info" %}
Note that the REST API does not define the application ID, this parameter will be checked by the plugin software to manage the payload according to the configuration entered in the
{% endhint %}

To create a new webhook integration follow the next steps in the TTN Stack web console:

1- Select the Application to be integrated\
2- In the main menu open the "Integrations" section and clinc the "Webhooks" option. The webhooks list will be shown.

![](<../.gitbook/assets/image (404).png>)

3- Clicking the `+Add webhook` blue button in the right top corner of the interface allows choosing between different webhooks integration templates. Thinger.io has not a template yet so"custom webhook option must be selected. Then, configure the webhook only requires filling the form with the information provided by Thinger.io "webhook settings" tab and selecting JSON webhook format.

![](<../.gitbook/assets/image (409).png>)

{% hint style="info" %}
note that the Authorization header must be setted up using the access token includding "Bearer" command
{% endhint %}

### Downlink Configuration

To configure the downlink behavior, it is only necessary to add a "Download API Key" in the Webhook configuration as explained below:

1- Going to the TTN Stack application menu, click over the "API Keys" section. A list with the previously created API Keys will be displayed. \
2-  Clicking the `+Add API Key`blue button on the right top corner of the interface allows to configure a new API Key with specific permissions.\
3- Select the individual   "Write downlink application traffic" option and copy the new API key

![](<../.gitbook/assets/image (408).png>)

Finally, paste this string into the "downlink API Key" input of the Endpoint Settings section of the webhook configuration form:

![](<../.gitbook/assets/image (411).png>)

## Executing downlink processes&#x20;

Thinger.io's TTN plugin has been prepared to automatically manage the sending of downlink messages to the TTN server, this system takes the data from the device\_downlink property, autogenerated during device provisioning and inserts it as a response to the next HTTP request from the system. In this way we can create device configuration and controlling processes just modifying the value of this property by means of a Dashboard widget, Node-RED, or direct API integration.

To create a new downlink process make sure to follow the next steps:

[1- Configure the plugin downlink settings at Thinger.io](https://docs.thinger.io/plugins/the-things-stack#downlink-settings)\
2- Write a codification script if required using the plugin "downlink payload processing" \
3[- Create an API Key and introduce it in the TTN Stack Webhook configuration](https://docs.thinger.io/plugins/the-things-stack#downlink-settings)\
4- Modify the value of the property to launch the execution of the downlink process. &#x20;

After this, the plugin will execute the payload processing and send it in response of the next TTN API request to your server. It is possible to follow the trace of this communication by accessing the plugin's log going to Plugins>Plugin profile>log.
