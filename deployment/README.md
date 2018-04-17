# Install on Ubuntu (starting from 16.04)

The server can be installed in any architecture, like x86, amd64, arm64, or armhf, which is compatible with the Ubuntu Snap packages (on only Ubuntu OS, check out [this page](https://snapcraft.io/)) . However, it is recommended to use 64 bit architectures, as the MongodB database is limited to 2GB of data in 32 bits systems.


It is highly recommended to update your Ubuntu installation before doing any other step by running this commands:

```bash
sudo apt update
sudo apt upgrade
``` 


## Install MongoDB
	
Thinger.io IoT platform requires a MongoDB server for storing some server information. So, the first step is to install a MongoDB Server in your host. The following information has been obtained from the official documentation. 
[https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

*Notice* that this steps are for amd64/arm64 architectures. If you are installing the server in a different system, you should check the specific install instructions for the architecture.


### Import the public key used by the package management system.

The Ubuntu package management tools (i.e. dpkg and apt) ensure package consistency and authenticity by requiring that distributors sign packages with GPG keys. Issue the following command to import the MongoDB public GPG Key:

```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```

### Create a list file for MongoDB


Create the /etc/apt/sources.list.d/mongodb-org-3.4.list list file:

```bash
echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```

### Reload local package database.

Issue the following command to reload the local package database:

```bash
sudo apt update

```

### Install the MongoDB packages

```bash
sudo apt install -y mongodb-org
```


### Make MongoDB run at startup

Edit the following file to make MongoDB run at startup as a service.
```bash
sudo nano /etc/systemd/system/mongodb.service
```

Then copy the following configuration and save the file.

```bash
[Unit]
Description=High-performance, schema-free document-oriented database
After=network.target

[Service]
User=mongodb
ExecStart=/usr/bin/mongod --quiet --config /etc/mongod.conf

[Install]
WantedBy=multi-user.target
```

### Start MongoDB

Now, start the MongoDB instance and enable it as a system service.

```bash
sudo systemctl start mongodb
sudo systemctl enable mongodb
```

### Check that MongoDB is running

```bash
sudo systemctl status mongodb
```

```bash
alvaro@supermicro:~$ sudo service mongod status
● mongod.service - High-performance, schema-free document-oriented database
   Loaded: loaded (/lib/systemd/system/mongod.service; disabled; vendor preset: enabled)
   Active: active (running) since sáb 2017-01-21 10:56:13 CET; 9h ago
     Docs: https://docs.mongodb.org/manual
 Main PID: 3825 (mongod)
    Tasks: 87
   Memory: 77.5M
      CPU: 2min 35.209s
   CGroup: /system.slice/mongod.service
           └─3825 /usr/bin/mongod --quiet --config /etc/mongod.conf

ene 21 10:56:13 supermicro systemd[1]: Started High-performance, schema-free document-oriented database.
```

## Install Thinger.io Maker Server


### Snap Command
Installing the server is as easy as installing a snap package. Just type in your terminal.


```bash
sudo snap install thinger-maker-server

```

### Ubuntu Store

You can also install the server by installing it from the Ubuntu Store. Just search for `Thinger.io` and the package should appear.


## Check service status

You can check the status of the Thinger.io daemon service by running:

```
sudo service snap.thinger-maker-server.thingerd status
```

It should return a result like the following:

``` bash
alvaro@supermicro:/var/snap/thinger-maker-server/common$ sudo service snap.thinger-maker-server.thingerd status
● snap.thinger-maker-server.thingerd.service - Service for snap application thinger-maker-server.thingerd
   Loaded: loaded (/etc/systemd/system/snap.thinger-maker-server.thingerd.service; enabled; vendor preset: enabled)
   Active: active (running) since vie 2017-01-20 22:39:19 CET; 4s ago
  Process: 30329 ExecStart=/usr/bin/snap run thinger-maker-server.thingerd (code=exited, status=0/SUCCESS)
 Main PID: 30340 (thingerd)
    Tasks: 49
   Memory: 8.4M
      CPU: 73ms
   CGroup: /system.slice/snap.thinger-maker-server.thingerd.service
           └─30340 thingerd --fork --runpath=/var/snap/thinger-maker-server/common

ene 20 22:39:19 supermicro systemd[1]: Starting Service for snap application thinger-maker-server.thingerd...
ene 20 22:39:19 supermicro systemd[1]: Started Service for snap application thinger-maker-server.thingerd.

```

At this moment you should be able to open a browser pointing to your server IP address, and the web console should appear, just like the cloud console.

<p align="center">
    <img src="assets/console.png" width="650">
</p>

### Restart service

You can reload the service if you need to refresh config files.

```
sudo service snap.thinger-maker-server.thingerd restart
```

#### Stopping the service

Or you can just stop the service when required.

```
sudo service snap.thinger-maker-server.thingerd stop
```

# Thinger.io Configuration

## Config file

When using the snap package, the default config files, buckets exports, and logs are stored in:

```bash
/var/snap/thinger-maker-server/common/
```

```json
{

  "deployment" : {
    "contact_email" : "admin@thinger.io"
  },

  "ssl" : {
    "ssl_certificate" : "certificates/server.crt",
    "ssl_certificate_key" : "certificates/server.key",
    "tmp_dh_file" : "certificates/dh2048.pem",
    "ssl_ciphers" : "ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA256:ECDHE-RSA-AES256-SHA:ECDHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:DES-CBC3-SHA:HIGH:!aNULL:!eNULL:!EXPORT:!DES:!MD5:!PSK:!RC4",
    "ssl_prefer_server_ciphers" : true
  },

  "http_server" : {
    "address" : "0.0.0.0",
    "port" : "80",
    "ssl_port" : "443",
    "hosts" : [
      {
        "host": "*",
        "type": "rest",
        "cors": {
          "enabled" : true
        },
        "web_fallback" : {
          "enabled" : true,
          "root": "${SNAP}/console"
        }
      }
    ]
  },

  "thing_server" : {
    "address" : "0.0.0.0",
    "port" : "25200",
    "ssl_port" : "25202"
  },

  "database" : {
    "type" : "mongodb",
    "mongodb" : {
      "host" : "localhost",
      "database" : "thinger"
    }
  },

  "buckets" : {
    "storage" : {
      "type" : "mongodb",
      "mongodb" : {
        "host" : "localhost",
        "database" : "thinger_data",
        "table" : "buckets_data"
      }
    },
    "export" : {
      "type" : "filesystem",
      "filesystem" : {
        "export_path": "exports"
      }
    }
  },

  "util" : {
    "maxmind_database" : "data/GeoLite2-City.mmdb"
  },

  "log" : {
    "enabled" : false,
    "level" : "info",
    "output" : {
      "file" : {
        "enabled" : true,
        "flush" : true,
        "log_path" : "logs"
      },
      "clog" : {
        "enabled" : false
      }
    }
  },

  "rate_limiter" : {
    "enabled" : false,
    "type" : "memory"
  },

  "accounts" : {
    "invalid_usernames" : [],
    "invalid_email_domains" : [],
    "required_email_domains" : [],
    "require_email_verification": false,
    "min_password_length" : 6,
    "limits" : {
      "devices" : {
        "max_count" : -1
      },
      "buckets" : {
        "max_count" : -1,
        "min_interval" : -1
      },
      "endpoints" : {
        "max_count" : -1,
        "min_interval" : -1
      },
      "dashboards" : {
        "max_count" : -1
      },
      "tokens" : {
        "max_count" : -1
      }
    }
  }
}
```

Documentation in progress...

## Configure SMTP Server

It is possible to configure an SMTP Server for sending emails through the endpoints, for the sign in process, forgot password, etc. Just add another field with the following information. The following is an example for Gmail:

``` json
"email" : {
    "type" : "smtp",
    "domain" : "gmail.com",
    "sender" : "alvarolb",
    "smtp" : {
        "host" : "smtp.gmail.com",
        "port" : "465",
        "username" : "alvarolb@gmail.com",
        "password" : "your app password goes here (required if 2FA is enabled)",
        "secure" : true
    }
},
```

# Connect the devices to your server


## Arduino Devices

Connecting the devices to your own server, does not require a complex setup. In your sketch, just add a definition to your server, by adding the `THINGER_SERVER` define pointing you your server IP Address or host name, as in the following example:

```cpp
#define THINGER_SERVER "192.168.1.120"

#include <ESP8266WiFi.h>
#include <ThingerESP8266.h>

#define USERNAME "your_user_name"
#define DEVICE_ID "your_device_id"
#define DEVICE_CREDENTIAL "your_device_credential"

#define SSID "your_wifi_ssid"
#define SSID_PASSWORD "your_wifi_ssid_password"

ThingerESP8266 thing(USERNAME, DEVICE_ID, DEVICE_CREDENTIAL);

void setup() {
  pinMode(BUILTIN_LED, OUTPUT);

  thing.add_wifi(SSID, SSID_PASSWORD);

  // digital pin control example (i.e. turning on/off a light, a relay, configuring a parameter, etc)
  thing["led"] << digitalPin(BUILTIN_LED);

  // resource output example (i.e. reading a sensor value)
  thing["millis"] >> outputValue(millis());

  // more details at http://docs.thinger.io/arduino/
}

void loop() {
  thing.handle();
}
```

Note: The `THINGER_SERVER` definition must appear before any other includes in the Sketch.
