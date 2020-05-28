# MQTT CLIENTS

## Introduccion to MQTT

MQTT is a M2M communication protocol that has become very popular for IoT purposes due to its simplicity and lightness. It uses a pub-sub messaging paradigm in which the devices, also called **"clients"** maintain a TCP/IP connection with a server, being able to perform two different types of communication: 

* **Publish** messages with an identificator that is called "topic". In this way, the devices can send data to the server. 
* **Subscribe** to an specific topic, to receive data from the server. 

The server, also called **"broker"**, keeps a register of all connected devices and their pub-sub type, allowing fast and efficient data transmission to all subscribed devices when it arrives to an specifics topic in an asynchronous way. 

## Integration of MQTT devices 

Thinger.io Platform private instances has been provided of a basic MQTT broker, that can be used to integrate devices with this protocol in a very simple way, allowing publish and subscribe communications, in the next sections it is explained how to work with both, and how to work with it's data.

The process is carried out in two parts, on the one hand, the definition of the device at Thinger.io Platform Server that will acts as Broker, and on the other hand, the configuration of an MQTT Client that will create the communication with the server:

{% hint style="danger" %}
Note that this feature is not available on Thinger.io platform freemium accounts. So to test this integration it is necessary to deploy an on-premise or cloud private instance.  
{% endhint %}

### Create device at Thinger.io  

Creating the device on Thinger.io is done like any other resource on the platform, juts accessing the device list, available in the "devices" section of the main menu and clicking on the "Add device" button: 

![](../../.gitbook/assets/image%20%28138%29.png)

Then the "new device form" will appear, allowing to introduce device information:

![](../../.gitbook/assets/image%20%28119%29.png)

* **Device Type**: MQTT device should be selected
* **Device identifier:** Must be unique within your devices
* **Device description:** Additional information that may help to identify each device
* **Device credentials:** Is the device security password, it can be random created using the bottom button.

When all the information has been introduced, pressing "Add Device" button will create a new device profile in the device list. If everything is right a confirmation message will appear, meaning that Thinger.io Platform is ready to receive data from your MQTT devices.  

### Configure MQTT client to connect with Thinger.io 

The second part of the integration is configure any device or program as a client to connect Thinger.io and start Publishing or subscribing data. In any case, the following parameters must be introduced in the client in order to create the connection: 

* **Broker Address**: The Thinger.io instance domain \(i.e. acme.aws.thinger.io\) 
* **Broker Port**: 1883 or 8883 for SSL/TLS
* **User Name**: Thinger.io user account ID
* **Client ID**: The device identifier that was configured at the device form
* **Password**: Must be the same key that was placed on Thinger.io "Device Credentials" parameter
* **MQTT version**: Currently Thinger.io supports 3.1 or 3.1.1 versions of the protocol

{% hint style="info" %}
It is recommended to use SSL/TLS communication using the port 8883
{% endhint %}

Once the client has been configured, it should be able to publish and subscribe data with the server, for example, using the `mosquit_pub` client:

```text
mosquitto_pub -d -h trincado.do.thinger.io -p 1883 -i MQTT -u jt -P testCredential -t telemetry -m '{"temperature":5}'  

Client MQTT sending CONNECT
Client MQTT received CONNACK (0)
Client MQTT sending PUBLISH (d0, q0, r0, m1, 'telemetry', ... (17 bytes))
Client MQTT sending DISCONNECT
```

Where `MQTT` is the  Client ID, `jt` is the user account, and `testCredentials` is the device password. 

Note that Thinger.io MQTT broker has been designed to support multi-tenancy by default. It supports multiple clients / organisations to use the same broker without overlapping topics.

## Working with MQTT data

{% hint style="warning" %}
As MQTT broker is **not creating a Device API**, it won't possible to work with its data in real-time with Endpoints or Dashboards until we develop an adaptation of these features. However, it is possible to store data in Buckets and configure Dashboard widgets to read from that bucket. 
{% endhint %}

### Storing data in buckets

Thinger.io data buckets are a virtual storage where any kind of time series data can be saved, this information can be used to be plotted in dashboards, or to be exported in different formats for offline processing.  
  
Configure a data bucket to store data from an specific MQTT topic just requires going to bucket section of the main menu and pressing the "Add Bucket" button to access the "new bucket form" in which introduce the topic configuration, as has been made in the image below:  

![](../../.gitbook/assets/image%20%285%29.png)

The next parameters needs to be configured: 

* **Bucket Id**: Unique identifier for the bucket. 
* **Bucket name**: Use a representative name to remember the bucket scope, like `WeatherData`.
* **Bucket description**: Fill here any description with more details, like Temperature and humidity in house.
* **Enabled**: Data bucket recording can be enabled or disabled. Just switch it on to enable it.
* **Data Source**: Commonly this defines the Thinger.io device or resource that will be subscribed by the server. In this situation "From MQTT Topic" must be placed
* **MQTT Topic**: place here the MQTT topic that will be subscribed by the server 

This way, Thinger.io Platform server will be configured as MQTT broker but also as a topic consumer in order to provide additional features. Then, the client must be configured to send data in JSON format.

{% hint style="success" %}
Use **JSON** as the payload type for the device messages stored by buckets.
{% endhint %}

### Showing data in Dashboards

Now that the MQTT data is being stored into the data bucket, it is possible to show it on dashboards, where multiple widgets can be used to create real-time or historical representations by selecting the bucket as the data source as it is shown in the image below: 

![](../../.gitbook/assets/image%20%2894%29.png)

Dashboard widgets can show data from different devices, and been configured to create flexible data representations as we have explained in the [**dashboard section of this documentation**](../../features/dashboards.md). 

![](../../.gitbook/assets/image%20%28161%29.png)

### Processing data with Node-RED Plugin 







