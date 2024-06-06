# Visual Studio Code

For advanced developers and more complex projects, an advanced IDE can be beneficial. Visual Studio Code (VS Code) offers features like GIT control, code completion, and a variety of useful extensions to streamline the development process. By combining VS Code with PlatformIO, you get a powerful setup for your Thinger.io projects.

**Benefits of Using Visual Studio Code with PlatformIO:**

* **GIT Control**: Easily manage your version control with integrated GIT support.
* **Code Completion**: Enjoy enhanced code completion for faster and more accurate coding.
* **Extensions**: Access a wide range of extensions to add functionality and improve productivity.

To get started, download and install Visual Studio Code and the PlatformIO extension.

## Install Visual Studio Code

**Visual Studio Code** is a free distribution source-code editor developed by Microsoft for Windows, Linux and macOS. It can be extended via [extensions](https://en.wikipedia.org/wiki/Plug-in\_\(computing\)), available through a central repository to add language support, new programming languages, [themes](https://en.wikipedia.org/wiki/Theme\_\(computing\)), and [debuggers](https://en.wikipedia.org/wiki/Debugger), or perform [static code analysis](https://en.wikipedia.org/wiki/Static\_code\_analysis). It can be downloaded for free from the official website.

&#x20;[**Download Visual Studio Code**](https://code.visualstudio.com/download)

![Visual Studio Code](<../.gitbook/assets/image (386).png>)

## **Install PlatformIO**

PlatformIO is a cross-platform, cross-architecture, multi-framework professional tool for embedded systems engineers and software developers working on embedded products. It can be installed as an extension in Visual Studio Code.

**Steps to Install PlatformIO:**

1. **Open Extensions in Visual Studio Code**:
   * Press `Ctrl + Shift + X` on Windows or `Command + Shift + X` on Mac to open the Extensions view.
2. **Search for PlatformIO**:
   * In the Extensions view, type "PlatformIO" in the search bar.
3. **Install PlatformIO**:
   * Click on the **PlatformIO IDE** result.
   * Click the **Install** button.

Once installed, PlatformIO provides powerful features to enhance your development process for Thinger.io projects.

![](<../.gitbook/assets/image (329).png>)

## Starting a Project

Once Visual Studio code with PlatformIO is installed, it is possible to create a new project for our specific board. For this purpose, we can access PIO Home, and click on the `New Project` button:

![Create a new Project from PIO Home.](<../.gitbook/assets/image (263).png>)

For this example, we will be using the ESP32 board, so, in the `Project Wizard` pop-up we enter a `Project Name`, select the `Espressif ESP32 Dev Module`, as a generic ESP32 board, and the `Arduino` Framework. Once done, click on `Finish` and wait PlatformIO to download the required toolchains for the device.

![PlatformIO project Wizard](<../.gitbook/assets/image (290).png>)

After the project initialization is done, PlatformIO generates a file structure like the following:

![PlatformIO default project structure](<../.gitbook/assets/image (301).png>)

As shown in the above picture, each PlatformIO project has a configuration file named `platformio.ini` in the root directory for the project. This is a [INI-style](http://en.wikipedia.org/wiki/INI\_file)file.

`platformio.ini` has sections (each denoted by a `[header]`) and key / value pairs within the sections. Lines beginning with `;`are ignored and may be used to provide comments.

In our default `ESP32` project looks like the following:

```bash
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
```

Now, to start working with Thinger.io, it is required to add the Thinger.io client library using the `lib_deps` property:

```bash
[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino
lib_deps = thinger.io
```

After this configuration is done, it is possible to start compiling for our device. A basic example for our ESP32 device will look like the following:

{% tabs %}
{% tab title="main.cpp" %}
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
