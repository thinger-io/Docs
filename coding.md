---
description: >-
  This section will cover how to add different functionality to your devices for
  exposing resources, calling endpoints, or streaming data to real-time
  websockets.
---

# CODING

## Sketch Overview

Almost all Arduino Sketches looks the same. There is a setup method, and there is a loop method. Nothing changes here while integrating with Thinger.io. However you must know where you should define your device resources, or where it is possible to interact with external services. In general terms, any device resource \(led, relay, sensor, servo, etc.\) must be defined inside the `setup()` method. As well as you initialize your devices, set the input/output direction of a digital pin, or initialize the Serial port speed, you also need to initialize here your resources. This basically consists on configuring what values or resources you want to expose over the Internet.

The `loop()` is the place to always call to the `thing.handle()` method, so the thinger libraries can handle the connection with the platform. This is the place also for calling your endpoints, or streaming real-time data to an open WebSocket. Please, take into account to **do not add any delay inside the `loop()`** except if you know what you are doing, like working with deep sleep modes or so in your device. Any other delay will condition the proper functioning of Thinger in your device. Also it can be bad to read a sensor value in every loop if the sensor takes too much time to complete a read. This will result in a device with a noticeable lag while attending to our commands.

```cpp
// add required headers according to your device
#include <SPI.h>
#include <Ethernet.h>
#include <ThingerEthernet.h>

// initialize Thinger instance (type can change depending on your device)
ThingerEthernet thing("username", "deviceId", "deviceCredential");

void setup() {
    // initialize your sensors and pins

    // initialize wifi (see examples for your device)

    // add resources here, like sensors, lights, etc.
}

void loop() {
  // call always the thing handle in the loop and avoid any delay here
  thing.handle(); 
  // here you can call endpoints
  // and also you can stream resources
}
```

You can easily start with some available example for your device after you install the client libraries.

![](.gitbook/assets/arduino-examples.png)

> It is recommended to start with some of the examples available in the Arduino IDE when you install the librarires

## Setting Credentials

All the devices connected to the platform needs to be authenticated against the server. When you create a [device in the console](https://link_to_console) you are basically creating a new device identifier and setting a device credential. Therefore, you need to setup this credentials also in your Arduino code so the device can be recognized and associated to your account. This is normally done while initializing the Thinger instance in the code. That is, when you define the `thing` instance. Replace here your `username`, `deviceId`, and `deviceCredential` with the values you have registered in the cloud.

```cpp
ThingerWifi thing("username", "deviceId", "deviceCredential");
```

## Adding Resources

In the Thinger.io platform, each device can define several resources. You can think that a resource is anything you can sense or actuate. For example, a typical resource will be a sensor value like temperature or humidity, or a relay that turns on and off a light. This way, you should define the resources you need to expose over the Internet.

All resources must be defined inside the `setup()` method of the Arduino sketch. This way the resources are configured at the beginning, but can be accessed later as necessary.

There are three different types of resources, which are explained in the following sections.

### Input Resources

If you need to control or actuate your IoT device, it is necessary to define an input resource. In this way, an input resource is anything that can provide information to your device. For example, it can be a resource for turning on and off a light or a relay, change a servo position, adjust a device parameter, etc.

To define an input resource it is used the operator `<<` pointing to the resource name, and it uses a C++11 Lambda function to define the function.

The input resource function takes one parameter of type `pson` that is a variable type that can contain booleans, numbers, floats, strings, or even structured information like in a JSON document.

The following subsections will show how to define different input resources for typical use cases.

#### _**Turn on/off a led, a relay, etc**_

This kind of resources only requires an on/off state so it can be enabled or disabled as required. As the `pson` type han hold multiple data types, we can think that the `pson` parameter of the input function is like a boolean.

So, inside the `setup` function you can place a resource called `led` \(but you can use any other name\), of input type \(using the operator `<<`\), that takes a reference to a `pson` parameter. This example will turn on/off the digital pin 10 using a ternary operator over the `in` parameter.

```cpp
thing["led"] << [](pson& in){
  digitalWrite(10, in ? HIGH : LOW);
};
```

#### _**Modify a servo position**_

Modifying a servo position is quite similar to turning on/off a led. In this case, however, it is necessary to use an integer value. As the `pson` type can hold multiple data types, we can still use the `pson` type as an integer value.

```cpp
thing["servo"] << [](pson& in){
    myServo.write(in);
};
```

#### _**Update sketch variables**_

You can use the input resources also for updating your sketch variables, so you can change your device behaviour dynamically. This is quite useful in some situations where you want to temporary disable an alarm, change the reporting intervals, update an hysteresis value, and so on. In this way, you can define additional resources to change your variables.

```cpp
float hysteresis = 0; // defined as a global variable
thing["hysteresis"] << [](pson& in){
    hysteresis = in;
};
```

#### _**Pass multiple data**_

The `pson` data type can hold not only different data types, but also is fully compatible with JSON documents. So you can use the pson data type to receive multiple values at the same time. This example will receive two different floats that are stored with the `lat` and `lon` keys.

```cpp
thing["location"] << [](pson& in){
    float lat = in["lat"];
    float lon = in["lon"];
};
```

#### _**Show Input Resources State in Dashboards and API**_

The Dashboards or API works in a way that when you open them, they query the associated resources to correctly print its current state, i.e., the switch is on or off. In this way, when the API or a Dashboard is open, each associated input resource is called, receiving empty data in the call, as there is no intention to control the resource \(the pson input will be empty\).

So, how the Dashboards or the API knows what is the current state of an input resource? The resource must set its current state in the input parameter, if it is empty, or use the input value if there is one. This way, we can obtain three different things: query the current resource state \(without modifying it\), modify the current resource state, and obtain the expected input on the resource \(this is how the API explorer on the device works\).

Therefore, a correct input resource definition that actually allows to display the current state of the resource in a Dashboard or in the API, will be like this example code.

```cpp
thing["resource"] << [](pson& in){
    if(in.is_empty()){
        in = currentState;
    }
    else{
        currentState = in;
    }
}
```

This sample code basically returns the current state \(like a boolean, a number, etc\) if there is no input control, or use the incoming data to update the current state. This can be easily adapted for controlling a led, while showing its current state in the dashboard once opened or updated.

```cpp
thing["led"] << [](pson& in){
    if(in.is_empty()){
        in = (bool) digitalRead(pin);
    }
    else{
        digitalWrite(pin, in ? HIGH : LOW);
    }
}
```

Note: for controlling a digital pin just use the method explained in the Easier Resources Section.

### Output Resources

Output resources should be used in general when you need to sense or read a sensor value, like temperature, humidity, etc. So the output resources are quite useful for extracting information from the device.

To define an output resource it is used the operator `>>` pointing out of the resource name, and it uses a C++11 Lambda function to define the output function.

The output resource function takes one parameter of `pson` type that is a variable type that can contain booleans, numbers, floats, strings, or even structured information like in a JSON document.

The following subsections will show how to define different output resources for typical use cases.

#### _**Read a sensor value**_

Defining an output resource is quite similar to defining an input resource, but in this case it is used the operator `>>`. In the callback function we can fill the out value with any value we want, like in this case the output from a sensor reading.

```cpp
thing["temperature"] >> [](pson& out){
      out = dht.readTemperature();
};
```

#### _**Read multiple data**_

In the same way the input resources can receive multiple values at the same time, the output resources can also provide multiple data. This is an example for providing both latitude and longitude from a GPS.

```cpp
thing["location"] >> [](pson& out){
      out["lat"] = gps.getLatitude();
      out["lon"] = gps.getLongitude();
};
```

#### _**Read sketch variables**_

If your sketch cannot provide a single sensor reading, as it is doing some kind of data integration, an output resource can be used also for reading your sketch variables, where the computed result is updated frequently.

```cpp
float yaw = 0; // defined as a global variable
thing["yaw"] >> [](pson& out){
      out = yaw;
};
```

### Input/Output Resources

The last resource type is a resource that not only takes an input or an output, but takes both parameters. This is quite useful when you want to read an output that depends on a input, i.e., when you need to provide a changing reference value to a sensor.

This kind of resources are defined with the operator `=`. In this case the function takes two different `pson` parameters. One for input data and another one for output data. This example provides an altitude reading using the BMP180 Sensor. It takes the reference altitude as input, and provides the current altitude as output.

```cpp
thing["altitude"] = [](pson& in, pson& out){
    out = bmp.readAltitude(in);
};
```

You can also define more complex input/output resources, that takes several input values, to provide also multiple output values, like in this example that takes `value1` and `value2` to provide the `sum` and `mult` values.

```cpp
thing["in_out"] = [](pson& in, pson& out){
    out["sum"] = (long)in["value1"] + (long)in["value2"];
    out["mult"] = (long)in["value1"] * (long)in["value2"];
};
```

### Resources without parameters

It is also possible to define resources that does not require any input nor generates any output. They are just like callbacks that can be executed as you want, for example to reboot the device, or do some required action.

In this case, the resource is defined as a function without any input or output parameters.

```cpp
thing["resource"] = [](){
    // write here your execution code
};
```

## Easier Resources

The client library also includes some useful syntactic sugar definitions for declaring resources more easily without having to think in input or or output resources. This syntactic sugar features are macros that are expanded automatically to define the resources in the standard way.

The advantage of using this kind of definitions is that your resources will be able to handle state when you query them from the API. For example, if you have a digital pin enabled or disabled, you will be able to see its current state both in the API explorer or in a dashboard.

### Control a digital pin

This kind of resources will allow defining a resource for declaring a control over a digital pin, so you can alternate over on/off states, that can be used for controlling a led, a relay, a light, etc.

It is required to define the digital pin as OUTPUT in your setup code, or the resource will not work properly.

```cpp
thing["relay"] << digitalPin(PIN_NUMBER);
thing["relay"] << invertedDigitalPin(PIN_NUMBER);
```

### Define Output Resources

This kind of resources will allow defining a resource for declaring a read-only resource, like a value obtained from a sensor, or a given variable in our sketch.

In this example we are defining a resource that exposes a sensor reading, like the DHT11 sensor temperature.

```cpp
thing["temperature"] >> outputValue(dht.readTemperature());
```

But it is also possible to define a output resource for any global variable in our sketch.

```cpp
thing["variable"] >> outputValue(myVar);
```

### Modify Sketch Variables

Our sketch usually defines some parameters or variables that are used inside the loop code. This kind of resources are normally used to handle or control the execution behaviour. With this kind of resources we can modify any parameter we want to expose, like a float, an integer, a boolean, etc.

In this example it is possible to remotelly modify the boolean `sdLogging` variable defined as a global variable.

```cpp
thing["logging"] << inputValue(sdLogging);
```

It is also possible to define a callback function to know when the variable has changed, so we can perform any other action. For this use case, define the resource as the following to have some code executed when the `hysteresisVar` changes.

```cpp
thing["hysteresis"] << inputValue(hysteresisVar, {
    // execute some code when the value change
    Serial.println("Hystereis changed to: ");
    Serial.print(hysteresisVar);
});
```

### Servo control

It is also possible to define a resource for controlling a servo instance. This way, the defined resource will automatically handle your servo instance, reading its current position, or changing to a new one according to the API interactions.

For defining a servo resource just define and initialize your servo as usual, and then use the declared instance in the resource definition.

```cpp
thing["servo"] << servo(myServoInstance);
```

## Communication between devices

In Thinger.io, it is possible that devices can communicate between them. There are two possibilities here. One is the communication between devices from the same account, and the other is the communication between devices from different accounts. Here we describe the two different approaches:

### Same account communication

For this use case, in which both devices belongs to the same user account, there is an specific method that allows devices to communicate with other devices with low latency and simple codification. this communication can contain data or not \(it is possible to make an empty call\). lets Suppose that we have two devices: `deviceA` and `deviceB`, and we want to communicate both calling from `deviceB`to an specific `deviceA` input resource. We can use "thing.call\_device\(,\);" as shown in the example below:

The `deviceA` defines a resource like in the following example.

```cpp
setup(){
    thing[“resource_On_A”] = [](){
        Serial.println("Someone is calling me!");
    };
}
```

`deviceB` can easily call this resource and send data to it by running the following command.

```cpp
loop(){
    thing.handle();
    // be sure to call it at an appropiate rate
    thing.call_device("deviceA", "resource_On_A");
}
```

On the other hand, if we want to send the message with a `pson` payload in order to share data between devices. In this case, the `deviceA` will need to define a resource with some expected input

```cpp
setup(){
    thing[“resourceOnA”] << [](pson& in){
        int val1 = in["anyValue1"];
        float val2 = in["anyValue2"];
        // Work with the updated parameters here
    };
}
```

Then `deviceB` can call this method providing the appropriated input by defining a `pson` type that is filled with the same keys used on `resourceOnA`, as shown in the code below:

```cpp
loop(){
    thing.handle();
    // be sure to call it at an appropiate rate
    pson data;
    data["anyValue1"] = 3;
    data["anyValue2"] = 43.1;
    thing.call_device("deviceA", "resourceOnA", data);
}
```

`deviceB` can also call this method by providing the information from an defined resource that generates the information, in this case, the call is similar as the previous example, but using the resource as the data source.

```cpp
setup(){
    thing["resourceName"] >> [](pson& out){
        out["anyValue1"] = 3;
        out["anyValue2"] = 43.1;
    };
}

loop(){
    thing.handle();
    // be sure to call it at an appropiate rate
    thing.call_device("deviceA", "resourceOnA", thing["resourceName"]);
}
```

### Communication between different accounts

If we want to communicate devices from different accounts, we can do that through calling an endpoint of type `Thinger.io Device Call`. Just register an endpoint of this type in the console, like in the following example.

![](.gitbook/assets/device_call.png)

In this case it is required to define different parameters in the endpoint:

* Endpoint Identifier: The endpoint id that the device will use for calling the device.
* Device Owner: The device owner username.
* Device Identifier: The device id of the other account.
* Resource Name: The resource on the device to be called.
* Device Access Token: A device token generated in the other account for granting external access to the device.

Once defined, the device will be able to call the endpoint, as explained in the following section. It basically consists on calling the `call_endpoint`method.

```cpp
thing.call_endpoint("DeviceACall");
```

## Using Endpoints

In Thinger.io, an endpoint is defined as some kind of external resource that can be accessed by the device. With the endpoints feature, devices can easily send emails, SMS, push data to external WebServices, interact with IFTTT, and any general action that can be made by using WebHooks \(Calling HTTP/HTTPS URLs\).

Calling an endpoint is so easy from the Arduino sketch, as it is only required to call the `call_endpoint` method over the `thing` variable.

```cpp
thing.call_endpoint("endpoint_id");
```

You can simply call an endpoint to make some action like sending a predefined email, or also call the endpoint with some data, which is specially useful when you are using third party services that consume your devices data.

**You should take extra attention while calling resources, and call them at an appropriate rate. Otherwise you can consume easily your available data, receive hundred of emails, or consume your API calls in third-party services.**

### Calling Endpoints

In this case we will see a simple example to send an email alert based on a temperature value. For this example, we have configured an email endpoint called `high_temp_email` that contains some warning text about the temperature. For this case we do not want to check the temperature every millisecond, so we are introducing some variables to control the sensing and warning frequency. In this example, the temperature is checked every hour, and if it is above 30ºC, it will call the endpoint called `high_temp_email` which will send us an email with the predefined text. It is important here to **do not add delays** inside the loop method, as it will prevent the required execution of the `thing.handle()` method, so we are using here a non-blocking delay based on the `millis()` function.

```cpp
unsigned long lastCheck = 0;
loop(){
    thing.handle(); // required thing handle
    unsigned long currentTs = millis();
    if(currentTs-lastCheck>=60*60*1000){
        lastCheck = currentTs;
        if(dht.readTemperature()>30){
            thing.call_endpoint("high_temp_email");
        }
    }
}
```

You can be so creative here and call your endpoints when the presence sensor makes a detection, when your humidity sensor reports that there is no water in your plants, when the location of a device is not as expected, and many other stuff. Other interesting way of using endpoints is by its integration with IFTTT, so you can interact with multiple third-party services!

### Sending Data to Endpoints

Sending data to an endpoint \(in JSON format\) is also quite easy. We need to call also the `call_endpoint` method, but in this case adding some information based on the `pson` data format, which will be automatically converted to JSON. For example, if we want to report data to a third party service like Keen.io, we can create such kind of endpoints in the console. Once configured, we can call the endpoint with our readings, for example with humidity and temperature values from a DHT sensor.

```cpp
// be careful of sending data at an appropriate rate!
pson data;
data["temperature"] = dht.readTemperature();
data["humidity"] = dht.readHumidity();
thing.call_endpoint("keen_endpoint", data);
```

You can also send data based on a defined resource, i.e., suppose you have a resource that already serves the temperature and humidity. It is possible to reuse this definition for sending this same data to the endpoint, without having to redefine the sensor reading, like in the following example.

```cpp
setup(){
    // defined resource in the setup for reading a sensor value
    thing["data"] >> (pson& out){
        out["temperature"] = dht.readTemperature();
        out["humidity"] = dht.readHumidity();
    }
}

loop(){
    // be careful of sending data at an appropriate rate!
    thing.call_endpoint("endpoint", thing["data"]);
}
```

### Email Type Endpoint Example

This is a simple example, applied to email type endpoint, with custom body

```cpp
setup()
{
 thing["Level"] >> outputValue(actualRelative);
}
loop()
{
 if(actualLevel>UpperLevel && endpointUpperFlag)
   {
    thing.call_endpoint("endpoint_id",thing["Level"]);
    endpointUpperFlag=0;
   }
}
```

Notice that there are a variable that limitates the run of this "if" just once, its important to define any condition or method to warrantee that this kind of enpoint call is executed just once \(or at appropiate rate\), because it can get a lot of emails generated by the microcontroller across thinger.io platform.

At endpoint configuration, in the custom body email, must add double brackets "" to invoke the variable sent by the microcontroller, in our example, we used the following body

"The actual level is %"

And receiving an email with the text:

The actual level is 80.34%

## Using Data Buckets

Thinger.io provides an easy to use and extremely scalable virtual storage system, that allows to store long term device data from device output resources. This information can be used to be ploted in dashboards, or can be exported in different formats for offline processing or third party Data Analysis process.

### From Device Resource

It is not necessary to implement specific codification in your device firmware to start storing data in a data bucket, because they will retrieve information from your output resources, just configure your Data Bucket to set the source and sampling interval as it is explained in our Console documentation at: [http://docs.thinger.io/console/\#data-buckets](http://docs.thinger.io/console/#data-buckets)

### Streaming Resource Data

It is also possible to let he device stream the information when required, i.e., by raising an event when detected. In this case, we can use the "Update by Device" option while configuring the bucket, and we will use the streaming resource instruction as described here:

Using a previous defined output reource, that was called for example \["location"\], it could be done like in the following code snippet.

```cpp
 void loop() {
  thing.handle();
  // use your own logic here to determine when to stream/record the resource.
  if(requires_recording){
      thing.stream("location");
  }
}
```

### From Write Call

This option will allow setting the bucket in a state that it will not register any information by default, but it will just wait for writing calls, both from the Arduino library using the write\_bucket method, as shown here, or calling the REST API directly like done with Sigfox. This feature opens the option to register information in the same bucket from different devices, or store information from devices that are not connected permanently with the server, that are in sleep mode, or use a different technology like Sigfox.

Here is an example of an ESP8266 device writing information to a bucket using the write\_bucket function:

```cpp
void setup() {
  // define the resource with temperature and humidity
  thing["door_status"] >> [](pson &out){ 
    out["OPEN"] = (bool)digitalRead(SENSOR_PIN);
  };
}

void loop() { 
  // handle connection
  thing.handle();

  if(digitalRead(SENSOR_PIN)!=previous_status){
    // write to bucket BucketId when the door changes its status
    thing.write_bucket("BucketId", "door_status");
  }
  previous_status=digitalRead(SENSOR_PIN);
}
```

Note that this instruction will retrieve the \["door\_status"\] resource PSON, so it is also possible to call this function by attaching a custom PSON, as shown down below:

```cpp
void loop(){
  // handle connection
  thing.handle();

  if(digitalRead(SENSOR_PIN)!=previous_status){
    // write to bucket BucketId when the door changes its status
    thing.write_bucket("BucketId", "door_status");
  }
  previous_status=digitalRead(SENSOR_PIN);
}
```

## Streaming Resources

In Thinger.io you can open WebSockets connections against your devices, so you can receive sensor values, events, or any other information in real-time. The WebSockets are mainly used in the Dashboard feature of the Console, and are normally used for streaming resources at a fixed configurable interval. This functionality is available right out of the box when you define an output resource. However, if you want to transmit the information right when it is required, like when your device detects a movement, presence, etc., you must program some code, that is quite similar to calling an endpoint.

In this case, you must detect when you want to stream the event, like the accelerometer value is over some threshold, your presence sensor is making a detection, or the compass heading is changing. This is up to you when it is necessary to stream new data. Streaming resources also requires that another endpoint is connected listening for them \(i.e., from a WebSocket connection\), so if there is no one listening for this data, the data is not sent. This is handled automatically by the client library and the server, therefore it is safe to stream data always, as the device will transmit the information only when there is a destination.

The following example will report the compass heading in real-time if the heading value changes more than 1 degree.

![](.gitbook/assets/esp8266-real-time-websockets.gif)

```cpp
void setup(){
  thing["heading"] >> [](pson& out){     
    out = getHeading();
  };
}

float previousHeading = 0;
void loop() {
  thing.handle();
  float currentHeading = getHeading();
  if(abs(currentHeading-previousHeading)>=1.0f){
    thing.stream(thing["heading"]);
    previousHeading=currentHeading;
  }
}
```

## Enabling Debug Output

Thinger.io library provides extensive logging of its activities, which is especially useful when one needs to troubleshoot authentication and Wi-Fi connectivity issues. Include the following definition in your sketch, but _make sure it comes first, before any other includes_ \(it was reported to cause crashes on some boards otherwise\).

```text
#define _DEBUG_

// the rest of your sketch goes here
```

It is also necessary to enable `Serial` communication, as all the debugging information is displayed over Serial. So enable it in your sketch in the setup method.

```text
void setup() {
  Serial.begin(115200);
}
```

## ESP8266 Deep Sleep and SmartConfig

`SmartConfig` allows one to configure board's WiFi credentials via an external device on the same network \(e.g. smartphone or another wifi client\). This means no sensitive information goes into a sketch nor in a config file on a device.

`Deep Sleep` is a special mode of ESP8266 which allows it to shut down most of the circuits and wake up after some configurable time. For deep sleep \(and wake up\) to work properly, one has to connect `GPIO16` \(usually a `D0` on dev boards\) and `RST` pins.

However, some boards chose to wire a built-in LED to the same `D0` pin, and will go into a crash loop when using `ThingerSmartConfig` class, which uses the LED as a debugging aid at runtime. The solution is to use an overloaded constructor and disable its use of the LED.

```text
ThingerSmartConfig thing(USERNAME,
                         DEVICE_ID,
                         DEVICE_CREDENTIAL,
                         false); // required for deep sleep
```

## Interacting

It is possible to easily interact with your devices within minutes once you have defined your resources and the device is connected to the platform. There are several ways of interaction, like creating dashboards, accessing with a mobile application \(currently only in Android\), accessing from the API explorer, or calling the cloud API directly.

### Android

One of the easiest ways to access your devices is to simply issue a device token to grant access to the device. When you generate a device token you can restrict the shared resources, and the token expiration time, so you can share some device functionality with other people safely. The idea is that you can use this device token and scan it as a QR code in the Android application.

![](.gitbook/assets/token.png)

> Generate a device token in your cloud console, so you can easily interact from your phone within minutes.

#### Mobile APP

The current version of the [Android APP](https://play.google.com/store/apps/details?id=io.thinger.thinger) does not require any kind of login to interact with your devices. Simply scan your token as a QR code and your phone will be able to interact with your device for reading sensor values, changing led or relays states, and so on.

![](.gitbook/assets/phone.png)

> In the current version of the Android APP, you can interact with your device resources out of the box. Just scan the QR Code and start interacting with your resources directly from the Internet.

#### Android Wear

The Android APP supports Android Wear devices for controlling and reading values from your devices! The current version is still under testing and therefore it can still have some errors. At this moment the Wear version allows reading values \(not composed values at this moment\), execute resources, and control boolean resources. It is so cool to turn on and off things from the SmartWatch!

To get the devices available in the SmartWatch, it is not required to do anything special. As usually, just scan the QR code from the platform with the mobile phone.

![](.gitbook/assets/android_wear_1.png) ![](.gitbook/assets/android_wear_2.png) ![](.gitbook/assets/android_wear_3.png) ![](.gitbook/assets/android_wear_4.png)

### Cloud Console

The Cloud Console add some support for out of the box device interaction. You can interact with your devices from the API explorer, or also create real-time dashboards to display the device information. It is not necessary to write additional code to support this kind of interaction, just only declare your resources in your device and you will be able to access them directly in the console.

So this section will review different interaction approaches from the cloud console.

#### Dashboards

In the cloud console you can easily create and configure real-time dashboards for display your device data, like sensors. You just need to select the device, an associated resource, and select the update method, that can be a configurable sampling interval, or update as required by device.

TODO Reference to dashboards in cloud console

![](.gitbook/assets/dashboards.gif)

> Visualize your sensor data in real-time using the dashboards. Add different widgets, like maps, charts, donut charts, text, etc. This example is monitoring several sensors like pressure, temperature, humidity, compass, and altitude. So you can configure the dashboards with the devices and resources you want.

#### API Explorer

Another way of interaction with your device is trhough the API Explorer. Every defined resource in your device is automatically accesible from the API Explorer, so you can read data, send commands or values, call functions, etc.

This feature is so useful if you want to interact with your device using the Server API, so you can easily visualize the device exposed API, and the expected input and output. Also it is very important while writing code on your device, so you can easily check if your resources are working as expected, like reading a sensor value, turning on and off a relay, and so on.

TODO Reference to API Explorer in cloud console

Your browser does not support the video tag.

> Every defined resource in your device, like a sensor, a relay, a light, and so on, can be accessed out of the box from the API Explorer. This example is turning on and off a led directly from the explorer.

### Server API

In progress... Refer to the Server API documentation

