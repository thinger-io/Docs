# HTTP DEVICES

{% hint style="info" %}
[This feature is only available for advanced Thinger.io Server instances. freemium users can integrate their HTTP devices sending data to a bucket as explained in the "traditional integration" section of this doc.](https://docs.thinger.io/hardware-devices/http-devices#traditional-http-integration)
{% endhint %}

There are some projects that require the integration of third party data sources, such as a Cloud Service, devices without Thinger.io library on it source code, or a custom program. In this section it is explained how to use the "HTTP device" profile at Thinger.io, that allows receiving data via REST API from whatever source that is able to create a connection with Thinger.io Server and send an HTTP request, taking advantage of all Thinger.io features such as display real time data in dashboards, store in data buckets or process it using a plugin. 

This integration provides bidirectional communication between Thinger.io and the data source by making use of HTTP request and response data, that consist on basic HTTP POST messages with JSON codificated data. 

![](../.gitbook/assets/image%20%2888%29.png)

{% hint style="info" %}
Note that this integration can't explode the 100% of Thinger.io features and benefits in terms of communication efficiency, real-time data and device administration, so it is strongly recommended integrating the devices using a Thinger.io software client if it is available.
{% endhint %}

In the next sections it is explained how to create and configure the HTTP Device Profile at Thinger.io Platform and how to link it with the data source.

## Creating HTTP Device Profile

First step to work with this interface consist on creating the device profile at the "devices" main menu tab and clicking on "new device" button. Then, select "HTTP Device \(Sigfox, Lora, cURL\)" type and fill `Device ID` and `Description` slots as required.  

![](../.gitbook/assets/image%20%28144%29.png)

Ones the profile has been created it is possible to found it at the devices list, then, clicking the device identificator will open the "device dashboard", which is an interface that show device status and connection information and also allows working with the callback configuration and properties. 

![](../.gitbook/assets/image%20%28177%29.png)

However, when time this page is first accessed \(before making the first call to the REST API\) there won't be any information to show o it will have the same aspect as the previous image. Note that this interface contains two additional tabs: the "Callback" tab allows managing the device behavior and capacities, each of these features will be explained in the "[**Managing Callback Functionalities**](https://docs.thinger.io/hardware-devices/http-devices#managing-callback-functionalities)" section of this document.  On the other hand, the "Properties" tab allows to create and manage device properties, which are variables related with this device stored in Thinger.io Server, that can be edited, displayed, or forwarded to the device using callback menu functionalities.

{% hint style="success" %}
[Learn more about Thinger.io **Device Properties** at THIS section of our documentation](https://docs.thinger.io/console#device-properties)
{% endhint %}

## Building the HTTP request

It is necessary to obtain the HTTP request and the authorization that allows interacting with Thinger.io and start sending data from an external system. Clicking into "Callback" tab, it is possible show all callback details and build the request in a simple way, as explained in the next steps:

1\) Going to `Callback / Settings` tab, check the "authorization" box. A bearer token will appear into this section.  

![](../.gitbook/assets/image%20%28146%29.png)

2\) Then, going to `Callback/Overview` tab, an specification of the REST API that provides access to this device will be shown, ready to be copied into the program or HTTP request entry. 

![](../.gitbook/assets/image%20%2852%29.png)

After follow the first step, the authorization token that is shown on this interface will be fixed, being also the same as the one shown at "settings" tab, so it can be copied too in order to create the HTTP Request.  

{% hint style="success" %}
[Click here to find additional documentation about how to implement HTTP request over different systems in the section **"building the HTTP request in the data source"**](https://docs.thinger.io/hardware-devices/http-devices#building-the-http-request-in-the-data-source)\*\*\*\*
{% endhint %}

3\) Now the system is ready to start sending data to Thinger.io via HTTP request, however, note that this system is aimed to receive application/JSON data codified messages. If the system messages do not contain JSON data the server will answer with a 200 OK message to the communication but no data will be stored. The image below shows an example of well defined JSON in the bottom of this tab:

![](../.gitbook/assets/image%20%282%29.png)

## Managing Callback Functionalities 

Ones the callback has been integrated with the system,  the developer will be able to use almost all Thinger.io Platform features by selecting it into `Callback/Settings` tab. In the next section shows a complete specification of the features that can be exploit:

### **Store data in Buckets**

Thinger.io data Buckets is an scalable system that allows to store devices data in a simple way. Allowing support for historical data analysis or download IoT data into a file. The Callback manager allows to associate an existent data bucket to the HTTP Device data in order to store it as is retrieved. 

![](../.gitbook/assets/image%20%281%29.png)

To make this assignment, just check the checkbox and click into the text entry to select or search a previously defined data bucket as shown in the previous image. 

{% hint style="success" %}
[Click here to find additional documentation about data buckets definition and management click here to open Data Buckets documentation](https://docs.thinger.io/console#data-buckets)
{% endhint %}

### **Call Endpoints**

Thinger.io allows defining endpoint profiles that simplifies the execution of a service such as sending an email, send a SMS, call a REST API, interact with IFTTT, call a device from a different account, or call any other HTTP endpoint.

![](../.gitbook/assets/image%20%2834%29.png)

The Callback Manager allows to easy associate the device data with a previously defined Endpoint profile that will be called in real time when the data is processed by Thinger.io server. 

{% hint style="success" %}
[Click here to find additional documentation about Thinger.io Endpoint definition and management at the Endpoints documentation](https://docs.thinger.io/console#endpoints)
{% endhint %}

### **Set device properties**

This feature provides an easy way to select a property of this device in order to store just the last-received data from a device or set an specific attribute such as location data. This feature allows using Thinger.io server as a persistent memory for the device. To select the property that will be modified just select the checkbox and find it's ID in the text entry.

![](../.gitbook/assets/image%20%2862%29.png)

The device properties can also be shown and managed by just going to "Device Properties" tab of the device dashboard.  

![](../.gitbook/assets/image%20%28158%29.png)

### HTTP Response Data

HTTP communications allows sending data to the device in the confirmation message, this feature can be use to create bidirectional communication with these kind of devices using the "Response Data" section of the Device Callback management interface.

To start sending response data, just click over the Check-box and select a device property from the list as shown in the image blelow:

![](../.gitbook/assets/image%20%2830%29.png)

This feature is aimed to introduce an existent device's property into the HTTP response, so if there is not any previously created property, the first step will be adding a new one using the "Properties" menu. 

{% hint style="success" %}
[Learn more about Thinger.io **Device Properties** at THIS section of our documentation](https://docs.thinger.io/console#device-properties)
{% endhint %}

Note that there device properties will be sent using JSON content type, so the device codification has to be ready to retrieve and work with this data.

### **Connection Timeout**

This parameter allows to establish a device connection timeout in minutes, so the platform can consider the device as "disconnected" after a fixed time without receiving messages. 

![](../.gitbook/assets/image%20%2827%29.png)

## Building the HTTP request in the data source

Finally it is necessary to introduce the API given in the "callback overview" section in the system or device, allowing to connect with the platform and start sending data. If everything is done correctly the device dashboard will start displaying information as shown in the image below:

![](../.gitbook/assets/image%20%28110%29.png)

As there are different ways to make this integration, in this section it is explained how to properly implement the request over different supports.  

### Coding the request

For those users that needs creating the complete HTTP request manually, the next segment shows how to make this properly. Note that the request body is formed following the next schemma: `REST API` + `?authorization=` + `Token`

```text
https://trincado.do.thinger.io/v3/users/jt/devices/Example_Device/callback/data?authorization=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJEZXZpY2VDYWxsYmFja19FeGFtcGxlX0RldmljZSIsInVzciI6Imp0In0.RhNQsRz-Ngu7_KPMJxUikPzEvPck1VeZjwUN4YuyhfQ
```

### Using Postman / HTTP request manager

This services provide a useful way to test HTTP integrations in a simply way. It is only necessary to follow the next steps to configure a request:

![](../.gitbook/assets/image%20%28178%29.png)

1. Select &lt;POST&gt; message type
2. Introduce the device callback into the main textbox
3. Create an Authorization header with "bearer" command
4. Create a Content-type header with application/json type
5. Write some valid JSON data and send your query

![](../.gitbook/assets/image%20%2814%29.png)

As shown in the previous image, you should receive an empty 200 OK status message. 

### Using cURL

If the source system supports cURL instructions, there is an integration example into `Callback / Curl` tab, ready to copy and modify:

![](../.gitbook/assets/image%20%2867%29.png)

## Traditional HTTP integration

If the user is working traditional Thinger.io Community Server, the "HTTP" Device will not be available on the "device type" list when creating a new device \(it will, in the future, when the community server gets updated to the newest version of Thinger.io\). But it still being possible to create a similar integration by sending data directly from the device to a Data Bucket using the Open REST API. To make this just follow the next steps: 

1. Go to "Data Bucket" menu section and create a new data bucket selecting and **“From Write Call”** Data source.
2. Create an access token with permissions to write over the data bucket [![image](https://discoursefiles.s3.dualstack.eu-west-1.amazonaws.com/optimized/2X/c/c7ac34097facf9c1e4e60cfcba971cc74f0d0455_2_631x500.png)](https://discoursefiles.s3.dualstack.eu-west-1.amazonaws.com/original/2X/c/c7ac34097facf9c1e4e60cfcba971cc74f0d0455.png)
3. Build an HTTP Request using [Thinger.io](http://thinger.io/) REST API and the token authorization to allow a third party device sending data to the bucket from a third party software.
4. to know the right API, go to the data bucket interface an open the Network inpector \(F12 in Chrome browser\). for example, in this case, the REST API is: [https://api.thinger.io/v1/users/jt/buckets/NodeRED\_Device\_Example/data 2](https://api.thinger.io/v1/users/jt/buckets/NodeRED_Device_Example/data)![](../.gitbook/assets/image%20%2846%29.png) 
5. if we add now the **?authorization=** command and the token access, we obtain the complete request that can be used with any POSTMAN service or any device to send POST request to thinger.io server: [`https://api.thinger.io/v1/users/jt/buckets/NodeRED_Device_Example/data?authorization=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJOb2RlUkVEX0RldmljZV9FeGFtcGxlIiwidXNyIjoianQifQ.1BOzYXp3UveYxGEmKThrSTIblYgc1Dp3IPBFdGAcu_0`](https://api.thinger.io/v1/users/jt/buckets/NodeRED_Device_Example/data?authorization=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJOb2RlUkVEX0RldmljZV9FeGFtcGxlIiwidXNyIjoianQifQ.1BOzYXp3UveYxGEmKThrSTIblYgc1Dp3IPBFdGAcu_0)\`\`
6. note that the value needs to go in a application/json type, so the device code needs to create a valid JSON on it code. 
7. Finally open the data bucket inspector and check the result: [![image](https://discoursefiles.s3.dualstack.eu-west-1.amazonaws.com/optimized/2X/f/f8b64b1a2219d80512bd8028d5904298e6356424_2_492x500.png)](https://discoursefiles.s3.dualstack.eu-west-1.amazonaws.com/original/2X/f/f8b64b1a2219d80512bd8028d5904298e6356424.png)

it is possible to send JSON with the data you need. However, the problem with this integration is that you can’t explode full [thigner.io](http://thigner.io/) features as can be made using the HTTP device, that is why wy will try to upgrade the community server as soon as possible. 

### 

