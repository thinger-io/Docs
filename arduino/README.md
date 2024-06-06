---
description: Connecting Arduino Compatible devices to IoT
---

# DEVICES

The Thinger.io platform is designed to support almost any microcontroller or device with communication capabilities, whether it uses Ethernet, WiFi, or GSM. This flexibility allows you to choose the hardware that best suits your needs without being forced to purchase specific vendor-compatible hardware. This is crucial when designing your IoT projects.

**Steps to Connect Your Device:**

1. **Configure Your SDK**:
   * Ensure your SDK is configured according to the previous [SDK Setup instructions](../sdk-setup/).
2. **Start with Default Examples**:
   * Open the Arduino IDE or Visual Studio Code with PlatformIO.
   * Navigate to the default examples provided by Thinger.io.
   * In the Arduino IDE, go to **File > Examples > thinger.io** and choose an example for your device.
   * In Visual Studio Code, open the PlatformIO Home and select an example project compatible with your hardware.
3. **Upload the Example Code**:
   * Modify the example code if necessary to fit your specific hardware setup.
   * Upload the code to your device following the instructions for your development environment.

By following these steps, you can start connecting your device to the Thinger.io platform and leverage its full range of features.

{% hint style="info" %}
The best way to start connecting your device is by using the Arduino IDE and loading an example for your device from the Thinger.io client library in File > Examples > thinger.io.
{% endhint %}

Please, select the device typology you want to connect to get detailed instructions for each specific device.

{% content-ref url="espressif-esp32.md" %}
[espressif-esp32.md](espressif-esp32.md)
{% endcontent-ref %}

{% content-ref url="espressif-esp8266.md" %}
[espressif-esp8266.md](espressif-esp8266.md)
{% endcontent-ref %}

{% content-ref url="arduino-ethernet.md" %}
[arduino-ethernet.md](arduino-ethernet.md)
{% endcontent-ref %}

{% content-ref url="arduino-wifi.md" %}
[arduino-wifi.md](arduino-wifi.md)
{% endcontent-ref %}

{% content-ref url="arduino-gsm.md" %}
[arduino-gsm.md](arduino-gsm.md)
{% endcontent-ref %}

{% content-ref url="other-devices.md" %}
[other-devices.md](other-devices.md)
{% endcontent-ref %}
