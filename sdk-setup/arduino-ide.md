# Arduino IDE

Arduino is currently the best framework for learning, prototyping and even developing products, thanks to its simplicity and the large community of developers that is contributing daily to increase its capabilities. That is why at Thinger.io we have developed a Software Client to easily connect Arduino based devices, compatible with a wide variety of hardware. It is available for Windows, Macintosh and Linux distributions, and can be downloaded for free from their official website.

The following sections explains how to install and prepare Arduino IDE to work with Thinger.io client libraries.

![](../.gitbook/assets/image%20%28252%29.png)

## Install Arduino IDE

It is required a modern version of Arduino supporting `Library Manager` and some other features. Please install a version starting from **1.6.3** from the official Arduino download page. This step is not required if you already have a modern version.

[Download Arduino IDE](https://www.arduino.cc/en/Main/Software) from their official website.

## Install Thinger.io from Library Manager

Thinger.io Client libraries contains the software for connecting Arduino compatible devices with Thinger.io platform. It is the preferred way for connecting devices to the platform, as it allow to extract all the Thinger.io features.

The library can be obtained from the Arduino  `Library Manager`, which simplifies searching and installing new libraries and also supports updating libraries when new versions are released, so actually it is preferable using this method:

> Open the **Library Manager** in the Arduino menu in `Sketch` &gt; `Include Library` &gt; `Manage Libraries`

![Arduino Library Manager](../.gitbook/assets/image%20%28419%29.png)

> Search the library with name **thinger.io** and then click `Install`. You can update the library also from this manager when it is updated.

![Install Thinger.io Client library from Library Manager](https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/0e8bc7c86b5aff26aea7649741b592c8157cae11.png)

## Install Thinger.io from ZIP

If the preferred way using the Library Manager is not working or you prefer to manage the libraries yourself, you can obtain the `.zip` library file from the official Thinger.io project Github repository by clicking in the link below. This will download a file called `Arduino-Library-master.zip`.

[Download Library](https://github.com/thinger-io/Arduino-Library/archive/master.zip)

Now **rename** the `Arduino-Library-master.zip` file to something more relevant like `thinger.zip`.

The final step is to **import** the `zip` library using the Arduino IDE. This step will uncompress and copy the `zip` library in the Arduino libraries folder. Which is usually under your Documents folder.

![](../.gitbook/assets/add-zip-library.png)

> `Sketch` &gt; `Include Library` &gt; `Add .ZIP libraries`, and select the `thinger.zip` file created in the previous step.

## Starting a Project

Once Thinger.io Library has been installed, it is possible to start a new project by using some of the default examples provided. There are examples for different boards, so, choose the one that matches your device.

> Open `File` &gt; `Examples` &gt; `thinger.io` and start with an example for your device.

![Open Thinger.io example for an Arduino Compatible Device](../.gitbook/assets/image%20%28420%29.png)

 A basic example for an ESP32 device will look like the following:

{% tabs %}
{% tab title="ESP32.ino" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerESP32.h>
#include "arduino_secrets.h"

ThingerESP32 thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

void setup() {
  // open serial for debugging
  Serial.begin(115200);

  pinMode(16, OUTPUT);

  thing.add_wifi(SSID, SSID_PASSWORD);

  // digital pin control example (i.e. turning on/off a light, a relay, configuring a parameter, etc)
  thing["GPIO_16"] << digitalPin(16);

  // resource output example (i.e. reading a sensor value)
  thing["millis"] >> outputValue(millis());

  // more details at http://docs.thinger.io/arduino/
}

void loop() {
  thing.handle();
}
```
{% endtab %}

{% tab title="arduino\_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"
```
{% endtab %}
{% endtabs %}

