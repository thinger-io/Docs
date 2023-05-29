---
description: Store time series data from your devices with a few clicks.
---

# Buckets

A data bucket is a way to store time series data, i.e., data that is collected and recorded at regular time intervals over a period of time. This data is typically used to track changes in physical parameters, such as temperature, humidity, or pressure, over time. The collected data can then be analyzed to identify trends and patterns, and used to make decisions or take actions in real-time. Time series data is a critical aspect of IoT, as it enables organizations to monitor and control their connected devices and equipment more effectively.

Time series data can be used in a wide range of industries and applications, like:

* Predictive maintenance: time series data from IoT sensors can be used to monitor the health of equipment and predict when maintenance is required, reducing downtime and increasing efficiency.
* Energy management: time series data from smart meters can be used to optimize energy usage in buildings and reduce costs.
* Supply chain optimization: time series data can be used to monitor the movement of goods and optimize the supply chain, reducing waste and increasing efficiency.
* Healthcare: time series data from wearable devices can be used to monitor patients' health and provide insights for treatment and care.
* Environmental monitoring: time series data from sensors in the environment can be used to monitor air and water quality, and track changes in weather patterns and wildlife populations.

{% hint style="info" %}
Take a look at [Data Buckets](../../features/buckets.md) for more information.
{% endhint %}

Defining a Bucket inside a Product Profile allows scaling the buckets management. It can be used for:

* Automatic bucket provisioning and configuration.
* Automatic bucket ingestion from MQTT topic or IOTMP device resource for devices associated with the product.
* Automatic data tagging inside the bucket. Use just one bucket for multiple devices. Each measurement is tagged by device and its group, which can be easily filtered when using dashboards or querying data
* Data preprocessing before its insertion, i.e., modifying measurement units, filtering undesired data, etc.

By default, the Product profile presents a Buckets table that is empty. Like in the following image:

<figure><img src="../../.gitbook/assets/image (12) (1).png" alt=""><figcaption><p>Product Bucket - Buckets section on a Product Profile</p></figcaption></figure>

To create a new Bucket, click on `Add` button from the table, and a popup will appear for its configuration. It is quite similar to the process of configuring a [Property](../properties.md).

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption><p>Product Buckt - Bucket creation</p></figcaption></figure>

The following sections describe the different options available.

## Bucket Identifier

Each product can define any number of buckets, and each bucket is uniquely identified by its id. A bucket identifier **must be unique within the user account**, as they are created as regular buckets.

Choose a bucket identifier that represents its purpose, i.e., "productA\_temperature", "productA\_location", "productB\_consumption", etc.&#x20;

<figure><img src="../../.gitbook/assets/image (289).png" alt=""><figcaption><p>Product Bucket - Bucket Identifier</p></figcaption></figure>

{% hint style="danger" %}
Do not initialize the bucket manually, as it is automatically initialized by the Product on the first data item.
{% endhint %}

The bucket must **NOT** be created manually, as the product is able to initialize it automatically with the required configuration.  It is created with the arrival of the first data point. If the bucket is removed, it is created again automatically.

Any bucket with a fixed identifier, i.e., "productA\_temperature", will be created with two tags on it: `device` and `group`. As multiple devices can be writing to the same bucket simultaneously, tags let the platform differentiate all different sources (and groups) that are writing to the bucket:

<img src="../../.gitbook/assets/image (23).png" alt="" data-size="original">

* **device**: This will contain the device identifier where the data has been originated, i.e., the one that has published on a specific topic. It is automatically filled by the product, and should not arrive in the payload.
* **group**: This will contain the device group, as it can be useful for creating dashboards with aggregated data by groups, i.e., average consumptions of devices on floor 1. It is automatically filled by the product, and should not arrive in the payload.

#### Dynamic Bucket Identifiers

It is possible to use placeholders on the identifier, i.e., by using \{{device\}}, \{{user\}}, or \{{product\}}. It can be useful for generating dynamic bucket identifiers, i.e., one for each device within a product, i.e., by using `{{product}}_{{device}}`. It can be useful if it is required to isolate each device's data from other devices, i.e., because they are owned by different clients.

The placeholders that are available for the Identifier name are all values that come from the `device_resource_stream` or `device_topic_publish` event. It involves some common fields like:

* **device**: Hold the device identifier that is sending the information.
* **asset\_type**: Hold the device type identifier that is sending the information.
* **asset\_group**: Hold the device asset group identifier that is sending the information.
* **product**: Hold the device product identifier that is sending the information.
* **user:** Hold the device owner identifier that is sending the information.

Note that if the bucket identifier on the Product contains the `{{device}}` placeholder, the product will not initialize the device and group tags mentioned before, as now there is one bucket for each device.

## Bucket Source

A bucket can fetch time series data automatically from different sources:

* [**None**](buckets.md#none)**:** Used if the bucket should not be updated from any source.
* [**Device Resource**](buckets.md#device-resource): Used if the value to insert in a bucket must be taken from an IOTMP device resource.
* [**Device Topic**](buckets.md#device-topic): Used if the value to insert in a bucket must be taken from a topic (MQTT).

<figure><img src="../../.gitbook/assets/image (85).png" alt=""><figcaption><p>Product Bucket - Bucket Source Configuration</p></figcaption></figure>

### None

Setting the bucket source to `None` will prevent the bucket to be updated automatically.

### Device Resource

Setting the bucket source to `Device Resource` will configure the Product to automatically insert new data in the Bucket from a given Device Resource. A device resource is any [output resource](../../coding/coding-guide.md#output-resources) defined inside the IOTMP protocol (i.e., using Thinger.io Arduino or Linux client). As with any resource in IOTMP, it can be configured to be updated at a given sampling interval, or by letting the device update the value by itself.

The configuration fields are:

* **Resource Name**: Used to specify the device resource name that will be used as a source for inserting new values in the bucket.
* **Sampling Interval**: Used to specify the resource sampling interval (in seconds). The default is 0, which means that the device should update the value by itself via [stream](../../coding/coding-guide.md#streaming-resource-data) calls. Any other value greater than 0 will configure the Product to fetch the resource value at the provided interval. &#x20;
* **Payload**: Used to configure the resulting value that will be stored in the bucket. The value that arrives from the device resource becomes available at the `{{payload}}` placeholder, which is the default configuration. Take a look at the [Payloads](payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../../.gitbook/assets/image (271).png" alt=""><figcaption><p>Product Bucket - Device Resource configured as Bucket source</p></figcaption></figure>

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

Setting the bucket source to `Device Topic` will configure the Product to automatically insert data in the bucket from a given Device Topic (MQTT). This way, anytime a value is sent to the specified topic, it will be stored in the data bucket.

The configuration fields are:

* **Topic**: Used to specify the topic that will be used as a source for the new bucket values. Use topic placeholders like `{{device}}` or MQTT single-level wildcards `+` to capture data from multiple devices. For example, devices providing information on the topic `my_product/device_aabbccdd/temperature` can be configured both as `my_product/{{device}}/temperature` or `my_product/+/temperature`.
* **Payload**: Used to configure the resulting value that will be stored on the data bucket. The value that arrives from the topic becomes available at the `{{payload}}` placeholder, which is the default configuration. Take a look at the [Payloads](payloads.md) section to know more about the possibilities when defining payloads, like using Product Scripts for data conversion.

<figure><img src="../../.gitbook/assets/image (267).png" alt=""><figcaption><p>Product Bucket - Device Topic configured as Bucket Source</p></figcaption></figure>

#### Available Payload Placeholders

The placeholders that are available for the Payload configuration are all values that come from the `device_topic_publish` event, including:

* **payload**: Hold the data captured from the configured Device Topic.
* **user:** Hold the device owner identifier that is sending the information.
* **product**: Hold the device product identifier that is sending the information.
* **asset\_group**: Hold the device asset group identifier that is sending the information.
* **asset\_type**: Hold the device type identifier that is sending the information.
* **device**: Hold the device identifier that is sending the information.
* **ts**: Hold the event timestamp in milliseconds.

## Bucket Settings

Each bucket defined inside a Product can be enabled or disabled independently, for example, to temporarily disable the automatic data insertion from a topic or a device resource. It can include a description for a better understanding of its purpose, usage, or source.

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption><p>Product Bucket - Bucket configuration</p></figcaption></figure>
