---
description: Environmental and inertial sensing device based on ESP8266 processors.
---

# ClimaStick

## ClimaStick Reference

This board is a complete Internet of Things development kit, that integrates WiFi connectivity along with a set of powerful sensors to provide environmental and motion sensing. This way, it is possible to create several connected projects easily. It is fully compatible with the Thinger.io cloud infrastructure, and provides easy to use libraries that can be used in the Arduino IDE.

### Board Layout

#### ClimaStick V1.1:

![](../.gitbook/assets/i0.PNG)

#### ClimaStick V2:

![](../.gitbook/assets/i0_7.png)

### Board Features

* Environmental sensing for temperature, relative humidity, barometric pressure, and lux intensity. A micro weather station!
* Inertial Measurements Unit \(IMU\), integrating an accelerometer, a gyroscope, and a digital compass.
* Li-Po Charger. It is able to charge \(and be powered by\) batteries from a solar panel or the built-in USB.
* RGB Led \(only in ClimaStick V1\).
* User button.
* Fully compatible with the Arduino Environment. Can be programmed directly from the Arduino IDE. There are libraries for reading the sensors and connecting the board to the Thinger.io Cloud or other Internet services.

### Sample Use Cases

* Education: This board provides an easy to use environment for education. It is fully compatible with the Arduino IDE, so they can still use this friendly environment. It also integrates multiple sensors that the students can use for building their projects. The students can start coding projects directly without wiring multiple sensors and controllers in big protoboards, avoiding possible shorts and burns. Moreover, it integrates WiFi, so they can get one step further building connected solutions, building dashboards, sending emails, recording data, and so on.
* Remote Telemetry: It provides a full IMU with Wifi connectivity that can be used for remote telemetry. The low weight of the device \(4gr\) and the option for setting a different power supply than the USB, make this board an excellent option for monitoring drones or UAVs in real-time.
* Industry 4.0: It can be used in industrial environments for predictive maintenance, as it is possible to measure vibrations, temperature, and humidity in real-time and determine if the sensed parameters are between normal operation thresholds.
* Weather Station: This device can be used as a micro weather station. It can be powered easily from a battery and a solar panel, and collaborate with weather platforms, or just store your information in the Thinger.io cloud.

## Configure Environment

This section covers how to setup your computer to start working with the ClimaStick device.

### Install required components

* You may need to install the CP2102 drivers from Silicon Labs if the ClimaStick device is not recognized from your computer. This driver is for the USB to serial interface to communicate with the board.

[Download page &gt;](http://www.silabs.com/products/mcu/pages/usbtouartbridgevcpdrivers.aspx)

* Arduino IDE v1.6.13 or newer. 

[Download page &gt;](https://www.arduino.cc/en/main/software)

### Configure Arduino IDE

1- Open File &gt; Preferences &gt; Additional\_Boards\_URL\_Manager to include the "ESP8266 boards manager link" that you can retrieve from [Github community project](https://github.com/esp8266/Arduino). It is normally `http://arduino.esp8266.com/stable/package_esp8266com_index.json`

![](../.gitbook/assets/i4.PNG)

2- Open Tools &gt; Boards &gt; Boards Manager... and search for ESP8266 package, then install the last version.

![](../.gitbook/assets/i5.PNG)

3- Now you can program almost any ESP82XX processor directly from the Arduino IDE. From the Tools &gt; Boards you should see now the new ESP8266 community boards installed.

1. For program ClimaStick V1 select **NODE\_MCU V1.0 \(ESP-12E Module\)**.
2. For program ClimaStick V2 select **WeMos D1 Mini Lite**.

![](../.gitbook/assets/i6.png)

4- Open Sketch &gt; Include Library &gt; Manage Libraries, and search for **Thinger.io** libraries. Then install the Thinger.io and ClimaStick libraries, as shown in the following picture.

![](../.gitbook/assets/climastick_libraries.png)

5- Connect the ClimaStick to your computer and select its serial communication port number on: Tools &gt; Port. It normally will be a COM port, or named as /dev/cu.SLAB\_USBtoUART on Mac.

6- Now you can start developing with Thinger.io ClimaStick! It is helpful to start with the examples provided in the library, by opening File &gt; Examples &gt; ClimaStick

### Uploading firmware

The ClimaStick board can be programmed directly by pressing the Upload button of the Arduino IDE. If it does not work, please check the following:

* Be sure that your micro USB wire allows data transmission. Some cables are only for electrical power and may not work properly.
* Verify that your operating system properly recognize the CP2102 serial port interface.
* Checkout the selected serial COM port on Arduino IDE: Tools &gt; Port
* ⚠ **Flash boot mode:** If you are sure that everything is configured properly, and the problem still persists, you can force a flash boot up by keeping pressed the USR button of the board, then press the RST button once, and finally release the USR button.

## QuickStart Examples

Now you should be ready to open any example of the ClimaStick library and upload it to your ClimaStick device. On Arduino IDE, open File &gt; Examples &gt; ClimaStick and you will find some examples that illustrates the most useful functions over the board features, like sensors, like accessing the IMU, reading battery levels, read weather conditions, changing led state, and so on.

### ClimaStick Auto

The ClimaStick\_Auto example code, is a little sketch that integrates all ClimaStick functionality, defining all sensor resources that will be accesible from the Thinger.io Platform:

```cpp
#include <ClimaStick.h>

#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"

ClimaStick thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

void setup() {
  // configure board wifi
  thing.add_wifi(SSID, SSID_PASSWORD);
  // initialize board sensors
  thing.init_sensors();
  // define resources for all features
  thing.init_resources();
}

void loop() { 
  thing.handle();
}
```

### Data Recording using Sleep

You can easily configure your board for record environment values and then going to sleep. This is quite useful while powering the device from a battery. This example will write the environment values to the `BucketId`, that you must create in your Thinger.io console.

```cpp
#include <ClimaStick.h>

#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"

ClimaStick thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

void setup() {
  // configure board wifi
  thing.add_wifi(SSID, SSID_PASSWORD);
  // initialize board sensors
  thing.init_sensors();
  // define the "environment" resource
  thing.init_environment_resource();
}

void loop() { 
  thing.handle();
  // write to bucket BucketId
  thing.write_bucket("BucketId", "environment");
  // sleep the device 60 seconds
  thing.sleep(60);
}
```

This example is quite useful while requiring accurate temperature values. Due to the low power dissipation capacity, the small ClimaStick board gets hot after a few seconds of working, so it is not possible to make accurate temperature readings while the device is working constantly. For reading temperature more accurately, it is possible to sleep the processor, and wait a few minutes before trying to get a fresh sensor read. The board supports the `thing.sleep(seconds)` function, that will sleep the processor and all WiFi transmissions. After the sleep, the device will start again like a normal reboot.

> **⚠ DEEPSLEEP CONSIDERATIONS:**
>
> * During the deepSleep mode, it is not possible to flash code. To change the program you will need to make a forced flash mode boot up as described in the Uploading firmware section.
> * Note that, when the processor makes a hard reset, all dynamic variables will lost its values.

After some time, your bucket should look like:

![](../.gitbook/assets/climastick_bucket.png)

That will allow you to create historical dashboards like the following:

![](../.gitbook/assets/climastick_dashboard.png)

## ClimaStick Functions

You can run the ClimaStick functions to read any sensor value as required by your application. Here we describe some of the most important functions. Using these functions requires always to initialie the sensors on the setup method:

```cpp
void setup() {
    thing.init_sensors();
}
```

### Reading Accelerometer

```cpp
Accelerometer accel = thing.get_acceleration();
Serial.println(accel.ax);
Serial.println(accel.ay);
Serial.println(accel.az);
```

### Reading Gyroscope

```cpp
Gyroscope gyro = thing.get_gyroscope();
Serial.println(gyro.gx);
Serial.println(gyro.gy);
Serial.println(gyro.gz);
```

### Reading Compass

```cpp
Compass compass = thing.get_compass();
Serial.println(compass.heading);
Serial.println(compass.headingDegrees);
```

### Reading Magnetometer

```cpp
// you can also call thing.get_raw_magnetometer() to read raw values and not normalized
Magnetometer magnet = thing.get_magnetometer();
Serial.println(magnet.x);
Serial.println(magnet.y);
Serial.println(magnet.z);
```

### Reading Temperature

```cpp
float temperature = thing.get_temperature();
Serial.println(temperature);
```

### Reading Humidity

```cpp
float humidity = thing.get_humidity();
Serial.println(humidity);
```

### Reading Pressure

```cpp
float pressure = thing.get_pressure();
Serial.println(pressure);
```

### Reading Lux

```cpp
uint16_t lux = thing.get_lux();
Serial.println(lux);
```

### Change RGB Led \(Only in ClimaStick V1\)

```cpp
 int r=255, g=0, b=0;
 thing.set_rgb(r, g, b);
 // or
 thing.set_rgb("blue");
```

## OTHER CONSIDERATIONS

This section covers different considerations while using the board.

### General Considerations

* The device should be powered with a 5V USB power supply, being able to provide current from 250 to 1000mah.
* This board has a low heat dissipation capacity, so it is normal that it keeps hot on high transmission processes. The temperature sensor could read high values while doing full duplex communication process.
* This device is developed for prototyping and support software development, so it is not protected to support hard weather conditions without the proper cover case.
* Try to avoid touching the components surfaces while using the device, it can produce an electrostatic shock on the device, producing shortcuts and malfunction. It is recommended to take the board from the edges like in the following illustration.

![](../.gitbook/assets/i2.PNG) 

* If necessary, clean the circuit using a non-damaging contact cleaner like Isopropyl alcohol and soft brush. 
* Store in a cool, dry place. Protected from dust.

### External Power Supply

* If you use the VIN power header, be careful to connect it in the correct position, as it is shown in the following image. Not following this directive could damage the protection diode.

![](../.gitbook/assets/i1.PNG) 

* ⚠ Do not use VIN power supply and USB power supply at the same time! It can damage your hardware.

### Battery Power Supply

* You can power and charge a battery directly from the board. Use the BAT power header, and take care to of the polarity, as it is shown in the following picture:

![](../.gitbook/assets/i3.PNG) 

* BAT header is connected to a lithium battery charger that can manage 3.7Vdc, 500mah Li-Po / li-ion batteries charge and discharge process.
* To charge a battery, connect it on BAT header and power on the ClimaStick through USB / VIN connectors. The battery charger will manage the charging voltage to increase life battery and stops charging cycle when voltage ups to 4.2Vdc.
* ⚠ If you are using a different battery, plug it on VIN connector.
* ⚠ If cell voltage drops under 3.6V, an automatic battery protection circuit will power off the system.

## Device Documents

### Datasheets

Your can download the device datasheet from the following link:

[ClimaStick V1 Datasheet &gt;](https://github.com/thinger-io/Docs/tree/9fc057586e6704dcf058d1a33a7f25ae648c002c/hardware/climaStick/assets/ClimaStick_Datasheet.pdf)

[ClimaStick V2 Datasheet &gt;](https://github.com/thinger-io/Docs/tree/9fc057586e6704dcf058d1a33a7f25ae648c002c/hardware/climaStick/assets/ClimaStick_V2_Datasheet.pdf)

### Design files

You can download and edit the device files using eagle cad:

ClimaStick V1 design files \(.sch & .brd\)

ClimaStick V2 design files \(.sch & .brd\)

### Disclaimer

* This device is commercialized by the Thinger.io platform \(THINK BIG LABS S.L\) as a development kit, so it is not subject to commerce homologation rules. The device owner is liable for all injuries to third parties and damage to their properties. 

