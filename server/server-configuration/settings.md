# Server Settings

<figure><img src="../../.gitbook/assets/image (729).png" alt=""><figcaption></figcaption></figure>

To provide managers of private Thinger.io instances with full service configuration capabilities, the web console offers a host management tool. This tool features various sections that allow for simply setting operational criteria. It is accessible in the "**Cluster Hosts**" tab of the main menu, which will display the Host List:

<figure><img src="../../.gitbook/assets/image (730).png" alt=""><figcaption></figcaption></figure>

After selecting a server profile, in the top-right corner of the server status dashboard, it is possible to select the option "Settings", in which the next configuration options will be displayed:

## IoT Server Settings

This platform allows the connection of various device types, for which it is equipped with different data ingestion engines. Each section explained below enables configuring the IP addresses and ports through which each device type is connected.

### HTTP Server configuration

First, enter the host in the host list. Then, click **'Settings'** to access its configuration. To set the connection parameters of the [**HTTP devices integration**](../../http-devices.md) module on Thinger.io private instances:

<figure><img src="../../.gitbook/assets/image (733).png" alt=""><figcaption></figcaption></figure>

The configurable parameters are:&#x20;

* **Enabled:** Allows to disable the functionality of this ingest module.
* **Listen Address:** Allows to indicate the fixed IP address of the server, leaving it at 0.0.0.0 will use the default IP of the host.
* **TCP Port:** To configure the standard HTTP communications port with a customized value.
* **TLS Port:** To configure the standard TLS request collection port with a customized value.

### IOMTP Server configuration

It allows setting up the parameters for connecting Thinger.io to devices running the Thinger.io software client.

<figure><img src="../../.gitbook/assets/image (732).png" alt=""><figcaption></figcaption></figure>

The configurable parameters are:&#x20;

* **Enabled:** Allows to disable the functionality of this ingest module.&#x20;
* **Listen Address:** Allows to indicate the fixed IP address of the server, leaving it at 0.0.0.0 will use the default IP of the host.
* **TCP Port:** To configure the standard TCP communications port with a customized value.
* **TLS Port:** To configure the standard TLS request connection port with a customized value.

### MQTT Server configuration

This section allows setting the connection and data entry parameters of [**MQTT devices**](../../mqtt.md) with Thinger.io's built-in MQTT broker.

<figure><img src="../../.gitbook/assets/image (734).png" alt=""><figcaption></figcaption></figure>

The configurable parameters are:&#x20;

* **Enabled:** Allows to disable the functionality of this ingest module.
* **Listen Address:** Allows to indicate the fixed IP address of the server, leaving it at 0.0.0.0 will use the default IP of the host.
* **TCP Port:** To configure the standard MQTT communications port with a customized value.
* **TLS Port:** To configure the standard MQTT TLS connection request port with a customized value.

## Proxies

This area is crucial for managing how the Thinger.io instance handles proxy connections, which are often used to establish secure and direct communication with devices, especially those located behind firewalls or Network Address Translators (NATs).

<figure><img src="../../.gitbook/assets/image (739).png" alt=""><figcaption></figcaption></figure>

Specifically, this screen allows configuring the **'Dynamic Ports Range'**. This feature defines a pool of ports that the server can dynamically assign to individual proxy connections. This is particularly useful for scenarios where devices establish outgoing connections (e.g., tunnels) that require a unique and temporary port for each session. The **'Port Start'** field sets the lowest port number in this range, while the **'Port End'** field defines the highest, thereby establishing the full spectrum of available ports for dynamic assignment.

## Email Settings

This section allows configuring the mail server in charge of sending notifications to the users of the IoT instance. A standard SMTP server can be used, but if the client has an AWS SES (Simple Email Service), which is more scalable and easier to use, it can be configured here to be used instead.

<figure><img src="../../.gitbook/assets/image (740).png" alt=""><figcaption></figcaption></figure>

In both cases, domain and sender parameters can be configured from the main section. Then, depending on the selected service, some specific parameters need to be specified:

### SMTP configuration

The **Simple Mail Transfer Protocol**, better known as SMTP, is a protocol used to transmit email messages over the Internet. Thinger.io server instances contain an SMTP server that allows sending notifications to the instance users, which has been configured by default to use the same web domain as the IoT server host and the standard parameters, however,  these parameters can be customized by changing the server:

<figure><img src="../../.gitbook/assets/image (741).png" alt=""><figcaption></figcaption></figure>

* **Host:** Is the SMTP address or web domain.
* **Port:** Custom port to be used in order to send the notifications.
* **Username**: SMTP username credentials.&#x20;
* **Password:** SMTP username password.
* **SSL/TLS** **Connection:** should be enabled if the SMTP has been installed on a different host, but can be disabled if it is running on the same one.

### AWS SES configuration

The integration with Amazon SES provides a much simpler and scalable mailing tool. It can be selected instead of the common SMTP by selecting it on Email Type, and placing the credentials in its appropriate section. These credentials can be obtained on the AWS SES configuration section, as explained on[ **this link**](https://ongage.atlassian.net/wiki/spaces/HELP/pages/13795743/Amazon+SES+Setup+Tutorial)**.**

<figure><img src="../../.gitbook/assets/image (742).png" alt=""><figcaption></figcaption></figure>

## Buckets Database Settings

The Thinger.io Data buckets system is very flexible, it can be configured to use the user's preferred system for the different storage and export processes supported by the platform&#x20;

### Storage Engine Settings

Thinger.io's data bucket storage engine can be configured to use different database systems such as MongoDB, DynamoDB, or InfluxDB (used by default in private instances). The "Buckets" section of "Host Settings" allows selecting the desired system via the "General" section and connect it to a database server of the same type, both for storage and export of the bucket data to files:



#### Using InfluxDB to store Buckets data

The default configuration of Thinger.io's storage engine uses InfluxDB system, as it provides some interesting features as data aggregation or using data tags to classify data over specific criteria. To set a different configuration of InfluxDB, the next four parameters need to be introduced:

<figure><img src="../../.gitbook/assets/image (745).png" alt=""><figcaption></figcaption></figure>

* **InfluxDB Host:** Place here the web domain or IP of the InfluxDB server
* **InfluxDB Port:** Place here the standard or custom InfluxDB port number
* **InfluxDB Username:** Is the username used to create the InfluxDB database
* **InfluxDB Password:** An authorization token obtained from the InfluxDB configuration console&#x20;

To obtain this information about an InfluxDB server, it is required to go to the InfluxDB configuration tool, as it is explained on: &#x20;

#### Using DynamoDB to store Buckets data

DynamoDB is a highly scalable and consistent storage system provided by AWS that performs really well on IoT projects. It is possible to configure a DynamoDB system to be used by the Thinger.io server to store data by selecting DynamoDB on the General section and introducing the following data into the system:

<figure><img src="../../.gitbook/assets/image (746).png" alt=""><figcaption></figcaption></figure>

* **Table Name:** It is the identification of the DynamoDB table.
* **AWS Access Key ID:** This is the unique identifier for the AWS account, used to authenticate programmatic requests to AWS services. It acts as the public part of the AWS security credentials.
* **AWS Secret Access Key:** This is the confidential component of the AWS security credentials. It is used in conjunction with the AWS Access Key ID to cryptographically sign API requests to AWS, verifying the identity and protecting the account from unauthorized access. This key should be kept secure and never shared publicly.
* **Region:**  Enter here the AWS region in which the database is placed&#x20;

DynamoDB database configuration parameters can be obtained from the server as explained on the AWS documentation page.

#### Using MongoDB to store Buckets data

MongoDB is one of the most widespread database systems due to its robust operation and because it is an open-source project that offers free storage services for developers (companies that wish to make premium use of MongoDB can use the Atlas version). This database system can be used to store Thigner.io bucket data by entering a URI and some credentials:

<figure><img src="../../.gitbook/assets/image (744).png" alt=""><figcaption></figcaption></figure>

* **MongoDB URI:** Paste here the uniform resource identification obtained from the MongoDB configuration console
* **MongoDB Database:** Enter here the MongoDB database identifier
* **MongoDB Table:** Enter here the MongoDB table name in which the data will be stored

The server URI can be obtained from the "connect" context of the MongoDB or Atlas service as explained at this link.

### Bucket Export Settings

When a specific bucket is requested to be exported, the system creates a file to be stored in the instance's database. The export system allows selecting the database system where the resulting files will be stored, in the same MongoDB used to manage Thinger.io's user accounts, in the filesystem of the server instance or using Amazon S3 (a more scalable option and used by default in the public Thinger.io instances).

<figure><img src="../../.gitbook/assets/image (749).png" alt=""><figcaption></figcaption></figure>

When **'Filesystem'** (as shown) is selected, specify an **'Export Path'**, which is the local directory where the bucket data will be saved:

<figure><img src="../../.gitbook/assets/image (751).png" alt="" width="563"><figcaption></figcaption></figure>

To implement the **AWS S3** option, it must be selected on the General section and then configured to include the parameters:

<figure><img src="../../.gitbook/assets/image (750).png" alt="" width="563"><figcaption></figcaption></figure>

* **Bucket ID:** It is the identification of the AWS S3 data bucket&#x20;
* **AWS Access Key ID:**&#x20;
* **AWS Secret Access Key:**
* **Region:**  Enter here the AWS region in which the database is placed&#x20;

DynamoDB database configuration parameters can be obtained from the server as explained on the AWS documentation page.

## SSL Settings

Communications between the devices and Thinger.io are secured using the TLS SSL protocol. The chain introduced by default is an ordered list of the security protocols to be used for the key exchange. Modifying that order, we can balance between a more secure configuration, better performance or compatibility.

<figure><img src="../../.gitbook/assets/image (753).png" alt=""><figcaption></figcaption></figure>

This section allows configuring the Secure Sockets Layer (SSL) settings for the Thinger.io server. This is essential for establishing secure, encrypted communication (HTTPS) between the server, connected devices, and web browsers, ensuring data confidentiality and integrity. Here, define **SSL Server Ciphers** (encryption algorithms), control the use of **Insecure Protocols**, and manage **Client CA Certs** for mutual authentication.

## Account Management Settings

Thinger.io private instances offer the ability to create user accounts, where each user profile can control their own devices and functionality according to a specific setting entered in the "accounts" section of the "settings" menu. Instructions on how to create and manage user accounts can be found at this link. Within this section, we will find two sets of properties to manage:

<figure><img src="../../.gitbook/assets/image (754).png" alt=""><figcaption></figcaption></figure>

### User registration settings

This section manages all the parameters related to the registration of new users in an instance. By default, this configuration is set in private mode, that is, no public user will be able to register without the express authorization of the instance administrator, who will have to enter the email in the system manually. However, it is possible to enable the public signup and configure exclusion criteria through the "Registration" section, where a '**User Account Settings**' is shown as follows:

<figure><img src="../../.gitbook/assets/image (755).png" alt=""><figcaption></figcaption></figure>

The following parameters can be configured

* **Admin Emails**: In this parameter, the instance administrator's email is entered by default; however, it is possible to add other addresses to perform user network administration functions, branding and or the operation of the IoT server.&#x20;
* **Invalid Email Domains**: Allows us to indicate the email providers that will be rejected, so that we can limit the access of certain users to a private IoT instance.&#x20;
* **Required Email Domains:** Allows to indicate one or several providers that will be authorized to create a user account in the private instance, rejecting all the others.&#x20;
* **Invalid Usernames**: Allows locking or reserving specific usernames on a private server instance.&#x20;
* **Min Password Length**: It is useful to force the user to keep safe passwords.
* **Max Password Length**: It is useful to force the user to keep safe passwords.
* **Public signup**: This button enables public user access to a private network, provided sufficient user licenses are available in the Thinger.io subscription.
* **Account Role:** Defines the default role that will be automatically assigned to new user accounts created on the server.
* **Require email notification**: Enables explicit confirmation of the email account.&#x20;
* **Recaptcha**: Activates the use of a security system against robot access to the system Recaptcha.
* **Recaptcha Secret:** Allows changing the security key of the Recaptcha system.

### User accounts feature limitations

Para permitir el control de las funcionalidades quedespliegan los usuarios de una instancia privada, el apartado "Limits" de "User Account Settings" ofrece una interfaz gráfica en la que cada privilegio puede ser configurado de forma independiente. Los valores introducidos en este menú se aplicarán a todos los usuarios por igual pero no restringirán las capacidades de la cuenta de administración de la instancia:

<figure><img src="../../.gitbook/assets/image (756).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (757).png" alt=""><figcaption></figcaption></figure>

The amount of the following features can be configured using this interface:

* Access Tokens
* Data Buckets
* Dashboards
* Devices
* Endpoints
* Plugins
* Projects
* Project members
* Storages
* Alarm Rules
* Alarm Instances
* Syncs
* Assets Types
*   Assets Groups

    &#x20;

**Bucket Retention** defines how long data points are stored within a bucket before automatic deletion. It allows setting specific time limits or unlimited storage, crucial for managing data lifecycle and storage consumption.

## Deployment Settings

This section allows customizing the contact email to be used on the contact button of the platform's main menu:

<figure><img src="../../.gitbook/assets/image (759).png" alt=""><figcaption></figcaption></figure>

## Scripting Engine

This section allows managing the server's built-in scripting capabilities:

<figure><img src="../../.gitbook/assets/image (760).png" alt="" width="563"><figcaption></figcaption></figure>

This section, specifically showing the **'NodeJS'** configuration, provides an **'Engine' toggle**. This toggle enables or disables the execution of NodeJS scripts on the server. When the engine is enabled, it allows for the implementation of custom logic, automation of tasks, and extension of the platform's functionality using JavaScript code.

## Restart Server&#x20;

The Thinger.io server needs to be reset after modifying any of these parameters in order to be configured in its files. The restart section allows executing the process easily:

<figure><img src="../../.gitbook/assets/image (758).png" alt=""><figcaption></figcaption></figure>
