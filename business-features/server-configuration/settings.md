# Server Settings

In order to provide the managers of private Thinger.io instances a full configuration capability of the service, the web console provides a host management tool in which different sections allow to set some criteria of its operation in a simple way. It is accessible in the "**Cluster Hosts**" tab of the main menu, which will display the Host List as shown in the image below:

![](<../../.gitbook/assets/image (286).png>)

After selecting a server profile, in the top-right corner of the server status dashboard, it is possible to select the option "Settings" in which the next configuration options will be displayed:

## IoT Server Settings

This platform allows the connection of devices of different types, for which it has been equipped with different data entry engines. Each section explained below allows to set up the IP address and the ports through which each device type is connected.

### HTTP Server configuration

To set the connection parameters of the [**HTTP devices integration**](../../http-devices.md) module on Thinger.io private instances

![](<../../.gitbook/assets/image (550).png>)

The configurable parameters are:&#x20;

* **Enabled:** Allows to disable the functionality of this ingest module&#x20;
* **Listen Address:** Allows to indicate the fixed IP address of the server, leaving it at 0.0.0.0 will use the default IP of the host.
* **TCP Port:** To configure the standard HTTP communications port with a customized value.
* **TLS Port:** To configure the standard TLS request collection port with a customized value.

### Thinger Server configuration

It allows to set up the parameters for connecting Thinger.io to devices running the Thinger.io software client.

![](<../../.gitbook/assets/image (558).png>)

The configurable parameters are:&#x20;

* **Enabled:** Allows to disable the functionality of this ingest module&#x20;
* **Listen Address:** Allows to indicate the fixed IP address of the server, leaving it at 0.0.0.0 will use the default IP of the host.
* **TCP Port:** To configure the standard TCP communications port with a customized value.
* **TLS Port:** To configure the standard TLS request connection port with a customized value.

### MQTT Server configuration

This section allows setting the connection and data entry parameters of [**MQTT devices**](../../mqtt.md) with Thinger.io's builtin MQTT broker.

![](<../../.gitbook/assets/image (520).png>)

The configurable parameters are:&#x20;

* **Enabled:** Allows to disable the functionality of this ingest module&#x20;
* **Listen Address:** Allows to indicate the fixed IP address of the server, leaving it at 0.0.0.0 will use the default IP of the host.
* **TCP Port:** To configure the standard MQTT communications port with a customized value.
* **TLS Port:** To configure the standard MQTT TLS connection request port with a customized value.

## Email Settings

This section allows configuring the mail server in charge of sending notifications to the users of the IoT instance. A standard SMTP server can be used, but if the client has an AWS SES (Simple Email Service), which is more scalable and easier to use, it can be configured here to be used instead.

![](<../../.gitbook/assets/image (413).png>)

In both cases, domain and sender parameters can be configured from the main section, then depending on the selecting service some specific parameters need to be specified:

### SMTP configuration

The **Simple Mail Transfer Protocol**, better known as SMTP, is a protocol used to transmit email messages over the internet. Thinger.io server instances contains an SMTP server that allows sending notifications to the instance users, that has been configured by default to use the same web domain as the IoT server host and the standard parameters, however,  these parameters can be customized by changing the server:

![](<../../.gitbook/assets/image (539).png>)

* **Host:** Is the SMTP address or web domain
* **Port:** Custom port to be used in order to send the notifications
* **Username**: SMTP username credentials&#x20;
* **Password:** SMTP username password
* **SSL/TLS** **Connection:** should be enabled if the SMTP has been installed in a different host but can be disabled if it is running in the same one.

### AWS SES configuration

The integration with Amazon SES provides a much simple and scalable mailing tool. It can be selected instead of the common SMTP by selecting it on Email Type, and placing the credentials on its appropriate section. These credentials can be obtained on the AWS SES configuration section as explained on[ **this link**](https://ongage.atlassian.net/wiki/spaces/HELP/pages/13795743/Amazon+SES+Setup+Tutorial)

![](<../../.gitbook/assets/image (584).png>)

## Buckets Database Settings

The Thinger.io Data buckets system is very flexible, it can be configured to use the user's preferred system for the different storage and export processes supported by the platform

### Storage Engine Settings

Thinger.io's data bucket storage engine can be configured to use different database systems such as MongoDB, DynamoDB, or InfluxDB (used by default in private instances). The "Buckets" section of "Host Settings" allows you to select the desired system via the "General" section and connect it to a database server of the same type, both for storage and export of the bucket data to files.

#### Using InfluxDB to store Buckets data

The default configuration of Thinger.io's storage engine uses InfluxDB system, as it provides some interesting features as data aggregation or using data tags to classify data over specific criteria. To set a different configuration of InfluxDB, the next four parameters need to be introduced, as shown in the image below:

![](<../../.gitbook/assets/image (395).png>)

* **InfluxDB Host:** Place here the web domain or IP of the InfluxDB server
* **InfluxDB Port:** Place here te standard or custom InfluxDB port number
* **InfluxDB Username:** Is the username used to create the influxDB database
* **InfluxDB Password:** An authorization tocken obtained from InfluxDB configuration console&#x20;

To obtain this information about an InfluxDB server it is required to go to InfluxDB configuration tool as it is explained on: &#x20;

#### Using DynamoDB to store Buckets data

DynamoDB is a highly scalable and consistent storage system provided by AWS that performs really well on IoT projects. It is possible to configure a DynamoDB system to be used by Thinger.io server to store data by selecting DynamoDB on the General section and introducing the next data into the system

![](<../../.gitbook/assets/image (414).png>)

* **Table Name:** It is the identification of the DynamoDB table&#x20;
* **AWS Access Key ID:**&#x20;
* **AWS Secret Access Key:**
* **Region:**  Enter here the AWS region in which the database is placed&#x20;

DynamoDB database configuration parameters can be obtained from the server as explained on AWS documentation page.

#### Using MongoDB to store Buckets data

Mongo DB is one of the most widespread database systems due to its robust operation and because it is an open-source project that offers free storage services for developers (companies that wish to make premium use of MongoDB can use the Atlas version). This database system can be used to store Thigner.io bucket data by entering a URI and some credentials:

![](<../../.gitbook/assets/image (387).png>)

* **MongoDB URI:** Paste here the uniform resource identification obtained from MongoDB configuration console
* **MongoDB Database:** Enter here the MongoDB database identificator
* **MongoDB Table:** Enter here the MongoDB table name in which the data will be stored

The server URI can be obtained from the "connect" context of the Mongo DB or Atlas service as explained at this link.

### Bucket Export Settings

When a specific bucket is requested to be exported, the system creates a file to be stored in the instance's database. The export system allows to select the database system where the resulting files will be stored in the same MongoDB used to manage Thinger.io's user accounts, in the filesystem of the server instance or using Amazon S3 (more scalable option and used by default in the public Thigner.io instances).

![](<../../.gitbook/assets/image (324).png>)

To implement AWS S3 option, it must be selected on the General section and then configured to include the parameters shown below:

![](<../../.gitbook/assets/image (562).png>)

* **Bucket ID:** It is the identification of the AWS S3 data bucket&#x20;
* **AWS Access Key ID:**&#x20;
* **AWS Secret Access Key:**
* **Region:**  Enter here the AWS region in which the database is placed&#x20;

DynamoDB database configuration parameters can be obtained from the server as explained on AWS documentation page.

## SSL Settings

Communications between the devices and Thinger.io are secured using the TLS SSL protocol. The chain introduced by default is an ordered list of the security protocols to be used for the key exchange, modifying that order we can balance between a more secure configuration, or better performance or compatibility.

## Account Management Settings

Thinger.io private instances offer the ability to create user accounts, where each user profile can control their own devices and functionality according to a specific setting entered in the "accounts" section of the "settings" menu. Instructions on how to create and manage user accounts can be found at this link. Within this section we will find two sets of properties to manage:

### User registration settings

This section manages all the parameters related to the registration of new users in an instance. By default this configuration is set in private mode, that is, no public user will be able to register without the express authorization of the instance administrator, who will have to enter the email in the system manually, however, it is possible to enable the public signup and configure exclusion criteria through the "Registration" section as shown in the following image

![](<../../.gitbook/assets/image (371).png>)

The following parameters can be configured

* **Admin Emails**: In this parameter, the instance administrator's email is entered by default; however, it is possible to add other addresses to perform user network administration functions, branding and or the operation of the IoT server.&#x20;
* **Invalid Email Domains**: Allows us to indicate the email providers that will be rejected, that way we can limit the access of certain users to a private IoT instance.&#x20;
* **Required Email Domains:** Allows to indicate one or several providers that will be authorized to create a user account in the private instance rejecting all the others.&#x20;
* **Invalid Usernames**: Allows to lock or reserve specific usernames on a private server instance.&#x20;
* Min Password Lenght: Is useful to force the user to keep safe passwords&#x20;
* **Public signup**: This button allows you to enable public user access to a private network provided you have enough user licenses in your Thinger.io subscription&#x20;
* **Require email notification**: Enables explicit confirmation of the email account&#x20;
* **Recaptcha**: Activates the use of a security system against robot access to the system Recaptcha&#x20;
* **Secret Key:** Allows to change the security key of the recaptcha system

### User accounts feature limitations

Para permitir el control de las funcionalidades quedespliegan los usuarios de una instancia privada, el apartado "Limits" de "User Account Settings" ofrece una interfaz gráfica en la que cada privilegio puede ser configurado de forma independiente. Los valores introducidos en este menú se aplicarán a todos los usuarios por igual pero no restringirán las capacidades de la cuenta de administración de la instancia

![](<../../.gitbook/assets/image (564).png>)

The amount of the following features can be configured using this interface:

* Access Tokens
* Data Buckets
* Dashboards
* Devices
* Endpoints
* Plugins
* Projects
* Project members
* File Storages&#x20;

## Deployment Settings

This section allows to custom the contact email to be used on the contact button of the patform's main menu

## Restart Server

Thinger.io server needs to be reset after modifying any of these parameters in order to be configured on its files. The restart section allows executing the process in a simple way.&#x20;

![](<../../.gitbook/assets/image (420).png>)
