# HTTP DEVICES

There are some projects that require the integration of third party data sources, such as a Cloud Service, devices without Thinger.io library on it source code, or a custom program. In this section it is explained how to use the "HTTP device" profile at Thinger.io, that allows receiving data via REST API from whatever source that is able to create a connection with Thinger.io Server and send an HTTP request, taking advantage of all Thinger.io features such as display real time data in dashboards, store in data buckets or process it using a plugin. 

This integration provides bidirectional communication between Thinger.io and the data source by making use of HTTP request and response data, that consist on basic HTTP POST messages with JSON codificated data. 

![](../.gitbook/assets/image%20%2836%29.png)

{% hint style="info" %}
Note that this integration can't explode the 100% of Thinger.io features and benefits in terms of communication efficiency, real-time data and device administration, as it don't count with the required software functionalities, so it is strongly recommended integrating the devices using a Thinger.io software client if it is available for your device
{% endhint %}

In the next sections it is explained how to create and configure the HTTP Device Profile at Thinger.io Platform and how to link it with the data source.

## Creating HTTP Device Profile

First step to work with this interface consist on creating the device profile at the "devices" main menu tab and clicking on "new device" button. Then, select "HTTP Device \(Sigfox, Lora, cURL\)" type and fill `Device ID` and `Description` slots as you require.  

![](../.gitbook/assets/image%20%2852%29.png)

Ones the profile has been created we can found it at the devices list, clicking its name text you can access to the "device dashboard", which is an interface that show device status and connection information and also allows working with the callback configuration and properties. 

![](../.gitbook/assets/image%20%2861%29.png)

However, first time you access to this page before making the first call to the REST API, there won't be any information to show so it will have the same aspect as the previous image.

## Building the HTTP request

To start sending data to the system, it will be necessary to obtain the HTTP request and the authorization that allows interacting with Thinger.io platform from an external system. Clicking into "Callback" tab of the last interface you will be able to show all callback details, then follow the next process:

1\) Going to `Callback / Settings` tab, check the "authorization" box. A bearer token will appear into this section. Copy this Token into your source authorization section. 

![](../.gitbook/assets/image%20%2854%29.png)

2\) Then go to `Callback/Overview` tab, where you will find an specification of the REST API that provides access to this device profile ready to copy into your program or HTTP request entry.

![](../.gitbook/assets/image%20%2820%29.png)

If you follow first step, the authorization token that is shown at this interface will be the same as the one in "settings" tab, so you can now copy it and paste into your program or HTTP Request Authorization entry.  

{% hint style="success" %}
[you will find an explanation about how to implement HTTP request over different systems in the next section **"building the HTTP request in the data source"**](https://docs.thinger.io/hardware-devices/http-devices#integrating-the-http-request-in-the-data-source)\*\*\*\*
{% endhint %}

3\) Now you are ready to start sending data to Thinger.io via HTTP request, however, note that this system is aimed to receive application/JSON data codified messages. If your messages do not contain JSON data the server will answer with a 200 OK message to the communication but no data will be stored. You can find an example of well defined JSON in the bottom of this tab, as shown in the image below:

![](../.gitbook/assets/image.png)

## Managing Callback Functionalities 

Ones the callback has been integrated with your system, you will be able to use almost all Thinger.io Platform features by selecting it into `Callback/Settings` tab. In the next section you will find a complete specification of the features you can exploit:

### **Store data in Buckets**

This feature allows to associate an existent data bucket to the device data in order to store it as is retrieved. 

### **Call Endpoints**

Allows to associate the device data with an Endpoint profile that will be called in real time when the data is processed by Thinger.io server. 

### **Set device properties**

This feature provides an easy way to store just the last-received data from a device or set an specific property, using Thinger.io server as a persistent memory for the device.  

### **Connection Timeout**

This parameter allows to establish a device connection timeout in minutes, so the platform can consider the device as "disconnected" after a fixed time without receiving messages. 

## building the HTTP request in the data source

### Using Postman / HTTP request manager

### Using cURL

If you are working with a system that supports cURL instructions, you will find a example integration by going into `Callback / Curl` tab ready to copy and modify.

![](../.gitbook/assets/image%20%2827%29.png)

### Coding the request \(hand made\) 

For those users that needs creating the complete HTTP request manually, the next segment shows how to make this properly. Note that the request body is formed following the next schemma: `REST API` + `?authorization=` + `Token`

```text
https://trincado.do.thinger.io/v3/users/jt/devices/Example_Device/callback/data?authorization=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJEZXZpY2VDYWxsYmFja19FeGFtcGxlX0RldmljZSIsInVzciI6Imp0In0.RhNQsRz-Ngu7_KPMJxUikPzEvPck1VeZjwUN4YuyhfQ
```



