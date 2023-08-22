---
description: Track system metrics from Thinger.io Platform
---

# Server Monitoring Client

The Linux Monitoring Client is a software module based on the Thinger.io Linux Client that keeps track of different system metrics, and publishes them to the Thinger.io Platform in order to keep track of a server or an embedded Linux device or a fleet of them.

## Resource Tracking

The client tracks different metrics for System Information as well as for six different types of resources: general system information, CPU, system memory, network interfaces, filesystem mount points, and I/O drives.

Apart from uploading the resource information to Thinger.io Platform, the resources can be queried directly to the client or from the local server running the client at http://localhost:7890/monitor

### General System Information

There is some system information that can be relevant from a server administration standpoint.

Currently, the metrics the client is retrieving are:

* Hostname
* Uptime
* OS version
* Quantity of normal and security updates (Ubuntu server)
* If a reboot is required to apply updates (Ubuntu server)

### CPU

To evaluate the performance of the CPU, the following metrics are tracked:

* Number of cores
* CPU usage
* CPU load for 1m, 5m and 15m

### System Memory (RAM)

As having reliable information about the system memory used is vital for capacity planning and maintaining the integrity of your servers, the following metrics are collected:

* Total, used and available memory capacity
* Memory usage
* Total, used and available swap memory capacity
* Swap memory usage

### Network Interfaces

For each network interface the following is tracked:

* Internal/Private IP
* Incoming and Outgoing network speed
* Incoming and Outgoing total network transfer

It will also retrieve the public IP of the server.

### Filesystem Mount Points

Each filesystem corresponds to one partition of a drive, from which it will track:

* Capacity
* Capacity free
* Capacity used
* Capacity usage (percentage)

### I/O Drives

To provide insight on how a server is performing, the following I/O drive metrics are monitored:

* Input and Output operation speed
* Disk usage

## Configuration

The basic device configuration is tracked over a JSON file located in the server. Additional configurations are tracked trough the device properties to allow live configuration changes through the Thinger.io Platform.

### Basic device configuration

The configuration will be auto created on first launch when it is auto provisioned.

The file structure is as follows:

```javascript
{
  "device": {
    "credentials": "<device_credentials>",
    "id": "<device_id>",
    "name": "<device_name>"
  },
  "server": {
    "ssl": true,
    "url": "<server_to_connect_url",
    "user": "<thinger.io_account>"
  }
}
```

{% hint style="info" %}
If launched with root, the default path for the configuration file is: /etc/thinger\_io/monitor/app.json

If launched with non-root, the default path for the configuration file is: /home/\<user>/.config/thinger\_io
{% endhint %}

### Configuration properties

As mentioned before, the configuration for the resources will be in the device properties.

It contains five sections:

* defaults: boolean value indicating whether the name of the first element of each resource will have a default name instead of the resource name. Useful when tracking different devices with one dashboard.
* interfaces: JSON array with the name of the network interfaces to track.
* filesystems: JSON array with the mount point of the partitions to track.
* drives: JSON array with the Linux devices to track.
* server: Object containing the host IP to listen in and port to launch the local server.

Here is an example `resources` property value:

```json
{
  "defaults": true,
    "drives": [
      "xvda"
    ],
    "filesystems": [
      "/"
    ],
    "interfaces": [
      "eth0"
    ],
    "server": {
      "host": "0.0.0.0",
      "port": 7890
    }
}
```

## First Launch

This module has the feature to auto provision the device in the Thinger.io Platform, but also allows to input a configuration if it's already provisioned.

It is also capable of creating a system service so the monitor is always running regarding if the server has been rebooted.

### With auto provision

Download the installation script from the [last software release](https://github.com/thinger-io/monitoring-client/releases/latest), and run the software as:

```
./install_thinger_monitor.sh -t <create_device token> -u <user_id>
```

{% hint style="info" %}
For update and reboot abilities it needs to be run with root user
{% endhint %}

### Without auto provision

If the device is already provisioned, we will need to set the user id, device id and credentials in the configuration file and launch the software as:

```
./thinger_monitor [-c <config file path>]
```

{% hint style="warning" %}
If the operating system is not Ubuntu or OpenSSL is installed in a different folder than default, it is neccesary to indicate the certificates directory by declaring the variable SSL\_CERT\_DIR before calling the installer or the binary:

Ex: `SSL_CERT_DIR=/etc/ssl/certs ./installer_thinger_monitor.sh`
{% endhint %}
