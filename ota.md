---
description: Over The Air Device Updates
---

# REMOTE OTA

Over-the-Air (OTA) Programming is a method that allows the remote sending of new firmware to IoT devices over the Internet, regardless of their location. This process works with the Thinger.io Cloud to deliver the new firmware binaries.

This feature enables us to keep devices updated with new security patches and code improvements quickly and on a large scale. It is an essential tool for the maintenance of IoT products, preventing the need to travel to the devices' locations.

The update process is performed using a **Visual Studio Code** extension that integrates with the Thinger.io cloud to upload new firmware binaries to the target device. This integration with the IDE streamlines the development process, allowing developers to compile and upload firmware as if the device were connected to the computer.

{% embed url="https://youtu.be/AHqfI7yP1-o" fullWidth="false" %}

## IDE Configuration

Before working with this tool it is necessary to install and configure Visual Studio Code and the PlatformIO extension as explained in the [SDK SETUP](sdk-setup/#visual-studio-code) section of the documentation. Then, you need to **install the Thinger.io VSCode extension** directly from the Extension manager. Search for "Thinger.io" or [Checkout on Microsoft Marketplace](https://marketplace.visualstudio.com/items?itemName=thinger-io.thinger-io).&#x20;

<figure><img src=".gitbook/assets/image (624).png" alt=""><figcaption><p>Visual Studio Code Thinger.io Extension for MCU OTA </p></figcaption></figure>

This extension will manage the OTA processes with several interesting features such as:

* OTA updates directly from the Internet over Thinger.io
* Device and Product switcher to search and select the target device(s) for the update
* Real-time device connection status and input/output indicators
* Compatibility with multiple PlatformIO configuration environments within a project
* Automatic build and upload over the Internet with a single click
* OTA with compression support for ESP8266, ESP32, and Arduino Portenta devices
* MD5 checksum verification for firmware binaries, both compressed and uncompressed

### Extension Configuration

Before running the OTA, it is necessary to configure the extension by accessing the VS Code Extensions Manager and selecting the Extensions Settings option.

<figure><img src=".gitbook/assets/image (625).png" alt=""><figcaption><p>Thinger.io Visual Studio Code Extension for OTA updates</p></figcaption></figure>

* **Thinger.io Host:** Place here the URL of the Thinger.io Instance you are working with (if using a private instance, otherwise by default it will be `backend.thinger.io`).
* **Thinger.io Port:** Specifies the connection port (443 by default).
* **Thinger.io Secure:** Verify SSL/TLS connection (enabled by default).
* **Thinger.io SSL:** Use SSL/TLS encryption (enabled by default).
* **Thinger.io Token:** Place here a Thinger.io Access Token with the following permissions:
  * ListDevices
  * AccessDeviceResources
  * ReadDeviceStatistics
  * ListProducts

The following image provides a token configuration example with the required permissions.

<figure><img src=".gitbook/assets/image (627).png" alt=""><figcaption><p>Example Thinger.io Token Configuration for Visual Studio Code Extension</p></figcaption></figure>

## Firmware Upload via OTA

The OTA feature is implemented on the default [Thinger.io Arduino IOTMP client libraries](https://github.com/thinger-io/Arduino-Library) required for the connectivity with the Thinger.io cloud.

The boards that supports OTA updates over the Visual Studio Code extension are the following:

* Espressif ESP8266
* Espressif ESP32
* Arduino Nano 33 IOT
* Arduino MKR WiFi 1010
* Arduino RPI2040 Connect
* Arduino MKR NB 1500
* Arduino Portenta
* Arduino Opta

The general requirement to start working with OTA updates for a specific device are:

1. Have a PlatformIO project on Visual Studio Code for the target device. More details [here](https://docs.thinger.io/sdk-setup/visual-studio-code#starting-a-project).
2. Have a basic Thinger.io firmware for your device, and ensure you it can connect to the cloud.
3. Modify the sketch to include the OTA functionality. More details in each specific device section.
4. Flash the initial firmware over a serial communication port on the computer.
5. Use the Thinger.io Visual Studio Code toolbar buttons to select your device and flash new firmware.

If everything is configured correctly it will be possible to start working with the Thinger.io extension via the new elements added to the bottom toolbar. There are two buttons that allow to select the target device to be flashed, and then compile and upload new firmware binaries.

![Thinger.io Buttons on Visual Studio Code Toolbar](<.gitbook/assets/image (318).png>)

**Select Target Device** 🚀

This button is a device selector, when you click it, it will prompt to search and select a target device from your Thinger.io account.

![Device Selector for OTA Updates over Visual Studio Code](.gitbook/assets/ota\_device\_selector.gif)

{% hint style="info" %}
When the target device is disconnected, the target device button background color will be red.
{% endhint %}

**Compile and Update ▶️**

This button compiles and uploads the code to the selected device. In the process it will show a window with the OTA progress.&#x20;

![Compile and upload example for ESP8266](<.gitbook/assets/iot-ota (1).gif>)

{% hint style="info" %}
To update your device over OTA, the first time it must be flashed from a serial com port.
{% endhint %}

**Clean Target Device** 🗑️

It is possible to clean the selected target device accessing the Visual Studio command Palette with `Ctrl` + `Shift` + `P`, and searching for `Thinger.io`

![Clean Target Device](<.gitbook/assets/image (307).png>)

### ESP32 OTA

To add OTA functionality for **ESP32** it is only required to include the `ThingerESP32OTA.h` header and create an instance of it. A complete example for a basic firmware with OTA support can be as the following:

{% tabs %}
{% tab title="main.cpp" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerESP32.h>
#include <ThingerESP32OTA.h>
#include "arduino_secrets.h"

ThingerESP32 thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);
ThingerESP32OTA ota(thing);

void setup() {
  // open serial for monitoring
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

{% tab title="ardunio_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"
```
{% endtab %}
{% endtabs %}

### ESP8266 OTA

To add OTA functionality for **ESP8266** it is only required to include the `ThingerESP8266OTA.h` header and create an instance of it. A complete example for a basic firmware with OTA support can be as the following:

{% tabs %}
{% tab title="main.cpp" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerESP8266.h>
#include <ThingerESP8266OTA.h>
#include "arduino_secrets.h"

ThingerESP8266 thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);
ThingerESP8266OTA ota(thing);

void setup() {
  // open serial for monitoring
  Serial.begin(115200);

  // set builtin led as output
  pinMode(LED_BUILTIN, OUTPUT);

  // add WiFi credentials
  thing.add_wifi(SSID, SSID_PASSWORD);

  // digital pin control example (i.e. turning on/off a light, a relay, configuring a parameter, etc)
  thing["led"] << digitalPin(LED_BUILTIN);

  // resource output example (i.e. reading a sensor value)
  thing["millis"] >> outputValue(millis());

  // more details at http://docs.thinger.io/arduino/
}

void loop() {
  thing.handle();
}
```
{% endtab %}

{% tab title="arduino_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"
```
{% endtab %}
{% endtabs %}

### Arduino Portenta/Opta

To add OTA functionality for **Arduino Portenta and Opta devices** it is required to include the `ThingerPortentaOTA.h` header and create an instance of it. A complete example for a basic firmware with OTA support can be as the following:

{% tabs %}
{% tab title="main.cpp" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerMbed.h>
#include <ThingerPortentaOTA.h>
#include "arduino_secrets.h"

ThingerMbed thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);
ThingerPortentaOTA ota(thing);

void setup() {
    // configure LED_BUILTIN for output
    pinMode(LED_BUILTIN, OUTPUT);

    // open serial for debugging
    Serial.begin(115200);

    // configure wifi network
    thing.add_wifi(SSID, SSID_PASSWORD);

    // pin control example (i.e. turning on/off a light, a relay, etc)
    thing["led"] << digitalPin(LED_BUILTIN);

    // resource output example (i.e. reading a sensor value, a variable, etc)
    thing["millis"] >> outputValue(millis());

    // start thinger on its own task
    thing.start();

    // more details at http://docs.thinger.io/arduino/
}

void loop() {
    // use loop as in normal Arduino Sketch
    // use thing.lock() thing.unlock() when using/modifying variables exposed on thinger resources
    delay(1000);
}
```
{% endtab %}

{% tab title="arduino_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
It may be required to update the bootloader of your Portenta/Opta device to enable OTA updates.&#x20;

* Update the Bootloader of your Arduino Portenta/Opta: Run the sketch 'STM32H747\_System\_manage\_Bootloader'
* Format the QSPI flash memory: Run the sketch 'QSPIFormat'.
* Run WiFiFirmwareUpdater&#x20;

![](<.gitbook/assets/image (629).png>)
{% endhint %}

### Arduino Nano 33 IOT OTA

To add OTA functionality for **Arduino Nano 33 IOT** it is required to include the `ThingerWiFiNINAOTA.h` header and create an instance of it. A complete example for a basic firmware with OTA support can be as the following:

{% tabs %}
{% tab title="main.cpp" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerWiFiNINA.h>
#include <ThingerWiFiNINAOTA.h>
#include "arduino_secrets.h"

ThingerWiFiNINA thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);
ThingerWiFiNINAOTA ota(thing);

void setup() {
  // configure LED_BUILTIN for output
  pinMode(LED_BUILTIN, OUTPUT);

  // open serial for debugging
  Serial.begin(115200);

  // configure wifi network
  thing.add_wifi(SSID, SSID_PASSWORD);

  // pin control example (i.e. turning on/off a light, a relay, etc)
  thing["led"] << digitalPin(LED_BUILTIN);

  // resource output example (i.e. reading a sensor value, a variable, etc)
  thing["millis"] >> outputValue(millis());
  
  // more details at http://docs.thinger.io/arduino/
}

void loop() {
  thing.handle();
}
```
{% endtab %}

{% tab title="arduino_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Note: it is required to include `lib_archive = no` to `platformio.ini` configuration file.
{% endhint %}

{% code title="platformio.ini" %}
```bash
[env:nano_33_iot]
platform = atmelsam
board = nano_33_iot
framework = arduino
lib_archive = no
lib_deps = thinger.io
```
{% endcode %}

### Arduino MKR WiFi 1010 OTA

To add OTA functionality for Arduino **MKR WiFi 1010** it is required to include the `ThingerWiFiNINAOTA.h` header and create an instance of it. A complete example for a basic firmware with OTA support can be as the following:

{% tabs %}
{% tab title="main.cpp" %}
```cpp

#include <ThingerWiFiNINA.h>
#include <ThingerWiFiNINAOTA.h>
#include "arduino_secrets.h"

ThingerWiFiNINA thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);
ThingerWiFiNINAOTA ota(thing);

void setup() {
  // configure LED_BUILTIN for output
  pinMode(LED_BUILTIN, OUTPUT);

  // open serial for debugging
  Serial.begin(115200);

  // configure wifi network
  thing.add_wifi(SSID, SSID_PASSWORD);

  // pin control example (i.e. turning on/off a light, a relay, etc)
  thing["led"] << digitalPin(LED_BUILTIN);

  // resource output example (i.e. reading a sensor value, a variable, etc)
  thing["millis"] >> outputValue(millis());
  
  // more details at http://docs.thinger.io/arduino/
}

void loop() {
  thing.handle();
}
```
{% endtab %}

{% tab title="arduino_secrets.h" %}
```cpp
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Note: it is required to include `lib_archive = no` to `platformio.ini` configuration file.
{% endhint %}

{% code title="platformio.ini" %}
```bash
[env:mkrwifi1010]
platform = atmelsam
board = mkrwifi1010
framework = arduino
lib_archive = no
lib_deps = thinger.io
```
{% endcode %}

### Arduino RPI2040 Connect OTA

To add OTA functionality for **Arduino RPI2040 Connect** it is only required to include the `ThingerMbedOTA.h` header and create an instance of it. A complete example for a basic firmware with OTA support can be as the following:

{% tabs %}
{% tab title="main.cpp" %}
```cpp

#define THINGER_SERIAL_DEBUG

#include <ThingerMbed.h>
#include <ThingerMbedOTA.h>
#include "arduino_secrets.h"

ThingerMbed thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);
ThingerMbedOTA ota(thing);

void setup() {
  // open serial for debugging
  Serial.begin(115200);

  // configure LED_BUILTIN for output
  pinMode(LED_BUILTIN, OUTPUT);

  // configure wifi network
  thing.add_wifi(SSID, SSID_PASSWORD);

  // pin control example (i.e. turning on/off a light, a relay, etc)
  thing["led"] << digitalPin(LED_BUILTIN);

  // resource output example (i.e. reading a sensor value, a variable, etc)
  thing["millis"] >> outputValue(millis());

  // start thinger task
  thing.start();

  // more details at http://docs.thinger.io/arduino/
}

void loop() {
  // your code here
}
```
{% endtab %}

{% tab title="arduino_secrets.h" %}
```cpp
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"
```
{% endtab %}
{% endtabs %}

### Arduino MKR NB 1500 OTA

To add OTA functionality for **MKR NB 1500** it is only required to include the `ThingerMKRNBOTA.h` header and create an instance of it. A complete example for a basic firmware with OTA support can be as the following:

{% tabs %}
{% tab title="main.cpp" %}
```cpp
#define THINGER_SERIAL_DEBUG

#include <ThingerMKRNB.h>
#include <ThingerMKRNBOTA.h>
#include "arduino_secrets.h"

ThingerMKRNB thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);
// OTA seems to not work if no reset button pressed
ThingerMKRNBOTA ota(thing);

void setup() {
  // enable serial for debugging
  Serial.begin(115200);

  // optional set pin number
  thing.set_pin(PIN_NUMBER);

  // set APN
  thing.set_apn(GPRS_APN, GPRS_LOGIN, GPRS_PASSWORD);

  // set builtin led to output
  pinMode(LED_BUILTIN, OUTPUT);

  // pin control example over internet (i.e. turning on/off a light, a relay, etc)
  thing["led"] << digitalPin(LED_BUILTIN);

  // resource output example (i.e. reading a sensor value, a variable, etc)
  thing["time"] >> [&](pson& out){
      out = thing.getNB().getTime();
  };

  // more details at http://docs.thinger.io/arduino/
}

void loop() {
  thing.handle();
}

```
{% endtab %}

{% tab title="arduino_secrets.h" %}
```cpp
#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define PIN_NUMBER ""

#define GPRS_APN "your_apn_name"
#define GPRS_LOGIN "your_gprs_login"
#define GPRS_PASSWORD "your_gprs_password"
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Note: it is required to include `lib_archive = no` to `platformio.ini` configuration file.
{% endhint %}

{% code title="platformio.ini" %}
```bash
[env:mkrnb1500]
platform = atmelsam
board = mkrnb1500
framework = arduino
lib_archive = no
lib_deps = thinger.io
```
{% endcode %}

{% hint style="warning" %}
At this moment, it seems that MKR NB 1500 requires pressing the reset button to apply the OTA update.
{% endhint %}

## Firmware Versioning

Firmware versioning is essential for managing updates and maintaining the security of IoT devices. As IoT deployments grow in complexity and scale, proper versioning ensures that devices operate reliably and securely.

Our recommended versioning system follows the Semantic Versioning ([SemVer](https://semver.org/)) approach, which uses a Major.Minor.Patch format:

* **Major**: Incremented for incompatible API changes.
* **Minor**: Incremented for added functionality in a backwards-compatible manner.
* **Patch**: Incremented for backwards-compatible bug fixes.

To define your firmware version, it is required to define a preprocessor definition called `THINGER_OTA_VERSION`. This is used inside the firmware and VS Code to decide when a device requires an update.

Each time an OTA update process is started, the system will display a confirmation dialog showing the details of the firmware to be uploaded. This dialog includes the device name, firmware version, and the environment. It ensures that the user is informed about the firmware update specifics before proceeding, providing an additional layer of verification.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>OTA Update Confirmation Dialog with Firmware Version</p></figcaption></figure>

Devices that are already running the current firmware version will not be updated, ensuring that only devices with outdated firmware receive the new update. This helps to optimize the update process, reducing unnecessary data transfer and minimizing downtime for devices that are already up-to-date.

{% hint style="info" %}
For development purposes, it is possible to remove the **`THINGER_OTA_VERSION`** definition, which will prevent version checking on the update process.
{% endhint %}

It is possible to define the firmware version in different places, such as:

* In the source code
* Through external build flags
* Based on version control, like git tags

Below, we describe the different alternatives in detail.

### Fixed on Code

Define `THINGER_OTA_VERSION` in code before Thinger.io includes. This method involves hardcoding the firmware version directly in your source files. Each time you need to update the firmware version, you must manually change the version number in the code and redeploy the firmware.

{% code title="main.cpp" %}
```cpp
#define THINGER_OTA_VERSION "1.0.0"

// Thinger.io includes
#include <ThingerMbed.h>
#include <ThingerPortentaOTA.h>
```
{% endcode %}

### Fixed Build Flag

Create a build flag in `platformio.ini` with a static version definition. This approach sets the firmware version through build configuration. To update the firmware version, you need to modify the version number in the `platformio.ini` file and rebuild the firmware.

{% code title="platformio.ini" %}
```ini
build_flags = 
    -D THINGER_OTA_VERSION=1.0.0
```
{% endcode %}

### Dynamic Build Flag

This is the recommended approach and consists of dynamically setting the firmware version based on version control information, such as git tags. This method ensures the firmware version is automatically updated based on the latest git tag, reducing manual intervention. Each time you tag a new version in git, the build process will automatically use this tag as the firmware version.

To achieve this configuration, it is required to modify the `platformio.ini` file and include a build flag that will be dynamically composed using the `git describe` command. This command retrieves the latest git tag and uses it to set the `THINGER_OTA_VERSION` preprocessor definition.

{% code title="platformio.ini" %}
```ini
build_flags = 
    !echo '-D THINGER_OTA_VERSION='$(git describe --tags)''
```
{% endcode %}
