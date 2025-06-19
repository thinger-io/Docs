# Cluster / Server Status

The 2.7.5 release of Thinger.io private instances introduced a dashboard to overview some host status variables. This is a very useful tool when working with multiple user accounts or extensive deployments because it allows the administrator to check the computational load of the server in order to evaluate if a more powerful subscription is required or if it is oversized.

{% hint style="info" %}
This feature is only available for the instance's manager account
{% endhint %}

The server monitoring terminal is accessible only to cluster/instance administrators by accessing the "**Cluster Hosts**" section of the Thinger.io main menu, which will display the Host List:

<figure><img src="../../.gitbook/assets/image (761).png" alt="" width="563"><figcaption></figcaption></figure>

The server list shows all the associated instances in the same cluster. If the IoT network consists of only one server, only one profile will be shown.

![](<../../.gitbook/assets/image (440).png>)

The values shown bring together the connection data of all devices and user accounts on the same server. The next parameters can be shown:&#x20;

* **HTTP Client Connections:** Connections made by devices that call the server's REST API.
* **HTTP Server Connections:** Made by running plugins.
* **Device Connections:** Is the number of Thinger.io software client devices connected.
* **HTTP Websockets:** Can be both thinger.io software client devices or web consoles opened.
* **HTTP SSE:** This is the number of server events that have been sent.
* **CPU Usage:** Is the load of the microprocessor, it increases with intensive data processing uses
* **RAM Usage:** Amount of RAM memory consumption, is actually the most critical variable, as it increases a lot with the number of deployed plugins.&#x20;
* **Disk Usage:** Amount of SSD consumed&#x20;

Thinger.io IoT server has been developed with a great effort to improve computational efficiency, allowing the creation of large device networks and ingesting thousands of data points per second with minimum overhead in the host. However, there are some factors that can increase the load on the server and cause malfunctions or data losses, such as:

1. The number of executing plugins, as each plugin deployed by each user account creates a Docker container, increases the RAM and CPU memory consumption.&#x20;
2. Intensive data computational processes over raw device data.
3. The number of user accounts and active interfaces: It will require thousands of users and requests at the same time to actually overload the system, so just a little instance can manage a huge network if no plugins are deployed.&#x20;
