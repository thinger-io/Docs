# MQTT CLIENTS

## Introduccion to MQTT

MQTT is a M2M communication protocol that has become very popular for IoT purposes due to its simplicity and lightness. It uses a pub-sub messaging paradigm in which the devices, also called **"clients"** maintain a TCP/IP connection with a server, being able to perform two different types of communication:&#x20;

* **Publish** messages with an identificator that is called "topic". In this way, the devices can send data to the server.&#x20;
* **Subscribe** to an specific topic, to receive data from the server.&#x20;

The server, also called **"broker"**, keeps a register of all connected devices and their pub-sub type, allowing fast and efficient data transmission to all subscribed devices when it arrives to an specifics topic in an asynchronous way.&#x20;

## Integration of MQTT devices&#x20;

Thinger.io Platform private instances has been provided of a basic MQTT broker, that can be used to integrate devices with this protocol in a very simple way, allowing publish and subscribe communications, in the next sections it is explained how to work with both, and how to work with it's data.

The process is carried out in two parts, on the one hand, the definition of the device at Thinger.io Platform Server that will acts as Broker, and on the other hand, the configuration of an MQTT Client that will create the communication with the server:

{% hint style="danger" %}
Note that this feature is not available on Thinger.io platform freemium accounts. So to test this integration it is necessary to deploy an on-premise or cloud private instance. &#x20;
{% endhint %}

### Create device at Thinger.io &#x20;

Creating the device on Thinger.io is done like any other resource on the platform, juts accessing the device list, available in the "devices" section of the main menu and clicking on the "Add device" button:&#x20;

![](<.gitbook/assets/image (343).png>)

Then the "new device form" will appear, allowing to introduce device information:

![](<.gitbook/assets/image (474).png>)

* **Device Type**: MQTT device should be selected
* **Device identifier:** Must be unique within your devices
* **Device description:** Additional information that may help to identify each device
* **Device credentials:** Is the device security password, it can be random created using the bottom button.

When all the information has been introduced, pressing "Add Device" button will create a new device profile in the device list. If everything is right a confirmation message will appear, meaning that Thinger.io Platform is ready to receive data from your MQTT devices. &#x20;

### Configure MQTT client to connect with Thinger.io&#x20;

The second part of the integration is configure any device or program as a client to connect Thinger.io and start Publishing or subscribing data. In any case, the following parameters must be introduced in the client in order to create the connection:&#x20;

* **Broker Address**: The Thinger.io instance domain (i.e. acme.aws.thinger.io)&#x20;
* **Broker Port**: 1883 or 8883 for SSL/TLS
* **User Name**: Thinger.io user account ID
* **Client ID**: The device identifier that was configured at the device form
* **Password**: Must be the same key that was placed on Thinger.io "Device Credentials" parameter
* **MQTT version**: Currently Thinger.io supports 3.1 or 3.1.1 versions of the protocol

![example using MQTT.fx client](<.gitbook/assets/image (276).png>)

{% hint style="info" %}
It is recommended to use SSL/TLS communication using the port 8883
{% endhint %}

Once the client has been configured, it should be able to publish and subscribe data with the server, for example, using the `mosquit_pub` client:

```
mosquitto_pub -d -h trincado.do.thinger.io -p 1883 -i MQTT -u jt -P testCredential -t telemetry -m '{"temperature":5}'  

Client MQTT sending CONNECT
Client MQTT received CONNACK (0)
Client MQTT sending PUBLISH (d0, q0, r0, m1, 'telemetry', ... (17 bytes))
Client MQTT sending DISCONNECT
```

Where `MQTT` is the  Client ID, `jt` is the user account, and `testCredentials` is the device password.&#x20;

Note that Thinger.io MQTT broker has been designed to support multi-tenancy by default. It supports multiple clients / organisations to use the same broker without overlapping topics.

## Working with MQTT data

{% hint style="warning" %}
As MQTT broker is **not creating a Device API**, it won't possible to work with its data in real-time with Endpoints or Dashboards until we develop an adaptation of these features. However, it is possible to store data in Buckets and configure Dashboard widgets to read from that bucket.&#x20;
{% endhint %}

### Storing data in buckets

Thinger.io data buckets are a virtual storage where any kind of time series data can be saved, this information can be used to be plotted in dashboards, or to be exported in different formats for offline processing.\
\
Configure a data bucket to store data from an specific MQTT topic just requires going to bucket section of the main menu and pressing the "Add Bucket" button to access the "new bucket form" in which introduce the topic configuration, as has been made in the image below: &#x20;

![](<.gitbook/assets/image (494).png>)

The next parameters needs to be configured:&#x20;

* **Bucket Id**: Unique identifier for the bucket.&#x20;
* **Bucket name**: Use a representative name to remember the bucket scope, like `WeatherData`.
* **Bucket description**: Fill here any description with more details, like Temperature and humidity in house.
* **Enabled**: Data bucket recording can be enabled or disabled. Just switch it on to enable it.
* **Data Source**: Commonly this defines the Thinger.io device or resource that will be subscribed by the server. In this situation "From MQTT Topic" must be placed
* **MQTT Topic**: place here the MQTT topic that will be subscribed by the server&#x20;

This way, Thinger.io Platform server will be configured as MQTT broker but also as a topic consumer in order to provide additional features. Then, the client must be configured to send data in JSON format.

{% hint style="success" %}
Use **JSON** as the payload type for the device messages stored by buckets.
{% endhint %}

### Showing data in Dashboards

Now that the MQTT data is being stored into the data bucket, it is possible to show it on dashboards, where multiple widgets can be used to create real-time or historical representations by selecting the bucket as the data source as it is shown in the image below:&#x20;

![](<.gitbook/assets/image (457).png>)

Dashboard widgets can show data from different devices, and been configured to create flexible data representations as we have explained in the [**dashboard section of this documentation**](features/dashboards.md).&#x20;

![](<.gitbook/assets/image (444).png>)

### Processing data with Node-RED Plugin&#x20;





