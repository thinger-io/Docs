---
description: >-
  To integrate any kind of third party device or third party data resource with
  thinger.io
---

# HTTP DEVICES

There are some projects that require the integration of third-party data sources, such as a Cloud Service, devices without Thinger.io library on it source code, or a custom program. In this section it is explained how to use the "HTTP device" profile at Thinger.io, that allows receiving data via REST API from whatever source that is able to create a connection with Thinger.io Server and send an HTTP request, taking advantage of all Thinger.io features such as display real-time data in dashboards, store in data buckets or process it using a plugin.&#x20;

This integration provides bidirectional communication between Thinger.io and the data source by making use of HTTP request and response data, which consist of basic HTTP POST messages with JSON codified data.&#x20;

<figure><img src=".gitbook/assets/imageN.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Note that this integration can't explode the 100% of Thinger.io features and benefits in terms of communication efficiency, real-time data and device administration, so it is strongly recommended integrating the devices using the Thinger.io software client if it is available.
{% endhint %}

In the next sections, it is explained how to create and configure the HTTP Device Profile at Thinger.io Platform and how to link it with the data source.

## Creating HTTP Device Profile

First step to work with this interface consists of creating the device profile at the "devices" main menu tab and clicking on "new device" button. Then, select "HTTP Device (Sigfox, Lora, cURL)" type and fill `Device ID` and `Description` slots as required. &#x20;

![](<.gitbook/assets/image (191).png>)

Ones the profile has been created it is possible to found it at the devices list, then, clicking the device identificator will open the "device dashboard", which is an interface that show device status and connection information and also allows working with the callback configuration and properties.&#x20;

![](<.gitbook/assets/image (236).png>)

However, when time this page is first accessed (before making the first call to the REST API) there won't be any information to show o it will have the same aspect as the previous image. Note that this interface contains two additional tabs: the "Callback" tab allows managing the device behavior and capacities, each of these features will be explained in the "[**Managing Callback Functionalities**](https://docs.thinger.io/hardware-devices/http-devices#managing-callback-functionalities)" section of this document.  On the other hand, the "Properties" tab allows to create and manage device properties, which are variables related with this device stored in Thinger.io Server, that can be edited, displayed, or forwarded to the device using callback menu functionalities.

{% hint style="success" %}
[Learn more about Thinger.io **Device Properties** at THIS section of our documentation](https://docs.thinger.io/console#device-properties)
{% endhint %}

## Building the HTTP request

It is necessary to obtain the HTTP request and the authorization that allows interacting with Thinger.io and start sending data from an external system. Clicking into "Callback" tab, it is possible to show all callback details and builds the request in a simple way, as explained in the next steps:

1\) Going to `Callback / Settings` tab, check the "authorization" box. A bearer token will appear into this section. &#x20;

![](<.gitbook/assets/image (193).png>)

2\) Then, going to `Callback/Overview` tab, an specification of the REST API that provides access to this device will be shown, ready to be copied into the program or HTTP request entry.&#x20;

![](<.gitbook/assets/image (69).png>)

After follow the first step, the authorization token that is shown on this interface will be fixed, being also the same as the one shown at "settings" tab, so it can be copied too in order to create the HTTP Request. &#x20;

{% hint style="success" %}
[Click here to find additional documentation about how to implement HTTP request over different systems in the section **"building the HTTP request in the data source"**](https://docs.thinger.io/hardware-devices/http-devices#building-the-http-request-in-the-data-source)****
{% endhint %}

3\) Now the system is ready to start sending data to Thinger.io via HTTP request, however, note that this system is aimed to receive application/JSON data codified messages. If the system messages do not contain JSON data the server will answer with a 200 OK message to the communication but no data will be stored. The image below shows an example of well defined JSON in the bottom of this tab:

![](<.gitbook/assets/image (3).png>)

## Managing Callback Functionalities&#x20;

Ones the callback has been integrated with the system,  the developer will be able to use almost all Thinger.io Platform features by selecting it into `Callback/Settings` tab. In the next section shows a complete specification of the features that can be exploit:

### **Store data in Buckets**

Thinger.io data Buckets is a scalable system that allows storing devices data in a simple way. Allowing support for historical data analysis or download IoT data into a file. The Callback manager allows associating an existent data bucket to the HTTP Device data in order to store it as is retrieved.&#x20;

![](<.gitbook/assets/image (2) (1) (1).png>)

To make this assignment, just check the checkbox and click into the text entry to select or search a previously defined data bucket as shown in the previous image.&#x20;

{% hint style="success" %}
[Click here to find additional documentation about data buckets definition and management click here to open Data Buckets documentation](features/buckets.md)
{% endhint %}

### **Call Endpoints**

Thinger.io allows defining endpoint profiles that simplifies the execution of a service such as sending an email, send a SMS, call a REST API, interact with IFTTT, call a device from a different account, or call any other HTTP endpoint.

![](<.gitbook/assets/image (47).png>)

The Callback Manager allows to easy associate the device data with a previously defined Endpoint profile that will be called in real time when the data is processed by Thinger.io server.&#x20;

{% hint style="success" %}
[Click here to find additional documentation about Thinger.io Endpoint definition and management at the Endpoints documentation](https://docs.thinger.io/console#endpoints)
{% endhint %}

### **Set device properties**

This feature provides an easy way to select a property of this device in order to store just the last-received data from a device or set an specific attribute such as location data. This feature allows using Thinger.io server as a persistent memory for the device. To select the property that will be modified just select the checkbox and find it's ID in the text entry.

![](<.gitbook/assets/image (82).png>)

The device properties can also be shown and managed by just going to "Device Properties" tab of the device dashboard. &#x20;

![](<.gitbook/assets/image (210).png>)

### HTTP Response Data

HTTP communications allows sending data to the device in the confirmation message, this feature can be use to create bidirectional communication with these kind of devices using the "Response Data" section of the Device Callback management interface.

To start sending response data, just click over the Check-box and select a device property from the list as shown in the image blelow:

![](<.gitbook/assets/image (41).png>)

This feature is aimed to introduce an existent device's property into the HTTP response, so if there is not any previously created property, the first step will be adding a new one using the "Properties" menu.&#x20;

{% hint style="success" %}
[Learn more about Thinger.io **Device Properties** at THIS section of our documentation](https://docs.thinger.io/console#device-properties)
{% endhint %}

Note that there device properties will be sent using JSON content type, so the device codification has to be ready to retrieve and work with this data.

### **Connection Timeout**

This parameter allows to establish a device connection timeout in minutes, so the platform can consider the device as "disconnected" after a fixed time without receiving messages.&#x20;

![](<.gitbook/assets/image (38).png>)

## Building the HTTP request in the data source

Finally it is necessary to introduce the API given in the "callback overview" section in the system or device, allowing to connect with the platform and start sending data. If everything is done correctly the device dashboard will start displaying information as shown in the image below:

![](<.gitbook/assets/image (143) (1).png>)

As there are different ways to make this integration, in this section it is explained how to properly implement the request over different supports. &#x20;

### Coding the request

For those users that needs creating the complete HTTP request manually, the next segment shows how to make this properly. Note that the request body is formed following the next schemma: `REST API` + `?authorization=` + `Token`

```
https://trincado.do.thinger.io/v3/users/jt/devices/Example_Device/callback/data?authorization=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJEZXZpY2VDYWxsYmFja19FeGFtcGxlX0RldmljZSIsInVzciI6Imp0In0.RhNQsRz-Ngu7_KPMJxUikPzEvPck1VeZjwUN4YuyhfQ
```

### Using Postman / HTTP request manager

This services provide a useful way to test HTTP integrations in a simple way. It is only necessary to follow the next steps to configure a request:

![](<.gitbook/assets/image (237).png>)

1. Select \<POST> message type
2. Introduce the device callback into the main textbox
3. Create an Authorization header with "bearer" command
4. Create a Content-type header with application/json type
5. Write some valid JSON data and send your query

![](<.gitbook/assets/image (22).png>)

As shown in the previous image, you should receive an empty 200 OK status message.&#x20;

### Using cURL

If the source system supports cURL instructions, there is an integration example into `Callback / Curl` tab, ready to copy and modify:

![](<.gitbook/assets/image (87).png>)
