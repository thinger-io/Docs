---
description: >-
  To integrate any kind of third party device or third party data resource with
  thinger.io
---

# HTTP DEVICES

Some projects require the integration of third-party data sources, such as a Cloud Service, devices without the Thinger.io library in their source code, or a custom program. In this section it is explained how to use the "HTTP device" profile at Thinger.io, that allows receiving data via REST API from whatever source that can create a connection with Thinger.io Server and send an HTTP request, taking advantage of all Thinger.io features such as display real-time data in dashboards, store in data buckets or process it using a plugin.&#x20;

This integration provides bidirectional communication between Thinger.io and the data source by making use of HTTP requests and response data, which consist of basic HTTP POST messages with JSON-encoded data.&#x20;

<figure><img src=".gitbook/assets/imageN.png" alt="" width="563"><figcaption></figcaption></figure>

{% hint style="info" %}
Note that this integration can't expose 100% of Thinger.io features and benefits in terms of communication efficiency, real-time data and device administration, so it is strongly recommended to integrate the devices using the Thinger.io software client if it is available.
{% endhint %}

In the next sections, it is explained how to create and configure the HTTP Device Profile at Thinger.io Platform and how to link it with the data source.

## Creating an HTTP Device Profile

The first step to work with this interface consists of creating the device profile at the "devices" main menu tab and clicking on the "new device" button. Then, select "HTTP Device (Sigfox, LoRa, cURL)" type and fill `Device ID` , `Device Name`  and `Description` slots as required. &#x20;

<figure><img src=".gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

Once the profile has been created, it is possible to find it in the devices list, then clicking the device identifier will open the "device dashboard", which is an interface that shows device status and connection information and also allows working with the callback configuration and properties.&#x20;

<figure><img src=".gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

Also, scrolling down a little, we can see the Daily Data Transmission:

<figure><img src=".gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

However, when this page is first accessed (before making the first call to the REST API), there won't be any information to show it will have the same aspect as the previous image. Note that this interface contains two additional tabs: the "Callback" tab allows managing the device behavior and capacities, and each of these features will be explained in the "[**Managing Callback Functionalities**](https://docs.thinger.io/hardware-devices/http-devices#managing-callback-functionalities)" section of this document.  On the other hand, the "Properties" tab allows creating and managing device properties, which are variables related to this device stored in Thinger.io Server, that can be edited, displayed, or forwarded to the device using callback menu functionalities.

{% hint style="success" %}
[Learn more about Thinger.io **Device Properties** at THIS section of our documentation](https://docs.thinger.io/console#device-properties)
{% endhint %}

## Building the HTTP request

It is necessary to obtain the HTTP request and the authorization that allows interacting with Thinger.io and start sending data from an external system. Clicking into the "Callback" tab, it is possible to show all callback details and build the request in a simple way, as explained in the next steps:

1\) Going to the Device profile, and clicking on the right-top menu, which will open, among others, the `Callback` tab:&#x20;

<figure><img src=".gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

2\) Then, going to `Callback/Overview` tab, a specification of the REST API that provides access to this device will be shown, ready to be copied into the program or HTTP request entry.&#x20;

<figure><img src=".gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

After following the first step, the authorization token that is shown on this interface will be fixed, being also the same as the one shown at the "settings" tab, so it can be copied too in order to create the HTTP Request. &#x20;

{% hint style="success" %}
[Click here to find additional documentation about how to implement HTTP requests over different systems in the section **"Building the HTTP request in the data source."**](https://docs.thinger.io/hardware-devices/http-devices#building-the-http-request-in-the-data-source)
{% endhint %}

3\) Now the system is ready to start sending data to Thinger.io via HTTP request; however, note that this system is aimed at receiving application/JSON data codified messages. If the system messages do not contain JSON data, the server will answer with a 200 OK message to the communication, but no data will be stored. An example of well-defined JSON is shown at the bottom of this tab:

![](<.gitbook/assets/image (227).png>)

## Managing Callback Functionalities&#x20;

Once the callback has been integrated with the system,  the developer will be able to use almost all Thinger.io Platform features by selecting them in the `Callback/Settings` tab. The next section shows a complete specification of the features that can be exploited:

<figure><img src=".gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

### **Store data in Buckets**

Thinger.io data Buckets is a scalable system that allows storing devices' data simply. Allowing support for historical data analysis or downloading IoT data into a file. The Callback manager allows associating an existing data bucket with the HTTP Device data in order to store it as it is retrieved.&#x20;

<figure><img src=".gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

To make this assignment, just check the checkbox and click into the text entry to select or search a previously defined data bucket, as shown in the previous image.&#x20;

{% hint style="success" %}
[Click here to find additional documentation about Data Buckets' definition and management. Click here to open Data Bucket documentation](features/buckets.md)
{% endhint %}

### **Call Endpoints**

Thinger.io allows defining endpoint profiles that simplify the execution of a service such as sending an email, sending an SMS, calling a REST API, interacting with IFTTT, calling a device from a different account, or calling any other HTTP endpoint.



![](<.gitbook/assets/image (184).png>)

The Callback Manager allows for easy association of the device data with a previously defined Endpoint profile that will be called in real time when the data is processed by the Thinger.io server.&#x20;

{% hint style="success" %}
[Click here to find additional documentation about Thinger.io Endpoint definition and management at the Endpoints documentation](https://docs.thinger.io/console#endpoints)
{% endhint %}

### **Set device properties**

This feature provides an easy way to select a property of this device in order to store just the last-received data from a device or set a specific attribute, such as location data. This feature allows using the Thinger.io server as a persistent memory for the device. To select the property that will be modified, just select the checkbox and find its ID in the text entry.

![](<.gitbook/assets/image (207).png>)

The device properties can also be shown and managed by just going to the "Device Properties" tab of the device dashboard. &#x20;

<figure><img src=".gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

### HTTP Response Data

HTTP communications allow sending data to the device in the confirmation message. This feature can be used to create bidirectional communication with these kinds of devices using the "Response Data" section of the Device Callback management interface.

To start sending response data, just click on the check box and select a device property from the list:

![](<.gitbook/assets/image (169).png>)

This feature is aimed at introducing an existing device's property into the HTTP response, so if there is any previously created property, the first step will be adding a new one using the "Properties" menu.&#x20;

{% hint style="success" %}
[Learn more about Thinger.io **Device Properties** at THIS section of our documentation](https://docs.thinger.io/console#device-properties)
{% endhint %}

Note that the device properties will be sent using JSON content type, so the device codification has to be ready to retrieve and work with this data.

### **Connection Timeout**

This parameter allows the establishment of a device connection timeout in minutes, so the platform can consider the device as "disconnected" after a fixed time without receiving messages. This will be shown with the red dot:

<figure><img src=".gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

## Building the HTTP request in the data source

Finally, it is necessary to introduce the API given in the "callback overview" section in the system or device, allowing it to connect with the platform and start sending data. If everything is done correctly, the device dashboard will start displaying information:

<figure><img src=".gitbook/assets/image (50).png" alt="" width="563"><figcaption></figcaption></figure>

As there are different ways to make this integration, in this section, it is explained how to properly implement the request over different platforms. &#x20;

### Coding the request

For those users who need to create the complete HTTP request manually, the next segment shows how to make this properly. Note how the request body is formed: `REST API` + `?authorization=` + `Token`

```
https://trincado.do.thinger.io/v3/users/jt/devices/Example_Device/callback/data?authorization=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJEZXZpY2VDYWxsYmFja19FeGFtcGxlX0RldmljZSIsInVzciI6Imp0In0.RhNQsRz-Ngu7_KPMJxUikPzEvPck1VeZjwUN4YuyhfQ
```

### Using Postman / HTTP request manager

These services provide a useful way to test HTTP integrations in a simple way. It is only necessary to follow the next steps to configure a request:

![](<.gitbook/assets/image (199).png>)

1. Select \<POST> message type
2. Introduce the device callback into the main textbox
3. Create an Authorization header with the "bearer" command
4. Create a Content-Type header with application/json type
5. Write some valid JSON data and send the query

![](<.gitbook/assets/image (191).png>)

An empty 200 OK status message should be received.&#x20;

### Using cURL

If the source system supports cURL instructions, there is an integration example into `Callback / Overview`tab, and scroll down until the Curl Example, ready to copy and modify:

<figure><img src=".gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>
