---
description: Plugin to improve the integration of Node-RED into Thinger.io
---

# Node-RED

Node-RED is an Open Source project created by IBM to provide the Rule Engines market of a simple but powerful framework with an easy to use graphical programing interface, and a huge users community that has made more than 2.000 contributions. 

![](../.gitbook/assets/nodered-intro%20%281%29.png)

{% hint style="info" %}
Note: Plugins are only available for private Thinger.io instances.
{% endhint %}

## Plugin Features

* [x] Node-RED server hosted in the same private host as Thinger.io instace, runing 24/7 with no nodes limitation. 
* [x] Improved palette with ad-hoc Thinger.io nodes that provide support for:

  1\) Subscribing to device resources at a given interval \(device stream node\).

  2\) Reading a device resource when a event occurs \(device stream node\).

  3\) Sending data to a connected device \(device write node\).

  4\) Calling endpoints \(endpoint call node\).  
  5\) writing to data buckets \(bucket write node\).

## Node-RED Concepts

Node red essentially consist in a development tool with two basic components: a "flow editor" that consist on a graphical programing tool that can be launch over any web-browser and a backend with the rule engine server that is able to execute the flows. This system is really easy to learn and to use, however there are some concepts that you should know before start working with it:

### Nodes

The "Nodes" are the basic building blocks for creating flows. Each node contains its own configuration form and an specific behavior, that can be accessed by double clicking the block onces it has been introduced in the web editor canvas. There are few basic types of nodes:

#### **Inject Nodes**                                        ![](../.gitbook/assets/node_inject%20%281%29.png) 

This nodes allows automatically start a flow when an event is triggered, at regular interval specified in the properties form or manually over the web editor. The message sent by these nodes van have its payload and topic properties set. 

**Function, Change, Switch Nodes**  ![](../.gitbook/assets/node_function.png) 

Node-RED counts with some nodes that allows to work with the flow's payload using pre-configured tools or coding their behavior in JavaScript functions that can be created within the editor using a rich text editor. With this proposal, this nodes are provided with both input and output connectors.

**Output and Debug Nodes**              ![](../.gitbook/assets/node_debug%20%281%29.png) ****

~~This nodes  the end haver only an input connector that allows to extract data from the flow to third parties, files or debug console in the web editor.~~

### Palette

The web editor contains a graphical list with all the available nodes that can be found in a Node-RED server, called "palette". in order to provide a simple access when creating a flow.  You can extend your palette just using the "palette manager" situated at Node-RED's main menu, in which you can find more than 2.000 additional nodes for your server that have been created by both Node-RED developers and by community contributors. 

### Flows

A flow is represented as a tab within the editor workspace and is the main way to organise nodes. Each flow can have a name, and description that is displayed in the [Information sidebar](https://nodered.org/docs/user-guide/editor/sidebar/info). All of the nodes in a flow can access the same [flow-scoped context](https://nodered.org/docs/user-guide/context).

The flow editor makes it easy to wire together flows using the wide range of nodes in the palette. Flows can be then deployed to the runtime just clicking into "debloy" button: 

![](../.gitbook/assets/nodered_deploy-button.png)

### Debug Console

Is a section of the right slide bar that provides a structured view of the messages that are being sent by the debud nodes,  making it easier to explore the performance of the execution.

Alongside each message, the debug sidebar includes information about the time the message was received and which Debug node sent it. Clicking on the source node id will reveal that node within the workspace.

{% hint style="info" %}
### NodeRED documentation

You can find additional documentation about the using of this tool in Node-RED's official website: [https://nodered.org/docs/user-guide/editor/workspace/](https://nodered.org/docs/user-guide/editor/workspace/)
{% endhint %}

## Plugin Configuration

In this section it is described the different steps you need to follow to configure the Node-RED plugin to work with Thinger.io server. 

When a Node-RED plugin is installed in Thinger.io Platform, the installation manager sets Thinger.io server as proxi-server for Node-RED communications, but during the first launch of the plugin, it is necessary to make a configuration of the particular server instance that is going to be used by the plugin Nodes. 

To make this configuration, just open any Thinger.io Node properties form and go to the last input, called "Server " and click into the edition button, which will open the Thinger-Server configuration menu.

![](../.gitbook/assets/nodeconfiguration.png)

Then you can fill the server credentials of the Thinger.io server instance that is going to receive your Node-RED requests. If you just want to use the same server that is hosting the plugin,  it is only necessary to include `$(THINGER_HOST)` into "Host" box and `$(THINGER_TOKEN_NODE_RED_PLUGIN)` in the "Token" box, then you can disable SSL communication, as all messages are going to run into the same computer.

![](../.gitbook/assets/server-configuration.png)

{% hint style="warning" %}
Note that, if you want to use a different Thinger.io Server or your aren't running Node-RED from a plugin, it will be necessary to **include its URL or IP Address** in the "Host" box and also a Thinger.io **Access Token with Admin-Access** privileges in the "Token" box, finally SSL would be preferable in this case. 
{% endhint %}

## Thinger.io Nodes

As it was explained on the beginning, this plugin has two purposes: Host a Node-RED server instance together with Thinger.io IoT server and inprove their integration by including a set of custom nodes to simplify extracting data from Thinger.io IoT devices or calling other platform features. The next list contains a complete description of each node and its properties configuration:

### Device Read                 ![](../.gitbook/assets/device-read-node.png)

It is a **Function** Node that retrieves data from a specified Thinger.io device resource when an injection Node requires it. It is useful to get data from devices with a Node-RED programmed sampling interval.  

To configure this node just include the `Device ID`and the `Resource Name` into the node parameters form. The input for this Node is a triggered event and the output will be a JSON format message with the device resource variables.  

### Device Stream             ![](../.gitbook/assets/device-stream-node.png)   

It is an **Injection** Node that retrieves data from an specified Thinger.io Device Resource at regular interval specified in seconds into the properties form. 

To configure this node just include the `Device ID`and the `Resource Name` into the node parameters form. The output of this node will be a JSON with the real time Device Resource data that was specified into the properties form.

### Endpoint Call              ![](../.gitbook/assets/nodeendpoint.png) 

It is an **Output** Node that allows calling the execution of a Thinger.io endpoint profile. It is able to receive a JSON that can be introduced in the endpoint in order to use that data in an e-mail, or send to third parties, so it is quite useful to create notifications or to integrating an IoT project with other systems.  

The configuration of this Node just requires introducing the `Endpoint ID`, however, it is important to make a proper management of the input json in order to send the right data. It is strongly recommended to filter the JSON keys using a "Change" Node and check the result with a "Debug" Node to be sure about the data that we are sending.  

### Bucket Write               ![](../.gitbook/assets/nodebucket.png)

It is an **Output** Node that allows to store data into a Thinger.io Data Bucket. It is able to receive a JSON from Node-RED flow and automatically create an entry into the Bucket, so results quite useful to implement scalable data storage to any project.

The configuration of this Node just requires introducing the `Bucket ID`, however, it is important to make a proper management of the input json in order to store the right data. It is strongly recommended to filter the JSON keys using a "Change" Node and check the result with a "Debug" Node to be sure about the data that we are sending.  

### Device Write                ![](../.gitbook/assets/imagen1ddd.png) 

It is an **Output** Node that allows sending data to a Thinger.io connected Device. It is able to receive a JSON from Node-RED flow and automatically send it to other device in real time. So results quite useful to implement scalable data storage to any project.

The configuration of this Node just requires introducing the `Device ID` and the `Resource ID` , however, it is important to make a proper management of the input json in order to store the right data. It is strongly recommended to filter the JSON keys using a "Change" Node and check the result with a "Debug" Node to be sure about the data that we are sending.  

### Server Events              ![](../.gitbook/assets/imagen1sswa.png) 

It is an **Inject** Node that allows to trigger in real time any event that take places in Thinger.io IoT Server, including:

1. Bucket Write
2. Device Status Change
3. Device Callback Call
4. Device Property Update
5. Device Location Update
6. Endpoint Call

The configuration of each type requires to include the ID of the element that wants to be supervised. It is also possible to monitor every elements leaving the identifier field empty.

When any of these events is triggered, this Node is able to inject a JSON in the flow with the identifier of the Bucket, Device, or Endpoint that has produced the event along with the relevant data. 

## Useful Example Flows

In this section you can find our own cookbook with some usefull flows that you can use to get the maximum benefit from this integration

{% tabs %}
{% tab title="Extract data from Bucket" %}

{% endtab %}

{% tab title="Device Disconnection Alert" %}

{% endtab %}

{% tab title="MQTT to Thinger.io" %}

{% endtab %}

{% tab title="GPS Geofences" %}

{% endtab %}
{% endtabs %}

## Plugin Development Details

