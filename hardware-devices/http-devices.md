# HTTP DEVICES

In this section it is explained how to create and use an "HTTP device" profile at Thinger.io, that will be able to receive data via REST API from whatever source that can create a connection with Thinger.io Server and send an HTTP request, such as third party platforms or any devices without Thinger.io library on it source code, taking advantage of all Thinger.io features such as display real time data in dashboards, store in data buckets or process it using a plugin. 

This integration provides a bidirectional communication between Thinger.io and the data source by making use of HTTP request and response data, that consist on basic HTTP POST messages with JSON codificated data. 

![](../.gitbook/assets/image%20%2828%29.png)

{% hint style="info" %}
Note that this integration can't explode the 100% of Thinger.io features and benefits in terms of communication efficiency, real-time data and device administration, as it don't count with the required software functionalities, so it is strongly recommended integrating the devices using a Thinger.io software client if it is available for your device
{% endhint %}

## Working with HTTP devices

### Creating HTTP Device Profile

The first step is to work with this flexible interface consist on creating the HTTP device profile at the "devices" main menu tab and clicking on "new device" button. 

hablar del callback y la autorización... copiar esto en la zona q sea 

### Building the HTTP request

Cómo construir la llamada, con el bearer y el JSON

### Managing Callback Functionalities 

Ones the callback has been integrated with your system, it is possible to 

* **Store data in Buckets:** This feature allows to associate an existent data bucket to the device data in order to store it as is retrieved. 
* **Call Endpoints:** Allows to associate the device data with an Endpoint profile that will be called in real time when the data is processed by Thinger.io server. 
* **Set device properties:** This feature provides an easy way to store just the last-received data from a device or set an specific property, using Thinger.io server as a persistent memory for the device.  
* **Connection Timeout:** This parameter allows to establish a device connection timeout in minutes, so the platform can consider the device as "disconnected" after a fixed time without receiving messages. 
* **Manage authorization:** 

## Integrating the HTTP request in the data source

### Postman / HTTP request manager

### Curl request

### Programming \(arduino based\) 



