---
description: Automate product pipelines between device datapoints and Thinger.io resources
---

# Flows

Product flows enable automated data pipelines and event-driven workflows within devices. A flow acts as an **event-driven automation layer** that listens to device activity and triggers specific actions in response.

Flows automate the orchestration between different components of a device lifecycle. When a device performs an action—such as updating a property, subscribing to an MQTT topic, or receiving data through an endpoint—a flow can automatically trigger downstream events without manual intervention.

This capability allows products to remain generic and flexible, delegating device-specific logic to the flow layer while keeping the core product architecture clean and maintainable.

#### Common Use Cases

* **Property Update Cascade**: When a device property changes (e.g., `temperature` exceeds a threshold), publish a MQTT event with a custom payload.
* **Endpoint Handler**: automatically trigger pre-configured Thinger.io [endpoints](../../../features/endpoints-1.md) from any source, enabling seamless integration with external APIs and webhooks.
* **Auto Provision devices from non-HTTP data entrypoints**: get the device ID from a MQTT topic or create device instances dynamically when data arrives from message brokers.

By default, the Product profile presents a Flow table that is empty:

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption><p>Products - Flows section on a Product Profile</p></figcaption></figure>

### Defining a Flow

To create a new Flow, press de button `add` in the Flow section of the Product Profile configurator. Once selected, a pop-up with the flow configuration will appear:&#x20;

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption><p>Products - Flow Configurator</p></figcaption></figure>

## Flow Identifier

Following the schema of other product-profile configurations, each flow must have a unique identifier. This identifier is used internally by the system to reference and manage the flow.

Choose a resource identifier that represents its purpose, i.e. "Device Alarm Handler", "Message forwarder", etc.

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Products - Flows Identifier</p></figcaption></figure>

## Flow Source

The Source tab defines what event will trigger the flow. Thinger.io supports multiple trigger types to accommodate different integration patterns.

## Flow Target Tab

The Actions tab defines what happens when the flow is triggered. Multiple actions can be configured to execute sequentially when a single trigger event occurs.
