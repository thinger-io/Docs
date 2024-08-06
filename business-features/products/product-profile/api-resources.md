---
description: Create custom API devices for interacting with your devices.
---

# API Resources

Using API resources inside a Product provides unified API access for various devices and protocols, including payload processing capabilities. These features simplify the integration of different devices and systems, making it easier to test and manage them.&#x20;

This is especially useful for MQTT or HTTP devices, as they will behave exactly the same as IOTMP devices inside Thinger.io, so they can be used easily on dashboards, use the API explorer, etc.

{% hint style="info" %}
Any API Resource defined inside a Product will generate a REST API on each device.
{% endhint %}

The advantages of designing IOT products to become accessible over REST APIs are the following:

* Scalability: REST APIs are designed to be scalable, making them well-suited for IoT applications that involve large numbers of devices.
* Flexibility: REST APIs are flexible, allowing them to accommodate changes in device capabilities and requirements over time.
* Interoperability: REST APIs use standard HTTP methods, making them interoperable with a wide range of systems and applications.
* Easy to use: REST APIs use simple and well-documented standards, making it easy for developers to integrate them into their applications.
* Security: REST APIs are secured using standard security measures, such as SSL/TLS encryption and OAuth authentication.
* Easy to test: REST APIs can be easily tested using tools such as the API explorer or Postman, making it easier to debug and troubleshoot integration issues.

By default, the Product profile presents an API Resources table that is empty. Like in the following image:

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Products - API Resources section on a Product Profile</p></figcaption></figure>

To create a new API Resource, click on `Add` button from the table, and a popup will appear for its configuration. It is quite similar to the process of configuring a [Property](properties.md) or a [Bucket](buckets.md) for the product.

<figure><img src="../../../.gitbook/assets/image (49).png" alt=""><figcaption><p>Products - API Resource creation</p></figcaption></figure>

The following sections describe the different options available.

## API Resource  Identifier

Each product can define any number of API resources, and each resource is uniquely identified by its id. A resource identifier can be shared among multiple devices and/or products, as they are defined at the device level.

Choose a resource identifier that represents its purpose, i.e., it can be used to control device actions: "power", "reboot", "configure", or for reading sensor data:  "temperature", ·"humidity".&#x20;

<figure><img src="../../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption><p>Products - API Resource Identifier</p></figcaption></figure>

## API Resource Request

Each API Resource can define a target destination, where the incoming request payload will be forwarded, i.e., for its transmission to a topic, to a device resource, for calling a custom function, etc.&#x20;

The target options available are the following:

<table><thead><tr><th width="184">Target</th><th>Description</th></tr></thead><tbody><tr><td>None</td><td>The request will not reach any destination.</td></tr><tr><td>Device Resource</td><td>The request will be forwarded to a connected IOTMP device resource.</td></tr><tr><td>Device Stream</td><td>The request will generate a device resource stream event that can be used by an event subscriber, or used within the product profile for writing to a data bucket, updating a property, and calling to an endpoint. (HTTP devices).</td></tr><tr><td>Device Property</td><td>The request will be forwarded to a Device Property.</td></tr><tr><td>Device Topic</td><td>The request will be published on a given topic (MQTT).</td></tr><tr><td>Device Function</td><td>The request will be forwarded to a Product Function.</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (51).png" alt=""><figcaption><p>Product - APi Request Configuration</p></figcaption></figure>

### None

Setting the target API Request to `None` indicates that the API resource is not expecting any input and the request will not reach any destination. This is useful if the [API Resource Response](api-resources.md#api-resource-response) is returning a value, i.e., from a device resource, a property, from a function call, etc.

### Device Resource

Setting the target API Request to `Device Resource` configures the Product to automatically forward the incoming request to a Device Resource. A device resource should be any [input resource](../../../coding-guide.md#input-resources) defined inside the device (i.e., using Thinger.io Arduino or Linux client).

The configuration fields are:

* **Resource Name**: Used to specify the device resource name that will be called inside the device.
* **Payload**: Used to configure the final payload that will be forwarded to the device resource. The value that arrives from the API request becomes available at the `{{payload}}` placeholder, which is the default configuration. Take a look at the [Payloads](payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../../../.gitbook/assets/image (52).png" alt=""><figcaption><p>Product - API Resource configuration for Device Resource target</p></figcaption></figure>

#### Available Payload Placeholders

The placeholders that are available for the Payload configuration are:

* **payload**: Hold the payload captured from the API request.
* **user:** Hold the username that is calling the API request.
* **device**: Hold the device identifier used in the API request.
* **resource**: Hold the API resource name that is being called on the API request.
* **property**: Online fetch data from a property, i.e., `{{property.propertyId}}`, `{{property.propertyId.value}}`.
* **api**: Online fetch data from an API, i.e., `{{api.resourceA}}`, `{{api.resourceB.value}}`.

### Device Property

Setting the target API Request to `Device Property` configures the Product to automatically store the incoming request payload to a Device Property.

The configuration fields are:

* **Property**: Used to specify the device property name that will be written.
* **Payload**: Used to configure the final payload that will bes stored on the device property. The value that arrives from the API request becomes available at the `{{payload}}` placeholder, which is the default configuration. Take a look at the [Payloads](payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption><p>Product - API Resource configuration for Device Property target </p></figcaption></figure>

#### Available Payload Placeholders

The placeholders that are available for the Payload configuration are:

* **payload**: Hold the payload captured from the API request.
* **user:** Hold the username that is calling the API request.
* **device**: Hold the device identifier used in the API request.
* **resource**: Hold the API resource name that is being called on the API request.
* **property**: Online fetch data from a property, i.e., `{{property.propertyId}}`, `{{property.propertyId.value}}`.
* **api**: Online fetch data from an API, i.e., `{{api.resourceA}}`, `{{api.resourceB.value}}`.

### Device Topic

Setting the target API Request to `Device Topic` will configure the Product to automatically publish data in the provided topic (MQTT).

The configuration fields are:

* **Topic**: Used to specify the topic that will be used as the target destination. Use topic placeholders like `{{device}}` or MQTT single-level wildcards `+` to capture data from multiple devices. For example, devices providing information on the topic `my_product/device_aabbccdd/temperature` can be configured both as `my_product/{{device}}/temperature` or `my_product/+/temperature`.
* **Payload**: Used to configure the resulting value that will be published to the topic. The value that arrives from the API request becomes available at the `{{payload}}` placeholder, which is the default configuration. Take a look at the [Payloads](payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption><p>Product - API Resource configuration for Device Topic target </p></figcaption></figure>

#### Available Payload Placeholders

The placeholders that are available for the Payload configuration are:

* **payload**: Hold the payload captured from the API request.
* **user:** Hold the username that is calling the API request.
* **device**: Hold the device identifier used in the API request.
* **resource**: Hold the API resource name that is being called on the API request.
* **property**: Online fetch data from a property, i.e., `{{property.propertyId}}`, `{{property.propertyId.value}}`.
* **api**: Online fetch data from an API, i.e., `{{api.resourceA}}`, `{{api.resourceB.value}}`.

### Product Function

Setting the target API Request to `Product Function` will configure the Product to automatically call a function inside the Product Script.

The configuration fields are:

* **Function**: Used to specify the target function name to be called.
* **Payload**: Used to configure the resulting value that will be used in the function call. The value that arrives from the API request becomes available at the `{{payload}}` placeholder, which is the default configuration. Take a look at the [Payloads](payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../../../.gitbook/assets/image (21).png" alt=""><figcaption><p>Product - API Resource configuration for Product Function target </p></figcaption></figure>

#### Available Payload Placeholders

The placeholders that are available for the Payload configuration are:

* **payload**: Hold the payload captured from the API request.
* **user:** Hold the username that is calling the API request.
* **device**: Hold the device identifier used in the API request.
* **resource**: Hold the API resource name that is being called on the API request.
* **property**: Online fetch data from a property, i.e., `{{property.propertyId}}`, `{{property.propertyId.value}}`.
* **api**: Online fetch data from an API, i.e., `{{api.resourceA}}`, `{{api.resourceB.value}}`.

## API Resource Response

Each API Resource can define a source response that is used to return a response payload to the REST API call. For example, it is possible to generate a response based on a property value, a device resource, a device topic, or even generate a response based on the request response.

The source options available are the following:

* **None:** The response will not generate any payload.
* **Device Resource**: The response payload will be based on an IOTMP device resource.&#x20;
* **Device Property**: The response payload will be based on a Device Property.
* **Device Topic**: The response payload will be based on the data captured from a topic (MQTT).
* **Product Function**: The response payload will be based on de data returned by a Script function.
* **Request Response**: The response payload will be based on the data returned by the request.

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption><p>Product - API Response Configuration</p></figcaption></figure>

### None

Setting the source API Response to `None` indicates that the API resource is not will not generate any output, i.e., will return an empty body.

### Device Resource

Setting the source API Response to `Device Resource` configures the Product to automatically read the response value from a Device Resource.

The configuration fields are:

* **Resource Name**: Used to specify the device resource name that will be called inside the device.
* **Payload**: Used to configure the final payload that will be returned to the API caller. The value that arrives from the Device resource becomes available at the `{{payload}}` placeholder, which is the default configuration. Take a look at the [Payloads](payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../../../.gitbook/assets/image (15).png" alt=""><figcaption><p>Product - API Resource configuration for Device Resource source</p></figcaption></figure>

#### Available Payload Placeholders

The placeholders that are available for the Payload configuration are:

* **payload**: Hold the payload captured from the Device Resource.

### Device Property

Setting the source API Response to `Device Property` configures the Product to automatically read the response value from a Device Property.

The configuration fields are:

* **Property**: Used to specify the device property name that will be used to generate the response payload.
* **Payload**: Used to configure the final payload that will be returned to the API caller. The value that arrives from the Device property becomes available at the `{{payload}}` placeholder. Take a look at the [Payloads](payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../../../.gitbook/assets/image (44).png" alt=""><figcaption><p>Product - API Resource configuration for Device Porperty source</p></figcaption></figure>

#### Available Payload Placeholders

The placeholders that are available for the Payload configuration are:

* **payload**: Hold the payload captured from the Device Property.

### Device Topic

Setting the source API Response to `Device Topic` configures the Product to automatically read the response value from a Device Topic. This response source is specifically designed for request-response over MQTT, where the request publishes on a topic, and the device publishes a response on a different topic. The product will be listening to a topic until it receives a response (or timeout after 15 seconds).

The configuration fields are:

* **Topic**: Used to specify the topic that will be used as the source value for the response. Use topic placeholders like `{{device}}` or MQTT single-level wildcards `+` to capture data from multiple devices. For example, devices providing information on the topic `my_product/device_aabbccdd/response` can be configured both as `my_product/{{device}}/response` or `my_product/+/response`.
* **Payload**: Used to configure the final payload that will be returned to the API caller. The value that arrives from the topic becomes available at the `{{payload}}` placeholder, which is the default configuration. Take a look at the [Payloads](payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption><p>Product - API Resource configuration for Device Topic source</p></figcaption></figure>

#### Available Payload Placeholders

The placeholders that are available for the Payload configuration are:

* **payload**: Hold the payload captured from the response topic.

### Request Response

Setting the target API Response to `Request Response` will configure the Product to automatically return the value resulting from the target request. For example, if the API Resource was configured to target a Device Resource that provides an output, then, use a `Request Response` source to manage the data returned from the resource. It also applies when the API Resource is targeting Device Properties or Product Functions. It does not apply to Device Topic targets, as by default, MQTT does not provide any output on publish (user a [Device Topic](api-resources.md#device-topic-1) source instead).

The configuration fields are:

* **Payload**: Used to configure the final payload that will be returned to the API caller. The value returned from the API Resource Request becomes available at the `{{payload}}` placeholder. Take a look at the [Payloads](payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../../../.gitbook/assets/image (40).png" alt=""><figcaption><p>Product - API Rresource configuration for Request Response source</p></figcaption></figure>

#### Available Payload Placeholders

The placeholders that are available for the Payload configuration are:

* **payload**: Hold the payload captured from the API request.

## API Resource Settings

Each API Resource defined inside a Product can be enabled or disabled independently, for example, to temporarily disable an API endpoint. It can also include a description for a better understanding of its purpose, usage, or source.

<figure><img src="../../../.gitbook/assets/image (28).png" alt=""><figcaption><p>Product - API Resource configuration</p></figcaption></figure>
