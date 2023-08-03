---
description: Store device configuration, latest values, or any other metadata.
---

# Properties

A property in a device is a way to store arbitrary data, i.e., store device configuration, latest device state, or any other data required for the use case.&#x20;

Some of the use cases for the properties are:

* Store device configurations, like preferred sampling intervals, alarm thresholds, or enabled features, to mention a few.
* Store device location. Each device can adjust its location so it can be displayed on the device dashboard or assets maps.
* Store device information, like serial number, model, current firmware version, etc.
* Store device owner information, like notification email, notification configurations, etc.
* Store the latest device state, i.e., if it has been configured on/off, or the latest measurement from a sensor.

{% hint style="info" %}
Take a look at [Device Properties](../features/devices-administration.md#device-properties) for more information.
{% endhint %}

Defining a property inside a Product Profile allows scaling the properties management. It can be used for:

* Update a device property automatically from an MQTT topic or IOTMP device resource.
* Placing a default value for all devices inside a Product, that can be overridden by each device.&#x20;
* Use custom scripts to process the property value, i.e., modifying measurement units, filtering undesired data, etc.

By default, the Product profile presents a Properties table that is empty. Like in the following image:

<figure><img src="../.gitbook/assets/image (129).png" alt=""><figcaption><p>Product Property - Properties section on a Product Profile</p></figcaption></figure>

To create a new Property, click on `Add` button from the table, and a popup will appear for its configuration. It is quite similar to the process of configuring a [Bucket](product-profile/buckets.md).

<figure><img src="../.gitbook/assets/image (264).png" alt=""><figcaption><p>Product Property - Property creation</p></figcaption></figure>

The following sections describe the different options available.

## Property Identifier

Each product (and the devices) can have any number of properties, and each property is uniquely identified by its id. A property identifier can be shared among multiple devices and/or products, as they are defined at the device level.

Choose a property identifier that represents its purpose, i.e., "config", "location", "state", etc.&#x20;

<figure><img src="../.gitbook/assets/image (285).png" alt=""><figcaption><p>Product Property - Property Identifier</p></figcaption></figure>

## Property Source

A property can be updated automatically from different sources:

* [**None**](properties.md#none)**:** Used if the property should not be updated from any source.
* [**Device Resource**](properties.md#device-resource): Used if the property value must be updated from an IOTMP device resource.
* [**Device Topic**](properties.md#device-topic): Used if the property value must be updated from a topic (MQTT).

<figure><img src="../.gitbook/assets/image (265).png" alt=""><figcaption><p>Product Property - Property Source Configuration: </p></figcaption></figure>

### None

Setting the property source to `None` will prevent the property to be updated automatically. It can be useful to provide just a [Default value ](properties.md#property-default-value)or a common configuration for all devices.

### Device Resource

Setting the property source to `Device Resource` will configure the Product to automatically update the device property from a given Device Resource. A device resource is any [output resource](../coding-guide.md#output-resources) defined inside the IOTMP protocol (i.e., using Thinger.io Arduino or Linux client). As with any resource in IOTMP, it can be configured to be updated at a given sampling interval, or by letting the device update the value by itself.

The configuration fields are:

* **Resource Name**: Used to specify the device resource name that will be used as a source for the property value.
* **Sampling Interval**: Used to specify the resource sampling interval (in seconds). The default is 0, which means that the device should update the value by itself via [stream](../coding-guide.md#streaming-resource-data) calls. Any other value greater than 0 will configure the Product to fetch the resource value at the provided interval. &#x20;
* **Payload**: Used to configure the resulting value that will be stored on the property. The value that arrives from the device resource becomes available at the `{{payload}}` placeholder, which is the default configuration. Take a look at the [Payloads](product-profile/payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../.gitbook/assets/image (271).png" alt=""><figcaption><p>Product Property - Device Resource configured as Property source</p></figcaption></figure>

#### Available Payload Placeholders

The placeholders that are available for the Payload configuration are all values that come from the `device_resource_stream` event, which include:

* **payload**: Hold the data captured from the configured Device Resource.
* **user:** Hold the device owner identifier that is sending the information.
* **product**: Hold the device product identifier that is sending the information.
* **asset\_group**: Hold the device asset group identifier that is sending the information.
* **asset\_type**: Hold the device type identifier that is sending the information.
* **device**: Hold the device identifier that is sending the information.
* **ts**: Hold the event timestamp in milliseconds.

### Device Topic

Setting the property source to `Device Topic` will configure the Product to automatically update the device property from a given Device Topic (MQTT). This way, anytime a value is sent to the specified topic, it will be stored on the device property.

The configuration fields are:

* **Topic**: Used to specify the topic that will be used as a source for the property value. Use topic placeholders like `{{device}}` or MQTT single-level wildcards `(+)` to capture data from multiple devices. For example, devices providing information on the topic `my_product/device_aabbccdd/temperature` can be configured both as `my_product/{{device}}/temperature` or `my_product/+/temperature`.
* **Payload**: Used to configure the resulting value that will be stored on the property. The value that arrives from the topic becomes available at the `{{payload}}` placeholder, which is the default configuration. Take a look at the [Payloads](product-profile/payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../.gitbook/assets/image (267).png" alt=""><figcaption><p>Product Property - Device Topic configured as Property Source</p></figcaption></figure>

#### Available Payload Placeholders

The placeholders that are available for the Payload configuration are all values that come from the `device_topic_publish` event, including:

* **payload**: Hold the data captured from the configured Device Topic.
* **user:** Hold the device owner identifier that is sending the information.
* **product**: Hold the device product identifier that is sending the information.
* **asset\_group**: Hold the device asset group identifier that is sending the information.
* **asset\_type**: Hold the device type identifier that is sending the information.
* **device**: Hold the device identifier that is sending the information.
* **ts**: Hold the event timestamp in milliseconds.

## Property Default Value

It is possible to define a default property value for all devices within a Product. In the property configuration, there is a tab for the Default Value. The Default Value accepts any JSON value, i.e., for placing a default configuration.

<figure><img src="../.gitbook/assets/image (159).png" alt=""><figcaption><p>Product Property . Default Value configuration</p></figcaption></figure>

When a device is configured within a Product, it will automatically inherit the default product properties (in the same way it happens with Asset Types and Asset Groups). For example, the above property with a default value can be observed inside the device properties. The `Source` column in the properties table indicates that this property comes from a Product. Note that inherited properties from Products, Asset Types, or Asset Groups, cannot be removed or edited directly from the device properties to avoid undesired changes on all devices. However, it is possible to create a new property with the same identifier to override the default one inherited from Products, Asset Types, or Asset Groups.

<figure><img src="../.gitbook/assets/image (284).png" alt=""><figcaption><p>Product Property - Inherited default values from Product</p></figcaption></figure>

## Property Settings

Each property defined inside a Product can be enabled or disabled independently, for example, to temporarily disable the automatic update from a topic or a device resource. It can include a description for a better understanding of its purpose, usage, or source.

<figure><img src="../.gitbook/assets/image (60) (1).png" alt=""><figcaption><p>Product Property - Property configuration</p></figcaption></figure>
