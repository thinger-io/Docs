# IDE Setup

## Arduino Framework

Arduino is currently the best framework for learning, prototyping and even developing products, thanks to its simplicity and the large community of developers that is contributing daily to increase its capabilities. That's why at Thinger.io we have developed a Software Client to easily connect Arduino based devices, which compatible with a wide variety of hardware as can be seen in the Compatible Devices/Arduino section of this documentation.

The following sections explains how to install and prepare different IDEs to work with this framework and the Thinger.io libraries

### Arduino IDE

It is probably the most simple development environment, perfect to **learn** and start creating connected projects. It is available for Windows, Macintosh and Linux distributions, and can be downloaded for free from their official website.

![](../.gitbook/assets/image%20%28252%29.png)

#### Download Arduino IDE

It is required a modern version of Arduino supporting `Library Manager` and some other features. Please install a version starting form **1.6.3** from the official Arduino download page. This step is not required if you already have a modern version.

Link-&gt; [Download Arduino IDE](https://www.arduino.cc/en/Main/Software) from their official website

#### Obtain Thinger.io libraries

The thinger.h libraries contains a Software Client that provides the easiest way to integrate with Tinger.io platform obtaining the best performance from all its features with just a couple code lines that has been explained in the [**Coding Guide**](./) ****section of this documentation. 

The library can be obtained from the Arduino  `Library Manager`, which simplifies searching and installing new libraries and also supports updating libraries when new versions are released, so actually it is preferable using this method:

![](https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/30a5f56c8917f8a26b03efb2438bfa444d531b2f.png)

> Open the **Library Manager** in the Arduino menu in `Sketch` &gt; `Include Library` &gt; `Manage Libraries`

**Search** and install the thinger.io library

![](https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/0e8bc7c86b5aff26aea7649741b592c8157cae11.png)

> Search the library with name **thinger.io** and then click `Install`. You can update the library also from this manager when it is updated.

Now the library should be be available with some default examples.

#### Manual Import

If the preferred way using the Library Manager is not working or you prefer to manage the libraries yourself, you can obtain the `.zip` library file from the official Thinger.io project Github repository by clicking in the link below. This will download a file called `Arduino-Library-master.zip`.

[Download Library &gt;](https://github.com/thinger-io/Arduino-Library/archive/master.zip)

Now **rename** the `Arduino-Library-master.zip` file to something more relevant like `thinger.zip`.

The final step is to **import** the `zip` library using the Arduino IDE. This step will uncompress and copy the `zip` library in the Arduino libraries folder. Which is usually under your Documents folder.

![](../.gitbook/assets/add-zip-library.png)

> Sketch &gt; Include Library &gt; Add .ZIP libraries

Now the library should be be available with some default examples.

### Visual Studio Code + PlatformIO Extension 

For advanced developers and bigger projects in which may be interesting to have an advanced IDE with Git control, directories and code navigation features, it would be preferable using a combination of these two amazing tools:

**Visual Studio Code** is a free distribution source-**code** editor developed by Microsoft for Windows, Linux and macOS. It can be extended via [extensions](https://en.wikipedia.org/wiki/Plug-in_%28computing%29), available through a central repository to add language support new [languages](https://en.wikipedia.org/wiki/Programming_language), [themes](https://en.wikipedia.org/wiki/Theme_%28computing%29), and [debuggers](https://en.wikipedia.org/wiki/Debugger), or perform [static code analysis](https://en.wikipedia.org/wiki/Static_code_analysis). It can be downloaded for free from the official website -&gt; [Download Visual Studio Code](https://code.visualstudio.com/download). 

![](../.gitbook/assets/image%20%28243%29.png)

**PlatformIO** is a cross-platform, cross-architecture, multiple framework, professional tool for embedded systems engineers and for software developers who write applications for embedded products. To acquire any extension, just navigate to Extensions \(`Ctrl` + `Shift` + `X` on Windows or `Command` + `Shift` + `X` on Mac\) in VS Code. Then search "Platformio" in the Marketplace searching bar and select **Install**.

![](../.gitbook/assets/image%20%28246%29.png)

#### Creating a new project 

Without going too deep into PlatformIO implementation details, work cycle of the project developed using PlatformIO is as follows:

* Users choose board\(s\) interested in [“platformio.ini” \(Project Configuration File\)](https://docs.platformio.org/en/latest/projectconf/index.html#projectconf)
* Based on this list of boards, PlatformIO downloads required toolchains and installs them automatically.
* Users develop code and PlatformIO makes sure that it is compiled, prepared and uploaded to all the boards of interest.

## Raspberry Pi

