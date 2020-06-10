# QUICK START

## Quick Start Guide <a id="quick-start-guide"></a>

To start working with Thinger.io just [**create a free account in our cloud platform**](https://console.thinger.io/#/signup) and follow the next steps to configure and connect your first IoT device

### 1. Create Device <a id="1-create-device"></a>

Using "Devices" menu tab, just click in "New device" button, and fill the form with the device ID, description and Credentials you prefer.

![](https://gblobscdn.gitbook.com/assets%2F-LpXqB3J1BMD5s4OpYSg%2F-LsN7P6PowUddTLYvGOA%2F-LsN9gb0xOpK0UNdi-vR%2Fimage.png?alt=media&token=0771fea4-13c3-4fdc-b419-a42dbe1e63cd)

### 2.Connect Device <a id="2-connect-device"></a>

After provisioning the device at Thinger.io cloud, it is the moment to configure it in the Hardware device. there are many different hardware supports and communication technologies but Thinger.io allows using all of them:

{% tabs %}
{% tab title="Arduino/Linux Devices" %}


1. Install Thinger.io libraries into your Arduino IDE
2. Going to "File&gt;Examples&gt;Thinger.io", open the example code that fix better for the PCB
3. Edit the example code to include your username, device ID and Thinger.io credentials. If the device use a secured networks, connection credentials needs to be written too as shown in the image below. 

![](../.gitbook/assets/image%20%2831%29.png)

This example contains a two simple resources to send and retrieve data from Thinger.io platform, however, it can be modified with many diff



erent functionalities that we have explained at the [**Firmware Coding**](coding/) ****section.  After modifying the source code, just click upload button to flash the sketch and wait for the device connection.

{% hint style="success" %}
Find additional information about Thinger.io devices in the next sections: 

1. \*\*\*\*[**Compatible Arduino and Linux devices**](devices/)\*\*\*\*
2. \*\*\*\*[**Zero to Hero Thinger.io Firmware Coding Guide**](coding/) ****
3. \*\*\*\*[**Connection Troubleshooting Guide**](https://docs.thinger.io/coding/good-practices-and-troubleshooting)\*\*\*\*
{% endhint %}
{% endtab %}

{% tab title="HTTP Devices" %}
1\) Create an HTTP device profile by selecting it in the "Device Type" when creating the device   
2\) Going to the device dashboard, create an HTTP device Callback   
3\) Create a device Access Token to authorize the device sending data to the platform   
4\) Introduce the HTTP request \(API+TOKEN\) into your device code or third party platform and start sending data to Thinger.io

{% page-ref page="devices/http-devices.md" %}
{% endtab %}

{% tab title="Sigfox / TTN Devices" %}
Any individual Sigfox or LoraWAN device can be integrated using our API as HTTP devices, just setting an HTTP device callback into their callback managers, but if a big network is going to be created using these technologies, it is better to use our integration plugins:

{% page-ref page="devices/sigfox.md" %}

{% page-ref page="../features/plugins/the-things-network.md" %}
{% endtab %}

{% tab title="MQTT Devices" %}
1\) Create a new device profile and select "MQTT" device type  
2\) Configure device credentials a secret password  
3\) Configure the MQTT client to send data to the embedded broker

{% page-ref page="devices/mqtt.md" %}
{% endtab %}
{% endtabs %}

Complete device list can be displayed by going to "devices" menu tab. This interface allows managing devices and check its connection status and accessing devices dashboard by clicking over each devices ID.

### 3.Devices & Data management <a id="3-devices-and-data-management"></a>

Each device can be managed through the "Device Dashboard". This interface shows connection data and also allows checking the "device API" with raw device data representation.

![](https://gblobscdn.gitbook.com/assets%2F-LpXqB3J1BMD5s4OpYSg%2F-Lt5H3EX8bLPyRKClaer%2F-Lt5f0kIMf7pn8Sh1TFK%2Fimage.png?alt=media&token=cc271cfb-7bea-488e-9f95-8ff461f9300e)

Thinger.io provides bidirectional communication, so it is possible to retrieve data into the server using "**devices output resources**" and also sending messages from server to the "**devices input resources**". Both resources are represented in the "device API" viewer.Input & Output resources in the device API inspector

![](https://gblobscdn.gitbook.com/assets%2F-LpXqB3J1BMD5s4OpYSg%2F-Lt5gEwTpqNw9VtvwFCV%2F-Lt5hV2FIioDNOVvDr0o%2Fimage.png?alt=media&token=409ed146-1d4e-45dd-812c-531ce4d25d6d)

### 4.Store, Show & Share Data <a id="4-store-show-and-share-data"></a>

Thinger.io provides three essential tools to work with devices data that are the basis for creating any IoT project, next tabs shows each tool introduction:

{% tabs %}
{% tab title="Data Buckets" %}
To **store** **device data** in an scalable way, programming different sampling intervals or recording events raised by devices.
{% endtab %}

{% tab title="Dashboards" %}
Panels with **customizable widgets** that can be created within minutes using drag'n drop technology, to show real-time and stored data.¡
{% endtab %}

{% tab title="Endpoints" %}
Extend the devices interoperatibility by using endpoints to interact with other services like IFTTT, custom Web Services, emails, or call other devices.
{% endtab %}

{% tab title="Access Tockens" %}
Dashboards, Data buckets or Device resources can be easily shared with third parties using **Access Tokens** and our **API**
{% endtab %}
{% endtabs %}

### 5.Extend Thinger.io <a id="5-extend-thinger-io"></a>

Thinger.io platform can be complemented with many different Internet services using **Plugins** that can be found and deployed within seconds Just going to our marketplace and selecting it.

{% page-ref page="../features/plugins/" %}

