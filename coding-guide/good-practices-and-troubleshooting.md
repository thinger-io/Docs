---
description: >-
  This section explains some tricks to create better code for connected devices
  using Thigner.io's libraries as well as a troubleshooting guide to identify
  and solve connection issues
---

# Good Practices & Troubleshooting

## Coding Good Practices

* First of all, read the platform documentation, it is strongly recommended to use the example codes provided there when creating new firmware.
* Thinger resources definition code must be introduced in the` setup()` function or any other function that is executed just one time in the program source code.
* `thing.handle()` instruction must be included in the `loop()` function or in the main recurrent execution function of the program source code.
* Avoid using `delay()` or any other locking instructions when coding for IoT purposes. Using it will make your device losing the connection with the network, which will result in continuous reconnection processes. If the program requires timing functionalities, it is possible to use non-locking structures for example:&#x20;

```
//to make a led blick every 5 seconds without lock the process 
if(millis()%5000==0){
    digitalWrite(LED_PIN, !digitalRead(LED_PIN);
}
```

* If a specific routine requires several time to be executed, including and additional `thing.handle()` instruction could be helpful to guarantee the connection.&#x20;
* Avoid polling data. Thinger.io implements a resources subscription paradigm that allows the server to take control of when does the devices send data, according to a user-defined sampling interval. However, there are some instructions such as `thing.call_endpoint` and `thing.write_bucket`, that has been implemented to send asynchronous communications to the server. It is important to be careful when using these instructions in a non-controlled way or create polling situations that will decrease the efficiency of the infrastructure. Next example code shows how to properly call to an asynchronous call just when an event is detected by the device code:

```
void loop(){

 thing.handle();
 
 button_pressed = digitalRead(BUTTON_PIN);

 //if the button wasn't pressed and is pressed now
 if(button_pressed == 1 && last_button_status == 0) {
      thing.call_endpoint("alert");   
 }

 last_button_status = button_pressed;

}
```

* Use the **debug** mode when testing and prototyping to identify connection problems (shee troubleshooting guidelines)
* Create consistent input resources using Pson.is\_empty() function. Every time Thinger server creates a new connection with any device, each resource code is executed with an empty Pson. To prevent this behavior making any runaway execution, it is useful to analyze the content of the Pson before allowing the resources to execute. The following example code implements the `pson.is_empty()` function and an auxiliary Pson to save the proper status of the resource (Arduino framework is being used):

```
pson aux;

void setup(){
aux=false;             //set an initial status for the aux pson


 thing["secure_input_resource"] >> [] (pson & in){
    
    if(in.is_empty()){
       in=aux;         //fill the pson with the aux value 
    }
    
    digitalWrite(LED_PIN, in? HIGH:LOW);
    in=aux;            //update the aux value
 }
} 
```

### Coding problems & Solutions list

| Problem                    | Source                                        | Solution                                                                                             |
| -------------------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| program doesn't compile    | errors on your code or incompatible libraries | try with basic source code example to check it is working before continue developing                 |
| Example code don't compile | PCB bootloader version incompatible           | Check the compiler output to find the problem or install an older version of the device bootloader.  |

## Connection Troubleshooting Guidelines

There are few situations that can produce the malfunction of the software client, hampering the connection with the IoT platform or making it unstable. But Thinger.io software client has been provided with some tools to detect and avoid these kind of problems.&#x20;

#### Enabling DEBUG mode

If a recently programmed device is showing problems to be "online" on Thinger.io Server or even is being locked, the debug function will help to identify the issue. Include the following definition in your sketch, but make sure it comes first, before any other includes (it was reported to cause crashes on some boards otherwise). When using Arduino framework it is necessary to enable `Serial` communication, as all the debugging information is displayed over Serial.

```
#define THINGER_SERIAL_DEBUG

#include "Thinger.h" //use the proper thinger.io library for each processor

void setup() {
  Serial.begin(115200);
}
```

When this command is included, the program will print all the communication traze. Next block shows a correct connection traze for an ESP8266 connection.

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

### Problems and Solutions list

| Problem                                  | Source                              | Solution                                                                                                                                                                                                |
| ---------------------------------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Can't connect to network                 | Wrong network credentials           | <ul><li>Check the network credentials as WiFi SSID, PASSWORD, or GSM configuration has been properly introduced </li><li>Check network signal</li></ul>                                                 |
| Can't connect to Thinger.io Server       | Wrong Thinger.io server instance ID | <ul><li>The device is trying to connect to standard Thinger.io server, set your server IP/web domain by using the definition #define THINGER_SERVER "&#x3C;instance_URL>"</li></ul>                     |
| Error while connecting Thinger.io server | SSL issue                           | <p></p><ul><li>Disable the secure TLS/SSL connection by pacing<code>#define _DISABLE_TLS_ </code>on the top of the sketch</li><li>Using MQTT try to test non SSL connection through 1883 port</li></ul> |
| Authentication Error                     | Wrong account or device credentials | <ul><li>Check the username, Device ID and Device Credentials are the same as the platform configuration</li></ul>                                                                                       |
| No debug trace                           | Coding problem                      | <ul><li>Delete delay() instructions</li><li>Verify sensors connection and other libraries behavior</li><li>Identify and delete<code> while(1)</code> loops</li></ul>                                    |

## Support / find help

If after following these instructions it was impossible to find a solution, we have created other resources that can be used in order to find help:&#x20;

![](<../.gitbook/assets/image (254).png>)

### **Community Discussion Forum**

It is a forum ([**accessible here**](https://community.thinger.io)) created to provide developers a place to share projects, knowledges and doubts with other Thinger.io users. Actually is the best way to obtain fast and free assistance to development problems because most of the doubts have probably been asked and solved before by other developers. So the **first step is to use the search bar **to find a similar post:

![](<../.gitbook/assets/image (323).png>)

Making proper use of this resource means using the browser in order to find old topics related to the doubt before creating a new one, but if it is required, take in consideration: \


* It is preferable to write in **English** language, as the more people can understand your problem, the more can help you providing different opinions or solutions.
* Use an attractive and descriptive title and select the most accurate category
* Write a proper description of your problem, including some **description of the context**, use case or objectives or the project in order to help other people to understand the situation.&#x20;
* Don't forget to provide all the material that will be required to replicate the problem such as: **Source code,** Libraries and PCB specification, Compiler output and** Debug connection trace**, as well as** thinger.io configuration. **
* Be polite, remember that other developers are not compelled to help you and if they do it is with good intentions

### **Extended Support**

For those professional developers that require quick assistance on development or maintenance processes, Thinger.io private instance licenses can be complemented with an extended support service.\
\
There is also an SLA service available for big infrastructure customers, that can be contracted separately just contacting us at info@thinger.io.
