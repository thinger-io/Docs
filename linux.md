# LINUX / RASPBERRY PI

![](.gitbook/assets/linux-versions.png)

This how-to will cover how to get the first steps while using the thinger.io platform in the Raspberry Pi. This includes how to download, compile and execute the main example available in the [GitHub Repository](https://github.com/thinger-io/Linux-Client).

## Requirements

* A Raspberry Pi running with Raspbian, and a terminal or SSH access. Other OS like Ubuntu or Debian may work, but has not been tested yet. This tutorial has been tester with buster version.
* Register a device in the thinger.io console and keep the credentials by hand. If you need help with this part, please check this other [how-to](https://community.thinger.io/t/register-a-device-in-the-console/23).

## Install development dependencies

Thinger.io implementation for Linux requires some tools and libraries for its compiling:

* A C++ compiler (GCC or Clang)
* CMake to guide the compiling and search installed libraries
* OpenSSL for using secure connections with the platform
* Boost Libraries used for high performance async input/output&#x20;

To install this dependencies, update the apt repository and installed packages first.

```
sudo apt update
sudo apt upgrade
```

Then, install the described packages:

```bash
sudo apt install gcc g++ cmake libssl-dev libboost-system-dev libboost-filesystem-dev libboost-thread-dev libboost-program-options-dev libboost-regex-dev
```

Download the latest Linux Client version from GitHub.

```bash
git clone https://github.com/thinger-io/IOTMP-Linux.git
```

Enter in the IOTMP-Linux folder we just cloned.

```bash
cd IOTMP-Linux
```

Create a build folder and enter into it:

```bash
mkdir build
cd build
```

Run CMake

```bash
cmake ../
```

If everything goes fine, it should display something like:

```bash
pi@RevPi20679:~/thinger_iotmp_linux_client/build $ cmake ../
-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Performing Test COMPILER_SUPPORTS_CXX17
-- Performing Test COMPILER_SUPPORTS_CXX17 - Success
-- Looking for pthread.h
-- Looking for pthread.h - found
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Failed
-- Looking for pthread_create in pthreads
-- Looking for pthread_create in pthreads - not found
-- Looking for pthread_create in pthread
-- Looking for pthread_create in pthread - found
-- Found Threads: TRUE  
-- Found OpenSSL: /usr/lib/arm-linux-gnueabihf/libcrypto.a (found version "1.1.1n")  
-- OpenSSL Version: 1.1.1n /usr/include /usr/lib/arm-linux-gnueabihf/libssl.a;-lpthread;dl /usr/lib/arm-linux-gnueabihf/libcrypto.a;-lpthread;dl
-- Found Boost: /usr/include (found version "1.67.0") found components: system thread regex program_options date_time chrono atomic 
-- Configuring done
-- Generating done
-- Build files have been written to: /home/pi/thinger_iotmp_linux_client/build
```

Then, run make to generate the binary.&#x20;

```bash
make
```

After it compiles, it is possible to execute the binary by providing the username, device, and credentials parameters.

```bash
pi@RevPi20679:~/thinger_iotmp_linux_client/build $ ./thinger -u username -d device -p credential --host "perf.aws.thinger.io" &
[1] 2442
pi@RevPi20679:~/thinger_iotmp_linux_client/build $ date       time         ( uptime  ) [ thread name/id ]                   file:line     v| 
2022-09-06 19:54:46.635 (   0.001s) [main thread     ]             loguru.cpp:647   INFO| arguments: ./thinger -u alvarolb -d macbook -p macbook --host perf.aws.thinger.io
2022-09-06 19:54:46.636 (   0.001s) [main thread     ]             loguru.cpp:650   INFO| Current dir: /home/pi/thinger_iotmp_linux_client/build
2022-09-06 19:54:46.636 (   0.002s) [main thread     ]             loguru.cpp:652   INFO| stderr verbosity: 0
2022-09-06 19:54:46.636 (   0.002s) [main thread     ]             loguru.cpp:653   INFO| -----------------------------------
2022-09-06 19:54:46.643 (   0.009s) [main thread     ]        asio_client.hpp:97    INFO| [CLIENT] Starting ASIO client...
2022-09-06 19:54:46.644 (   0.009s) [main thread     ]              thinger.h:242   INFO| [SOCKET] Connecting to perf.aws.thinger.io:25206 (TLS: 1)
2022-09-06 19:54:46.763 (   0.129s) [main thread     ]              thinger.h:251   INFO| [SOCKET] Connected!
2022-09-06 19:54:46.763 (   0.129s) [main thread     ]              thinger.h:263   INFO| [THINGER] Authenticating. user: '', device: ''
2022-09-06 19:54:46.764 (   0.130s) [main thread     ]              thinger.h:719   INFO| [MSG_OUT] (CONNECT) (2:1) (3:["username","device","password"]) (1:17767) 
2022-09-06 19:54:46.793 (   0.159s) [main thread     ]              thinger.h:677   INFO| [MSG__IN] (OK) (1:17767) 
2022-09-06 19:54:46.794 (   0.160s) [main thread     ]              thinger.h:266   INFO| [THINGER] Authenticated!
2022-09-06 19:54:47.795 (   1.161s) [main thread     ]              thinger.h:719   INFO| [MSG_OUT] (KEEP_ALIVE) 
2022-09-06 19:54:47.818 (   1.184s) [main thread     ]              thinger.h:677   INFO| [MSG__IN] (KEEP_ALIVE) 
2022-09-06 19:54:47.818 (   1.184s) [main thread     ]              thinger.h:278   INFO| [THINGER] Keep alive received
```
