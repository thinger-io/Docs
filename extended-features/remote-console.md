---
description: Remote console for IoT devices
---

# REMOTE CONSOLE

Remote console is an unique Thinger.io feature to allow remote access on your devices. It allows to create web terminals to interact with the device like in a serial interface. It also provides features to create interactive terminals on any Arduino compatible device. 

This feature is specially useful for remote diagnosis, show logs on devices, manage device configuration, or any other functionality, as the console can be easily extended to include new commands.

![Remote console for IoT devices](../.gitbook/assets/iot-terminal.gif)

## Web Serial Interface

By default, the Thinger.io Console can work as a web serial interface, similar to the Arduino Serial Monitor where the device write logs to serial interface. 

To use Thinger.io Console it is required to include the `ThingerConsole.h` file and create a instance over `thing` reference.

```text
#include <ThingerConsole.h>

ThingerConsole console(thing);
```

### Writing to Console

Then it is possible to use the `console` instance for logging to the remote terminal by using the same interface available in the Arduino `Serial` class, i.e, using `println`, `printf`, etc. For example, the following code will show a log every second:

```cpp
unsigned long last_log = 0;
void loop() {
  thing.handle();
  auto current = millis();
  if(current-last_log>=1000){
    last_log = current;
    console.printf("printing on serial interface: %lu\r\n", current);
  }
}
```

When the terminal is connected in the Thinger.io console, the device will start to send the configured log. If there is no any terminal connected to the device, the device will skip sending any data over the wire.

![Writing to Thinger.io web console](../.gitbook/assets/writing_to_web_serial.gif)

It is possible to check if the console is connected, so the associated loging code can be omitted, which can be useful if the log involves doing some calculation, reading a sensor, etc. The above example can be improved adding an extra check 

```cpp
unsigned long last_log = 0;
void loop() {
  thing.handle();
  
  // check if console is connected
  if(console){
    auto current = millis();
    if(current-last_log>=1000){
      last_log = current;
      console.printf("printing on serial interface: %lu\r\n", current);
    }
  }
}
```

{% hint style="info" %}
If web console is not connected, the device will not send any data over the wire.
{% endhint %}

### Reading from Console

Reading from serial can be done in the same way it is done over Arduino `Serial` interface, for example, the following code sample will check if there is any data available using the `available` method on the console \(in the same way it is used on Arduino `Serial`\) , and will print back to the console:

```cpp
void loop() {
  thing.handle();
  if(console.available()){
    console.setTimeout(100);
    String input = console.readStringUntil('\n');
    console.print("> I received: ");
    console.println(input);
  }
}
```

![Reading from Thinger.io remote console](../.gitbook/assets/console_read.gif)

## Interactive terminals

It is possible to create interactive terminals easily for handling custom commands with arguments. For working with the interactive terminal mode it is required to create and register commands on the console instance.

### Create Commands

Creating a new command in the console can be done on the `setup` function. It is required to call the function `command` over the `console` instance. This function requires two arguments: the `command name` and the `command function`. There is an optional parameters that is the `command description`, that can be useful to specify command arguments or any other help about the command.

In the following sections are presented some command examples covering different requirements, like writing to console, reading arguments, or reading from console.

#### Millis Command Example

The most simple example of a command can be defined as the following, where it is registered a command with the name `millis`, and a function that just print the result of the `millis()` function over the console terminal. It also includes a description in the third argument with this content.

```cpp
void setup() {
  // any other code
  
  // console commands
  console.command("millis", [&](int argc, char* argv[]){
    console.println(millis());
  }, "get current time millis");
}

void loop() {
  thing.handle();
}
```

This will result on a prompt with an interactive terminal in the console, with the ability to execute the `millis` function defined above.

![Millis Command Example](../.gitbook/assets/millis.gif)

{% hint style="info" %}
The function definition with the syntax `[&](int argc, char* argv[]{}`is a C++ lambda function, but it is possible to use any other`void function(int argc, char* argv[]){}`
{% endhint %}

#### Log Command Example 

There are some cases where commands require to be running during long periods, i.e., to print an ongoing log. In the following example, the command will print a sample log until the command is cancelled or the console is closed.

```cpp
void setup() {
  console.command("log", [&](int argc, char* argv[]){
      while(console.command_running()){
        console.printf("Running log at %lu\r\n", millis());
        delay(1000);
      }
  }, "show logs");
}
```

To work with long-running commands it is required to frequently check the `command_running` method, that will return false if the console has been closed, or the command was cancelled.

{% hint style="info" %}
To cancel a running command on the terminal just press Ctrl+C
{% endhint %}

![Log Command Example](../.gitbook/assets/iot_long_command_run.gif)

#### GPIO Command Example 

Some commands may need to accept arguments, i.e., turn on/off a given gpio, print a given string to a display, modify a threshold, etc. For this reason, all commands receive `argc` and `argv` parameters, like in any standard C/C++ main function. `argc` determines the number of arguments, and `argv` is an array of `char*` which holds all parsed parameters. Any command will receive at least one argument, which is the command name.

In the following example, it is created a command to modify digital state of a given GPIO. This function accepts two parameters, which are the GPIO pin number and the desired state. 

```cpp
void setup() {
  console.command("gpio", [&](int argc, char* argv[]){
      if(argc<3){
        console.error("missing parameters");
      }else{
        int pin = atoi(argv[1]);
        pinMode(pin, OUTPUT);
        digitalWrite(pin, strcmp(argv[2],"on")==0);
        console.printf("%d turned %sn", pin, argv[2]);
      }
  }, "<pin> <on|off> turn on/off a given gpio");
}
```

![GPIO Command Example](../.gitbook/assets/698jdrxvzx.gif)



#### Hello Command Example

Commands may require also to read console inputs interactively while they are running. In the following example it is outlined a simple command that ask for a name to say it hello.

```cpp
void setup() {
  console.command("hello", [&](int argc, char* argv[]){
    console.print("please, enter your name: ");
    console.flush();
    while(console.command_running() && !console.available());
    String name = console.readStringUntil('\n');
    console.printf("hello %s\r\n", name.c_str());
  }, "hello command");
}
```

Once the command is executed it will prompt the user for its name, waiting until it is ready. After typing the name and pressing enter, then it will display the name with a hello.

![Interactive Command Example](../.gitbook/assets/o9a763y2z7.gif)

### Terminal Prompt

By default, when the interactive terminal is enabled it will show a prompt with the device identifier. In the following capture, it shows the `esp32` name which is the identifier for the device.

![Default Console Prompt based on the device ID \(esp32\)](../.gitbook/assets/image%20%28429%29.png)

The prompt name can be modified by using the `set_prompt("my prompt")` method on the console instance. For example:

```cpp
void setup(){
    console.set_prompt("Arduino");
}
```

Will result on a modified prompt with the `Arduino` name.

![Custom Prompt](../.gitbook/assets/image%20%28432%29.png)

### Default Commands

By default, Thinger.io adds some general utily commands.

#### Help command

Help command will display all registered commands with an associated description.

![Help command to see the registered commands](../.gitbook/assets/image%20%28431%29.png)

#### Reboot command

Thinger.io client implements a command that will reboot your device, as in a normal computer. 

![Reboot command](../.gitbook/assets/image%20%28433%29.png)

#### Clear command

It can be useful to clear the console sometimes, so, it is a clear console command to clear all screen content.

![Clear Command](../.gitbook/assets/867dsm52ca.gif)

## SSH Connection

Console can be accessed over the Thinger.io web console, but there is also a possibility for connecting devices over standard SSH connections from the Internet \(no local network required\). This feature is a work in progress and will be released soon as a plugin!

![](../.gitbook/assets/eb2x26qtrh.gif)





