# DEVICES ADMINISTRATION

## Device Instance

\
A device in Thinger.io is typically a virtual representation of a physical object connected through an IoT terminal. This instance will receive a series of capabilities on the Thinger.io platform, such as:

* State registration (connection, IP, data bandwidth)
* A unique access REST API
* Storage of attributes through properties. These can be individual or inherited from a group or product profile.
* Storage capabilities in data buckets

Device instances can be provisioned in two ways:

## Create a Single Device

The first step to start any IoT project with Thinger.io (except for not connected devices like Sigfox) is creating a device profile, which will relate the hardware device to the user account. Any device in Thinger.io must be registered to get access to the cloud. Each one has its own identifier and credentials and is related to the user account. This section describes the required steps to register a new device in your account.

All device creation and management processes are performed from the devices tab in the main menu

![](<../.gitbook/assets/image (85).png>)

This section allows to show all registered devices and some information about its connection status as shown in the picture below:

![](https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/58e06f4e8771738d9a1fa7f26a53fdb2864b5937.png)

If it's your first time on thinger.io this list will be empty. Next, we'll show you how to create your first device. First of all, click on **Add Device** that will open a form in which you can introduce your device identification credentials and select a **Device Type** from the drop-down list, selecting one of these types:&#x20;

* **IOTMP device**: For devices with Thinger.io software client on it. Such as Raspberry Pi, Linux or Arduino devices, that will work using IoTMP (Internet of Things Message Protocol)  to transfer the status information and data points.&#x20;
* **HTTP device**: This option allows creating a virtual device to integrate data via REST API Callback, providing nice integration with third-party platforms and other frameworks (Java, Python) and allowing them to work with their data in a simple way.&#x20;
* **MQTT devices:** For MQTT devices that will work with the Thinger.io embedded broker (only for private instances).

![](../.gitbook/assets/AddDevice.PNG)

After selecting your the `device type` the form will adapt the device definition parameters to its specific requirements that must be provided to the system in order to create the device profile:

* **Device identifier** that must be unique within your devices
* **Device description** which will help you to identify your device
* **Device credentials**. Each device has its own identifier/credential, so a compromised device will not affect other devices. All your passwords in the server are stored securely using `PBKDF2 SHA256` with a 32 bytes salt generated with `PRNG` and a non-depreciable amount of iterations.&#x20;

\
Keep your **device identifier** and **device credential**, as you will need them for connecting your device (the password cannot be recovered later).

If everything has been properly filed, you should see some success message when pressing "Add Device" button

![](https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/68e13f2c4df7ecb9f0261d84ba36e12b3d8498ce.png)

Now, you can o back to your devices list, and your device should appear as disconnected.

![](../.gitbook/assets/Device\_list.png)

Now you can use your new device id and the device credentials to connect the new device. Depending on your device, you will need to install the required libraries or development environment, so check out the following sections according to your device:

{% content-ref url="../arduino/" %}
[arduino](../arduino/)
{% endcontent-ref %}

{% content-ref url="../linux.md" %}
[linux.md](../linux.md)
{% endcontent-ref %}

Remember that Sigfox devices does not share the concept of "connected devices", as they are by default offline devices that send information periodically. If you want to store information from these devices, please, check out the following documentation.

{% content-ref url="../lpwan/sigfox.md" %}
[sigfox.md](../lpwan/sigfox.md)
{% endcontent-ref %}

For the following example, we will be using the Arduino IDE along with an ESP8266 device, like the NodeMCU. In this case, you can open the example code for the ESP8266, and fill the device details: your username, the device ID, and the device credentials established while creating the device. The following picture represents the relation between the code and the device created in your account.

![](../.gitbook/assets/AddDevice2.PNG)

Once we have established in the code our account identifier, device identifier, and device credentials, we can compile and flash the program. Meanwhile, we can open our device in the cloud console, just by clicking its identifier in the devices list. In the device screen, you will be able to see some information about your device, like its IP address, connection status, or sent/received information in real time. By default, our device will appear as disconnected, as shown in the picture below.

![](<../.gitbook/assets/image (347).png>)

Once the device gets connected to the account, the interface will change its status, showing "Online" status, and some connection data like the IP address or the upload/download data amount:

![](<../.gitbook/assets/image (363).png>)

Note that the connected device dashboard can show an estimated location of the device, which can be customized when modifying the "location" property as explained in the [**properties**](https://docs.thinger.io/console/devices-administration#device-properties) section of this documentation.&#x20;

Having the first connected device, we are ready to discover all the other Thinger.io features.

## Create a Large Device Network

By means of the `Products` feature, Thinger.io enables efficient management of large fleets of IoT devices through self-provisioning. This means that, instead of manually registering each device, you can create a 'product profile' that represents a specific type of device or category with common characteristics.

Once you've configured a product, you can define its characteristics such as common data buckets to store the historical data of each device, default properties that will be inherited, device dashboard, and data processing scripts.&#x20;

Thinger.io facilitates this process by providing unique access tokens for each device, ensuring a secure and reliable connection. Additionally, through the self-provisioning functionality, devices can automatically register on the platform without manual intervention.

This is especially useful when you need to manage a large number of devices, as it greatly simplifies the onboarding process and subsequent management. In summary, the 'products' functionality with self-provisioning in Thinger.io is a powerful tool for scaling and effectively managing your IoT device fleets.

## Device Explorer

Each device counts with an explorer and administration interface, that allows showing and configuring different devices features. This interface is common for all device types in thinger.io but note that some features such as the "device API explorer" may not be available if the device has not an actual real-time connection with the server.&#x20;

In the next sections, it is explained each different feature of the device explorer:

### Device API

One cool feature of the Thinger.io platform, is that it allows to discover the resources defined in your device. A resource can be a sensor reading, like temperature, humidity, or pressure. A resource can be also any actionable element, like a light, a relay, or a motor. But in general, any device resource is like a callback function that can be called (on demand) through a Rest API. In this way, this section explains how to interact with your device resources over the cloud console, but also you will find how you can issue your own REST API calls to query your device.

Once you have your device connected to your account, as described in the previous section, you can access to its resources and explore the API Rest endpoints using the `API EXPLORER`. You can access to this screen over the Device Dashboard, by clicking on the small blue button called `Device API`.

In the API explorer interface you will see one different box for each resource defined in your code. Each resource has an identifier, that is related with the resource name defined in your code. In the Thinger.io platform, you can define 4 different types of resources, one for input (sending data to the device), one for output (the device will send information), one for input/output (you can send and receive information in one call), and just a callback resource, which you can just execute without sending or receiving information. The input and output data, from the API perspective, can be any JSON document. Take a look to your library documentation in order to see how to define these different resources.

For example, the default ESP8266 example in the Arduino libraries, defines two different resources. One input resource, called `led`, for controlling the `BUILTIN_LED`, and one output resource, called `millis` to extract the current "millis" value of the device, as defined in the following code. Notice that a resource name can be any arbitrary text to identify the underlying resource, as they are not tied to any constant defined in the platform.

So, these are our sample resource:

```cpp
thing["led"] << digitalPin(BUILTIN_LED);
thing["millis"] >> outputValue(millis());
```

If our device is connected to the platform, we can open our device API explorer and see the defined resources in the platform, like in the following picture.

![](../.gitbook/assets/deviceApi.PNG)

You can see how our defined resources in the device are now available in the platform, as the device is able to report the available resources and their format (or current state). The idea is that you can test here your resources, that is, interacting with them in real-time. In this case, you will be able to switch the led state or read the current milliseconds from the Arduino device. Every click in the `Run` button will execute your resource, i.e., forcing a read from a sensor, calling the `millis()` function, or sending a new state for the actuator, depending on the resource type (input or output).

Thanks to this feature, every device resource can be translated to a REST API endpoint in a very simple way, so can be consumed or interacted with any other devices or applications using standard REST queries, i.e., using a `POST` method to send values to the device, or using a `GET` method to read information from the device.&#x20;

It is also possible to create more complex resources with both input and output functionalities. The example below, basically returns the sum and multiplication between two integer numbers:

```cpp
thing["in_out"] = [](pson& in, pson& out){
    out["sum"] = (int)in["value1"] + (int)in["value2"];
    out["mult"] = (int)in["value1"] * (int)in["value2"];
};
```

This resource definition will be translated to the following resource in the platform, where it is possible to both test input values, and view the output result. So, you can try entering some values, click on `Run`, and see the output reported by the device. This example also emphasizes how the resources work, as they are not just static values, but callbacks you can call with any input or output value.

![](<../.gitbook/assets/inOutResource (1).PNG>)

In addition to this useful device API explorer where you can interact with your devices, you can also obtain specific information about the REST API endpoint by clicking on the `Show Query` button. This provides information about the method type, URL, content type, request body, and response body. You can also click on `Curl`, so you can copy the command to interact with your device directly from your console. The above example is translated to the following REST API call:

![](<../.gitbook/assets/showQuery (1).PNG>)

There is more information available about the API for interacting with your devices [here](http://docs.thinger.io/api/#devices-api-access-device-resources).

### Device Tokens

All the interactions with your connected devices, i.e., by using the REST API endpoints commented above, or mobile phone, needs to be authenticated against the platform. By default, when you interact with your devices over the Thinger.io console, you are implicitly signing all your requests to the platform with an access token you obtained from your username and password. This kind of authorization grants access to all your account resources, so you can configure devices, buckets, etc. However, this authorization expires quite frequently (but renewed automatically by your browser), and cannot be used to grant access to our devices to other users or platforms, as you will be providing access to all your account.

In this case, it is possible to create specific access tokens for granting access to your devices and even grant access to specific resources on your devices. Moreover, it is possible to define the token validity in time, by enabling an expiration date. This way, if you need to provide access to some of your device resources to a third-party tool like IFTTT, an external web page, a mobile phone, or any other service, it is highly recommended to create a device token.

To create a device token, open the device Dashboard and take a look at the subsection called "Device Tokens". Then, click on the green button `Add` on the right of the box. Then, a modal window will appear, where you can configure different parameters:

* Token name: Use a representative name to remember why the token was issued, i.e., IFTTT Access, Mobile phone, etc.
* Token access: Configure the token to allow accessing all device resources, or limit access to a set of resources.
* Token expiration: Configure the token to expire at some given date, or available indefinitely.

The following figure shows a sample screenshot while configuring a device token.

![](../.gitbook/assets/addTockenForm.PNG)

Once the token is saved, the interface will show the access token to be used in the REST API Calls, like in the following figure. If you need help to integrate this access token in the REST API calls, check out [this](http://docs.thinger.io/api/#authentication-api-rest-api-authentication) documentation.

![](../.gitbook/assets/deviceTokenValue.png)

### HTTP devices Callback <a href="#http-devices-callback" id="http-devices-callback"></a>

Because of the nature of these devices, [thinger.io](http://thinger.io/) applies a special treatment, based on the use of callbacks to make the integration. A callback is a functionality of the server that can be used to request a process with device data by means of an HTTP query, such as store it in a bucket, calling an endpoint profile or registering the information contained in a JSON that should be sent with the query.

To create a callback, open the device Dashboard and take a look at the subsection called "callback", that will show different options in the context `callback details` as shown in the image below:

![](../.gitbook/assets/CallbackDetails.PNG)

This context shows the different functionalities that can be requested from the server using a callback, by just clicking in the checkbox and selecting the resource that will receive the data, such as:

* Data storage in scalable [Data Buckets](http://docs.thinger.io/console/#data-buckets)
* Calling [Endpoint Profiles](http://docs.thinger.io/console/#endpoints) to integrate with third parties
* Retrieving or modifying [Device Properties](http://docs.thinger.io/api/#Device-properties) using `Set device property` or `response data` features.

Note that it is not possible to create properties, data buckets, or endpoints through callback requests, so it is necessary to initialize then first using the web console or via REST API.

Ones you have configured the callback details, the system will be ready to receive a request. In a similar way to the "show query" feature included in the Connected device's dashboard, you can find a precise specification of the HTTP request structure and a complete cURL example by clicking in the "overview" or "cURL Example" tabs in the upper side of `Callback Details` context as shown in the image below:

![](<../.gitbook/assets/image (419).png>)

Finally, to create a Callback HTTP request, take in count that the `Authorization Header` must be included 9in your HTTP request as shown in the example below:

```
https://<Thinger_Server>/v3/users/<Username>/devices/<Device_ID>/callback?authorization=<Authorization_Header>
```

### Device Tokens <a href="#device-tokens" id="device-tokens"></a>

All the interactions with your connected devices, i.e., by using the REST API endpoints commented above, or mobile phone, needs to be authenticated against the platform. By default, when you interact with your devices over the [Thinger.io](http://thinger.io/) console, you are implicitly signing all your requests to the platform with an access token you obtained from your username and password. This kind of authorization grants access to all your account resources, so you can configure devices, buckets, etc. However, this authorization expires quite frequently (but renewed automatically by your browser), and cannot be used to grant access to our devices to other users or platforms, as you will be providing access to all your account.

In this case, it is possible to create specific access tokens for granting access to your devices and even grant access to specific resources on your devices. Moreover, it is possible to define the token validity in time, by enabling an expiration date. This way, if you need to provide access to some of your device resources to a third-party tool like IFTTT, an external web page, a mobile phone, or any other service, it is highly recommended to create a device token.

To create a device token, open the device Dashboard and take a look to the subsection called "Device Tokens".

![](<../.gitbook/assets/image (492).png>)

Then, click on the green button \`Add\` on the right of the box. Then, a modal window will appear, were you can configure different parameters:

* **Token name**: Use a representative name to remember why the token was issued, i.e., IFTTT Access, Mobile phone, etc.
* **Token access**: Configure the token to allow accessing all device resources, or limit the access to a set of resources.
* **Token expiration**: Configure the token to expire at some given date, or available indefinitely.

The image below shows a sample screenshot while configuring a device token.

![](<../.gitbook/assets/addTockenForm (1).PNG>)

Once the token is saved, the interface will show the access token to be used in the REST API Calls, like in the following figure. If you need help to integrate this access token in the REST API calls, checkout [this](http://docs.thinger.io/api/#authentication-api-rest-api-authentication) documentation.

![](../.gitbook/assets/device\_token\_value.png)

### Device Properties <a href="#device-properties" id="device-properties"></a>

[Thinger.io](http://thinger.io/) provides a simple way to store additional information related to a specific device, such as location, identification, or even configuration parameters that may be retrieved by devices using common JSON files. In this way, the platform can be used as a device's persistent memory. To create a device property, open the device Dashboard, and take a look to the subsection called "Properties".

![](<../.gitbook/assets/image (490).png>)

This menu provides an easy way to create, manage, or delete the device's properties. Note that the property created in this example is specifying the device location. [Thinger.io](http://thinger.io/) system has been designed to detect this configuration and automatically represent it on the device dashboard map.

![](../.gitbook/assets/AddDeviceProperty.PNG)

Properties declarations and modifications are made by means of a special context, provided with a JSON validator that enhances the text and checks morphologic mistakes.

#### Coding with properties <a href="#coding-with-properties" id="coding-with-properties"></a>

It is also possible to create, retrieve and modify data properties from devices, however, at this point we must differentiate between HTTP devices or [thinger.io](http://thinger.io/) software client devices, which will use `set_property()` or `get_propery()` commands as shown in the examples below:

* **Setting a property**

```
                    /*set property value*/
void loop(){
thing.handle();

if(event){    //must be flow controlled

    //create a pson with new values
    pson data;
    data["longitude"]=-4.056;
    data["latitude"]=41.40;

    //sending new values to platform
    thing.set_property("location", data, true);
    }
}
```

* **Reading a property**

Where "location" is the property\_ID, "data" the PSON to be sent, and the boolean (true/false) to get writing confirmation.&#x20;

```
                /*retrieve property value*/
void loop(){
thing.handle();

//creating a pson to store the property values
pson data;

//retrieving data from the platform
thing.get_property("My_Property", data);
  float lng=data["longitude"];
  float lat=data["latitude"];
  Serial.print("L: ");
  Serial.print(lng);
  Serial.print(" , l: ");
  Serial.println(lat);
}
```

{% hint style="info" %}
More details about property codification functions at the **"**[**codification**](../coding-guide.md)**"** section of this documentation.
{% endhint %}

Using HTTP devices it's also possible to interact with properties through callback configuration submenu tools.

![](../.gitbook/assets/HTTPGetSetProperty.PNG)

According to this configuration, when [Thigner.io](http://thigner.io/) server receives any transmission from "SigfoxDevice1" the payload data will be stored into "data" property, creating a JSON with all variables. In the opposite situation, thanks to the "Response Data" feature, the values stored in the parameter with was called "downlink\_data" will be sent to the device thought Sigfox infrastructure.

### Device Online Terminal <a href="#device-settings" id="device-settings"></a>

This feature has been designed to work as a common program terminal, allowing to print debug or execution messages from the device program but working online through a device resource.&#x20;

{% hint style="info" %}
This feature is only available for generic devices equipped with thinger.io software client code library
{% endhint %}

![](<../.gitbook/assets/image (390).png>)

### Device Settings <a href="#device-settings" id="device-settings"></a>

It is possible to adjust some device details like its description or credentials going to the "Settings" subsection of the device dashboard. This way, you can change the device credentials by a new one of your choice in case you forgot it (the password cannot be recovered from database as it is encrypted). Notice that changing the device password, will not disconnect the device, but will prevent its reconnection once disconnected.

![](<../.gitbook/assets/deviceEdit (1).png>)

If you need to change the device identifier it is necessary to delete the device and register a new one with the desired one.
