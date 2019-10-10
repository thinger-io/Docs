# HTTP DEVICES

There are some projects that require the integration of third party data sources, such as a Cloud Service, devices without Thinger.io library on it source code, or a custom program. In this section it is explained how to use the "HTTP device" profile at Thinger.io, that allows receiving data via REST API from whatever source that is able to create a connection with Thinger.io Server and send an HTTP request, taking advantage of all Thinger.io features such as display real time data in dashboards, store in data buckets or process it using a plugin. 

This integration provides bidirectional communication between Thinger.io and the data source by making use of HTTP request and response data, that consist on basic HTTP POST messages with JSON codificated data. 

![](../.gitbook/assets/image%20%2839%29.png)

{% hint style="info" %}
Note that this integration can't explode the 100% of Thinger.io features and benefits in terms of communication efficiency, real-time data and device administration, as it don't count with the required software functionalities, so it is strongly recommended integrating the devices using a Thinger.io software client if it is available for the device
{% endhint %}

In the next sections it is explained how to create and configure the HTTP Device Profile at Thinger.io Platform and how to link it with the data source.

## Creating HTTP Device Profile

First step to work with this interface consist on creating the device profile at the "devices" main menu tab and clicking on "new device" button. Then, select "HTTP Device \(Sigfox, Lora, cURL\)" type and fill `Device ID` and `Description` slots as required.  

![](../.gitbook/assets/image%20%2857%29.png)

Ones the profile has been created it is possible to found it at the devices list, then, clicking the device identificator will open the "device dashboard", which is an interface that show device status and connection information and also allows working with the callback configuration and properties. 

![](../.gitbook/assets/image%20%2869%29.png)

However, when time this page is first accessed \(before making the first call to the REST API\) there won't be any information to show o it will have the same aspect as the previous image.

## Building the HTTP request

It is necessary to obtain the HTTP request and the authorization that allows interacting with Thinger.io and start sending data from an external system. Clicking into "Callback" tab, it is possible show all callback details and build the request in a simple way, as explained in the next steps:

1\) Going to `Callback / Settings` tab, check the "authorization" box. A bearer token will appear into this section.  

![](../.gitbook/assets/image%20%2859%29.png)

2\) Then, going to `Callback/Overview` tab, an specification of the REST API that provides access to this device will be shown, ready to be copied into the program or HTTP request entry. 

![](../.gitbook/assets/image%20%2822%29.png)

After follow the first step, the authorization token that is shown on this interface will be fixed, being also the same as the one shown at "settings" tab, so it can be copied too in order to create the HTTP Request.  

{% hint style="success" %}
[Click here to find additional documentation about how to implement HTTP request over different systems in the section **"building the HTTP request in the data source"**](https://docs.thinger.io/hardware-devices/http-devices#building-the-http-request-in-the-data-source)\*\*\*\*
{% endhint %}

3\) Now the system is ready to start sending data to Thinger.io via HTTP request, however, note that this system is aimed to receive application/JSON data codified messages. If the system messages do not contain JSON data the server will answer with a 200 OK message to the communication but no data will be stored. The image below shows an example of well defined JSON in the bottom of this tab:

![](../.gitbook/assets/image%20%281%29.png)

## Managing Callback Functionalities 

Ones the callback has been integrated with the system,  the developer will be able to use almost all Thinger.io Platform features by selecting it into `Callback/Settings` tab. In the next section shows a complete specification of the features that can be exploit:

### **Store data in Buckets**

Thinger.io data Buckets is an scalable system that allows to store devices data in a simple way. Allowing support for historical data analysis or download IoT data into a file. The Callback manager allows to associate an existent data bucket to the HTTP Device data in order to store it as is retrieved. 

![](../.gitbook/assets/image.png)

To make this assignment, just check the checkbox and click into the text entry to select or search a previously defined data bucket as shown in the previous image. 

{% hint style="success" %}
[Click here to find additional documentation about data buckets definition and management click here to open Data Buckets documentation](https://docs.thinger.io/console#data-buckets)
{% endhint %}

### **Call Endpoints**

Thinger.io allows defining endpoint profiles that simplifies the execution of a service such as sending an email, send a SMS, call a REST API, interact with IFTTT, call a device from a different account, or call any other HTTP endpoint.

![](../.gitbook/assets/image%20%2813%29.png)

The Callback Manager allows to easy associate the device data with a previously defined Endpoint profile that will be called in real time when the data is processed by Thinger.io server. 

{% hint style="success" %}
[Click here to find additional documentation about Thinger.io Endpoint definition and management at the Endpoints documentation](https://docs.thinger.io/console#endpoints)
{% endhint %}

### **Set device properties**

This feature provides an easy way to select a property of this device in order to store just the last-received data from a device or set an specific attribute such as location data. This feature allows using Thinger.io server as a persistent memory for the device. To select the property that will be modified just select the checkbox and find it's ID in the text entry.

![](../.gitbook/assets/image%20%2827%29.png)

The device properties can also be shown and managed by just going to "Device Properties" tab of the device dashboard.  

![](../.gitbook/assets/image%20%2863%29.png)

### **Connection Timeout**

This parameter allows to establish a device connection timeout in minutes, so the platform can consider the device as "disconnected" after a fixed time without receiving messages. 

![](../.gitbook/assets/image%20%2868%29.png)

## Building the HTTP request in the data source

In this section it is explained how to properly implement the request to a Thinger.io Device Callback using different supports.  

### Coding the request \(hand made\) 

For those users that needs creating the complete HTTP request manually, the next segment shows how to make this properly. Note that the request body is formed following the next schemma: `REST API` + `?authorization=` + `Token`

```text
https://trincado.do.thinger.io/v3/users/jt/devices/Example_Device/callback/data?authorization=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJEZXZpY2VDYWxsYmFja19FeGFtcGxlX0RldmljZSIsInVzciI6Imp0In0.RhNQsRz-Ngu7_KPMJxUikPzEvPck1VeZjwUN4YuyhfQ
```

### Using Postman / HTTP request manager

### Using cURL

If the source system supports cURL instructions, there is an integration example into `Callback / Curl` tab, ready to copy and modify:

![](../.gitbook/assets/image%20%2830%29.png)

### 

### 

