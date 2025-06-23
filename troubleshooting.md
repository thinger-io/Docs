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

If a newly programmed device is showing problems to be "online" on Thinger.io Server or is even being locked, the debug function will help to identify the issue. Include the following definition in the sketch, but make sure it comes first, **before any other includes**. When using the Arduino framework, it is necessary to enable `Serial` communication, as all the debugging information is displayed over Serial.

```cpp
#define THINGER_SERIAL_DEBUG

#include "Thinger.h" //use the proper thinger.io library for each processor

void setup() {
  Serial.begin(115200);
}
```

When this command is incorporated, the program will display all communication traces on the device's Serial port. The following section demonstrates a successful connection for an ESP8266 device.

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

With the debug traces, is it possible to determine what is happening inside the sketch.

{% hint style="info" %}
Enable DEBUG traces for better troubleshooting. Don't forget to open the Serial port.
{% endhint %}

### Possible Errors

#### Can't connect to the network

* On WiFi, check the network credentials as WiFi SSID, PASSWORD.
* On Ethernet, ensure the cable is properly plugged into a network.
* For GSM, ensure the correct APN is set and the module is correctly powered. Check the network signal and antennas.

#### Can't connect to Thinger.io Server

* The device is trying to connect to the standard Thinger.io server. On private server instances, set the server address by using the definition `#define THINGER_SERVER "acme.thinger.io"` at the top of the code. [More details](server/deployment/thinger.io-cloud-server.md#device-connection).

#### Error while connecting Thinger.io Server

* Disable the secure TLS/SSL connection by placing`#define _DISABLE_TLS_` at the top of the code. If it works, please review how to include proper TLS/SSL certificates on the device for a secure connection.
* Using MQTT, try to test a non-SSL connection through the 1883 port.

#### Authentication Error

* For private server instances, it is essential to ensure the device is pointing to the correct server by including the definition `#define THINGER_SERVER "acme.thinger.io"` at the top of the code. [More details.](server/deployment/thinger.io-cloud-server.md#device-connection)
* Check that the username, Device ID and Device Credentials are the same as the platform configuration.

#### No Debug Traces

* Delete any delay() instructions.
* Verify sensors' connection and other libraries' behavior. Ensure the setup function is able to end.
* Identify and delete `while(1)` loops.

## Support / Find help

If a solution remains elusive even after following these instructions, additional resources have been established for assistance. Searching for "Thinger.io" in a browser:

<figure><img src=".gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

### **Community Discussion Forum**

[Community Discussion Forum](https://community.thinger.io) is a forum designed to provide developers a platform to exchange ideas, share their projects, and seek advice among fellow Thinger.io users. It is currently the best avenue for quick and free assistance with development issues, as many queries have likely been previously asked and resolved by other developers. So, the first step should be to use the search bar to locate a similar post:

<figure><img src=".gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

Optimizing the use of this resource involves browsing for existing topics related to a query before starting a new one. However, if creating a new topic is necessary, please consider:

* It is preferable to write in **English**, as it broadens the pool of people who can understand and potentially provide diverse opinions or solutions to the problem.&#x20;
* Choose an engaging and descriptive title and select the most suitable category.&#x20;
* Compose a comprehensive description of the issue, which includes context, use case, or project objectives. This will help others grasp this situation.&#x20;
* Don't forget to supply all the necessary information to recreate the problem, such as: Source code, Libraries and PCB specifications, Compiler output and Debug connection trace, as well as Thinger.io configuration.&#x20;
* Remember to always be polite; other developers are under no obligation to assist, and any help received is a gesture of goodwill.
* If sharing source code, please use the **Preformatted text** option for optimal display.&#x20;

<figure><img src=".gitbook/assets/image (80) (1).png" alt=""><figcaption><p>Use preformatted text when sharing source code</p></figcaption></figure>

### **Extended Support**

For professional developers who require swift assistance with development or maintenance procedures, Thinger.io private instance licenses can be supplemented with an extended support service. Additionally, an SLA service is available for large infrastructure customers, which can be contracted separately. If interested, please contact us via [this webpage](https://thinger.io/contact-us/).
