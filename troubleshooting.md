---
description: This section explains how to identify and solve connection issues
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# TROUBLESHOOTING

## Connection Troubleshooting

Several scenarios may lead to the malfunctioning of the software client, thereby impeding or destabilizing its connection with the IoT platform. However, Thinger.io's software client is equipped with various tools to identify and prevent such issues.

### Enabling DEBUG

If a newly programmed device is showing problems to be "online" on Thinger.io Server or even is being locked, the debug function will help to identify the issue. Include the following definition in your sketch, but make sure it comes first, **before any other includes**. When using Arduino framework it is necessary to enable `Serial` communication, as all the debugging information is displayed over Serial.

```cpp
#define THINGER_SERIAL_DEBUG

#include "Thinger.h" //use the proper thinger.io library for each processor

void setup() {
  Serial.begin(115200);
}
```

When you incorporate this command, the program will display all communication traces on the device Serial port. The following section demonstrates a successful connection for an ESP8266 device.

```
[NETWORK] Connecting to network my_wifi_SSID
[NETWORK] Connected to WiFi!
[NETWORK] Getting IP Address...
[NETWORK] Got IP Address: 192.168.1.61
[NETWORK] Connected!
[_SOCKET] Connecting to iot.thinger.io:25202...
[_SOCKET] Using secure TLS/SSL connection: yes
[SSL/TLS] Warning: use #define _VALIDATE_SSL_CERTIFICATE_ if certificate validation is required
[_SOCKET] Connected!
[THINGER] Authenticating. User: my_user_name Device: my_device_ID
[THINGER] Writing bytes: 29 [OK]
[THINGER] Authenticated
```

With the debug traces is it possible to determine what is happening inside the sketch.

{% hint style="info" %}
Enable DEBUG traces for a better troubleshooting. Don't forget to open the Serial port.
{% endhint %}

### Possible Errors

#### Cant connect to network

* On WiFi, check the network credentials as WiFi SSID, PASSWORD.
* On Ethernet, ensure the cable is properly plugged to a network.
* On GSM, ensure you are setting the correct APN and the module is correctly powered. Check network signal and antennas.

#### Cant connect to Thinger.io Server

* The device is trying to connect to standard Thinger.io server. On private server instances, set your server address by using the definition `#define THINGER_SERVER "acme.thinger.io"` on top of your code. [More details](server/deployment/thinger.io-cloud-server.md#device-connection).

#### Error while connecting Thinger.io Server

* Disable the secure TLS/SSL connection by placing`#define _DISABLE_TLS_` on the top of your code. If it works, please review how you can include proper TLS/SSL certificates on your device for a secure connection.
* Using MQTT try to test non SSL connection through 1883 port.

#### Authentication Error

* If you are using a private server instance, ensure your device is pointing to your server by using the definition `#define THINGER_SERVER "acme.thinger.io"` on top of your code. [More details.](server/deployment/thinger.io-cloud-server.md#device-connection)
* Check the username, Device ID and Device Credentials are the same as the platform configuration.

#### No Debug Traces

* Delete any delay() instructions.
* Verify sensors connection and other libraries behavior. Ensure the setup function is able to end.
* Identify and delete `while(1)` loops.

## Support / Find help

If you're unable to find a solution even after following these instructions, we've established additional resources to assist you:

<figure><img src=".gitbook/assets/image (78).png" alt=""><figcaption><p>Ask our community</p></figcaption></figure>

### **Community Discussion Forum**

[Community Discussion Forum](https://community.thinger.io) is a forum, designed to provide developers a platform to exchange ideas, share their projects, and seek advice among fellow Thinger.io users. It is currently the best avenue for quick and free assistance with development issues, as many queries have likely been previously asked and resolved by other developers. So, your first step should be to use the search bar to locate a similar post:

<figure><img src=".gitbook/assets/image (79).png" alt=""><figcaption><p>Thinger.io community forum</p></figcaption></figure>

Optimizing the use of this resource involves browsing for existing topics related to your query before starting a new one. However, if creating a new topic is necessary, please consider the following:

* It is preferable to write in **English**, as it broadens the pool of people who can understand and potentially provide diverse opinions or solutions to your problem.&#x20;
* Choose an engaging and descriptive title and select the most suitable category.&#x20;
* Compose a comprehensive description of your issue, which includes context, use case, or project objectives. This will help others grasp your situation.&#x20;
* Don't forget to supply all the necessary information to recreate the problem, such as: Source code, Libraries and PCB specifications, Compiler output and Debug connection trace, as well as Thinger.io configuration.&#x20;
* Always be polite, remember that other developers are under no obligation to assist you, and if they do, it is out of goodwill.
* If you wish to share some source code, kindly utilize the Preformatted text option for optimal display.

<figure><img src=".gitbook/assets/image (80).png" alt=""><figcaption><p>Use preformatted text when sharing source code</p></figcaption></figure>

### **Extended Support**

For professional developers who require swift assistance with development or maintenance procedures, Thinger.io private instance licenses can be supplemented with an extended support service. Additionally, an SLA service is available for large infrastructure customers, which can be contracted separately. If interested, please contact us via [this webpage](https://thinger.io/contact-us/).
