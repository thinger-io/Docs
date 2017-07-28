<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/67c344bcaf24746012ef751c63731fa96b457e4a.gif" width="300" height="100"> 

This how-to will cover how to get the first steps while using the thinger.io platform in the Raspberry Pi. This includes how to download, compile and execute the main example available in the [GitHub Repository][1]. 

## Requirements ##

 - A raspberry running with Raspbian, and a terminal or SSH access. Other OS like Ubuntu may work, but has not been tested yet.
 - Register a device in the thinger.io console and keep the credentials by hand. If you need help with this part, please check this other [how-to][2].

Installing a newer GCC Version
------------------------------

**Note: Not required for Raspbian Jessie or newer versions.**

It is necessary to use a modern compiler to build thinger.io examples. The latest Raspbian version already provides a modern compiler, starting with Jessie, but you may install a newer compiler if you are using older Raspbian versions. It is required **at least GCC 4.8.2**. Please type `gcc -`v in a terminal to check if you need to update your compiler.

It is necessary to keep all your system updated, so please start by upgrading all the installed packages by typing the following commands. It may take some time depending on your Internet connection

    sudo apt-get update
    sudo apt-get upgrade

Next, open `/etc/apt/sources.list` in a editor and replace the name `wheezy` with `jessie`.

    sudo nano /etc/apt/sources.list

Then we are going to update the package list again, so we can access to newer jessie packages:

    sudo apt-get update

now we can finally install install GCC 4.9

    sudo apt-get install gcc-4.9 g++-4.9

Last step is to revert back from `Jessie` to `Wheezy`, open `/etc/apt/sources.list` again and replace `jessie` with `wheezy`, after that do an update of your package list:

    sudo nano /etc/apt/sources.list
    sudo apt-get update

If we type `gcc -v` at this moment, the default version is still 4.7. So we are going to change that to make the newer gcc 4.9 the default version. First remove all `gcc` alternatives.

    sudo update-alternatives --remove-all gcc
    sudo update-alternatives --remove-all g++

And now add both gcc alternatives with more priority to GCC 4.9 version. 

    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.6 40 --slave /usr/bin/g++ g++ /usr/bin/g++-4.6
    sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 60 --slave /usr/bin/g++ g++ /usr/bin/g++-4.9

At this stage, if you type `gcc-v` it should show version 4.9.2 or greater. You can always change the default compiler with the following command.

    sudo update-alternatives --config gcc

Install additional dependencies
----------------------------
It is necessary use CMake for compiling the examples and install thinger as daemon if you want. It is also required to install Open SSL if we want to securely connect to the platform trhough TLS. 

Update the apt repository first: 

    sudo apt-get update

Install Dependencies (CMake and OpenSSL)

    sudo apt-get install cmake libssl-dev

## Starting with the platform ##

Download the latest Linux Client version from GitHub.

    git clone https://github.com/thinger-io/Linux-Client.git

Enter in the Linux-Client folder we just cloned.

    cd Linux-Client

And now it is time to enter the credentials in the main project file. So we are going to edit the `main.cpp` file from `src` folder. You can use any editor you want. We will use here `nano`.

    nano src/main.cpp

In this case you must edit the fields `USER_ID`, `DEVICE_ID`, and `DEVICE_CREDENTIAL` with the information you provided while registering your device in the platform. Here is an example screenshot of how the `main.cpp` file should look like before editing these fields.
<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/2697e5c757b23eec7537fc9ac232544f5923d583.png" width="100%"> 
When you are done editing the parameters, exit the nano editor pressing `Ctrl+X` and then type '`y`' to save the changes.

Now you must add execute permissions to the `run.sh` script. 

    chmod +x run.sh

And now you can run it to test that everything is working.

    ./run.sh

If everything is going fine you should see how the program is compiled and executed automatically. The program actually reports some debug text that can help us to check if we have configured the credentials well. The expected result you should see is something like the following picture.
<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/e321714a8b9fcac120cb1dafae8502cca65e9b39.png" width="100%"> 
Now you can go to your thinger.io console and check that the Raspberry appears as connected. You can even try to execute the `sum` resource defined in the `main.cpp` that simply performs a sum. For test the device resources, please go to the API Explorer that appears in the device dashboard.

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/7b3bf8846f66eb57b422a803ac157560ea608e19.png" width="100%"> 

## Installing the client as daemon ##

At this moment, the client you have running will be stopped if you close the terminal or end the SSH connection. You can however install the client as a daemon service, so it is started automatically even if your Raspberry reboot.

To install the client you have already run as a service, please go to the Raspberry install folder:

    cd install/raspberry/

And then run the `install.sh` script, that will compile and install the client as a service. This step will copy a init script file to `/etc/init.d/thinger`, and also will copy the compiled binary file to `/usr/local/bin/thinger`. So if you want to remove the daemon client you can stop the service and remove those files.

    chmod +x install.sh
    ./install.sh 

Notice that this command will install and run the thinger.io client in background as a daemon, so if you call again the `run.sh` script, which run the standalone client, both clients will be connecting to the platform and disconnecting each other continuously. If you need to stop the background client, please use this command.

    sudo service thinger stop

## What's next? ##

Now you have to integrate your desired resources in the main.cpp file, like the `sum` example is already present. You can try to define a resource for turning on and off a led, read a sensor value, execute a system command, and so on. Some tutorials will be available soon for covering this basic functionality.

  [1]: https://github.com/thinger-io/Linux-Client
  [2]: https://community.thinger.io/t/register-a-device-in-the-console/23
