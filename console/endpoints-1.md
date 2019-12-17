# ENDPOINTS

An endpoint, is the entry point to a service, a process, or any other destination. So, in Thinger.io an endpoint can be defined like a target destination that can be called by devices to perform any action, like sending an email, send a SMS, call a REST API, interact with IFTTT, call a device from a different account, or call any other HTTP endpoint.

Calling those endpoints directly by devices can be complex in small microcontrollers, and would require more bandwidth in devices. This way, Thinger.io can handle endpoints calls that can be requested directly by devices, activating them by using its identifier and passing any information required. It also adds some flexibility, as the endpoint request can be dinamically changed as necessary, while the deployed code in the device remains the same.

## Create Endpoint

To manage all your endpoints, it is necessary to access to the Endpoints section, by clicking in the following menu item:

![](../.gitbook/assets/endpointtab.PNG)

Then click on the Add Endpoint button that will open a new interface for entering the endpoint details, like in the following screenshot:

![](../.gitbook/assets/addendpoint.png)

Here it is necessary to configure different parameters:

* **Endpoint Id**: Unique identifier for your endpoint \(_the device must use this identifier for activating the endpoint_\). 
* **Endpoint Description**: Fill here any description or detailed information you need to keep about the dashboard.
* **Endpoint Type**: Defines the endpoint type, depending on the selected type, the endpoint will present different fields. In the following sections are described some of these types.

## Endpoint types

### Email Endpoint

An email endpoint allows sending emails from your devices. You can define your target email address, subject, and compose your email body.

The configurable parameters are the following:

* **Email Address**: The target email address of your message.
* **Email Subject**: The email subject.
* **Email Body**: Allows defining the email body, that can be a plain JSON text with the data sent from your device, or a custom body that can contain also information gathered from your device.

In the following screenshot, there is an example of an email endpoint that contains some text and variables that are filled when the device calls the endpoint, adding the current temperature and humidity reported by the device. Notice that `temperature` and `humidity` variables are closed inside double brackets `{{}}`, so the endpoint will be expecting this information to complete the body. In the following, there is some code examples calling this endpoint.

![](../.gitbook/assets/emailendpoint.png)

Calling endpoints is well documented [here](http://docs.thinger.io/arduino/#coding-using-endpoints-calling-endpoints), but it is basically required to call the endpoint by using the `call_endpoint` method, which requires the endpoint id, `ExampleEmail` in this example, and the optional data to be sent to the endpoint, which is a `pson` document \(quite similar to JSON\) with two keys named `temperature` and `humidity` holding the readings from a DHT sensor. In the following there is an example of such call.

```cpp
pson data;
data["temperature"] = dht.readTemperature();
data["humidity"] = dht.readHumidity();
thing.call_endpoint("ExampleEmail", data);
```

**Note**: If you want to include a single value in the email body, you can use the double bracket `{{}}` without any key, and send a `pson` document from the device with a single value. So, the following body:

```text
Temperature is: {{}} ºC
```

Can be filled with this call in the device:

```cpp
pson data = dht.readTemperature();
thing.call_endpoint("ExampleEmail", data);
```

### HTTP Endpoint

An HTTP endpoint is a generic type of endpoint that can be used to interact with any other web service or web application. So, this endpoint can be configured to make any HTTP request, by configuring the method, URL, headers, and body.

The configurable parameters are the following:

* **Request URL**: Configure the method \(GET, POST, PUT, PATCH, or DELETE\), and the request URL.
* **Request Headers**: It is possible to add headers to the request, that can be useful for adding authorizations, control caches, configure content type, etc.
* **Request Body**: The body can be either a custom body with an specific content, or a JSON payload with the information sent by the device. In a custom body it is possible to add custom variables, like shown in the email example. This way, it is possible to create contents in different formats like XML, SOAP, etc \(remember to add the adequate content-type in this case\).

![](../.gitbook/assets/httpendpoint.png)

## 

