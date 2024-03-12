# On-Premise

## Subscribing and Deploying On-premise Instances

Thinger.io IoT instances can be deployed on-premise or on any kind of cloud or local host, providing users with full control over the entire infrastructure. This license is particularly well-suited for enterprises that need to host their own data. This section outlines the process of obtaining an on-premise license and deploying a private Thinger.io on-premise instance within minutes.

### 1. Select the right license

On-premise instances can be deployed with different licenses, depending on the project requirements, mainly in terms of platform features like rebrands, custom domains, additional support, plugins, etc. License codes can be purchased [here](https://pricing.thinger.io).\


|                      | MEDIUM                             | LARGE                                | PERPETUAL                         |
| -------------------- | ---------------------------------- | ------------------------------------ | --------------------------------- |
| **Devices**          | 1000                               | 2500                                 | Unlimited                         |
| **Plugins**          | 3                                  | 5                                    | 5                                 |
| **Multi-tenant**     | Up to 5                            | Up to 15                             | Single                            |
| **Features**         | Business                           | Business                             | Business                          |
| **White-labels**     | 1                                  | 5                                    | 1                                 |
| **MQTT Support**     | ✓                                  | ✓                                    | ✓                                 |
| **Guest accounts**   | Unlimited                          | Unlimited                            | Unlimited                         |
| **Support**          | Extended Support Available (Paid)  | Extended Support Available (Paid)    | Extended Support Available (Paid) |
| **Recommended use**  | Business B2B or B2B2C IoT product  | Consultancies with multiple projects | Companies without limits          |

### 2.  Checkout and payment options

After payment is processed, you will receive an email with the link to set up begin the installation process and your license token.

### 3.  On-premise install

Once you have received the license token by email, it is possible to easily deploy Thinger.io on your host with a few commands. Before starting this guide, please, install [Docker Engine](https://docs.docker.com/install/) and [Docker Compose](https://docs.docker.com/compose/install/) in your computer or server.&#x20;

{% hint style="success" %}
Install [Docker Engine](https://docs.docker.com/install/) and [Docker Compose ](https://docs.docker.com/compose/install/)before following this guide.
{% endhint %}

This guide assumes you are installing Thinger.io on a fresh Linux host with Docker support, as it will run databases like `MongoDB` and `InfluxDB`, and will start listening on several ports: `80`, `443`, `1883`,`8883`, `25200`, `25202`, `25204` and `25206` . It will also create a root directory in `/data` where all the Thinger.io data and database information will be stored.

To start, just launch the following command that will download the `docker-compose` file associated to your license:

```bash
curl https://subscriptions.thinger.io/v1/docker-compose.yml?token={LICENSE} -o docker-compose.yml
```

{% hint style="warning" %}
Never share or publish your LICENSE key as it may consist of a security risk for your host. License keys are issued per host, so do not reuse them between hosts. &#x20;
{% endhint %}

Ensure that your `docker-compose` file has been downloaded correctly:

```bash
cat docker-compose.yml
```

It should display something like the following:

{% code title="docker-compose.yml" %}
```yaml
version: '3.7'
services:

  # Thinger.io server
  thinger:
    image: thinger/server:latest
    container_name: thinger
    user: root
    volumes:
      # for controlling docker engine
      - /var/run/docker.sock:/var/run/docker.sock
      # folder for storing thinger generated data (maxmind, certificates, plugins...)
      - /data/thinger:/data
    entrypoint:
      - thinger
      - -v0
      - --runpath=/data
    environment:
      - TOKEN=ce84fc9f089227a4828f66c470d9319533611ee37adc41676ba1ef5a92bd1dca0f2258962d19627010525e437c2cbf48a
      - HOST_VOLUME=/data/thinger
    network_mode: host
    restart: always
    logging:
      driver: "json-file"
      options:
        max-size: "200k"
        max-file: "10"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/v1/server/healthcheck"]
      interval: 1m
      timeout: 5s
      retries: 3
      start_period: 2m
    depends_on:
      - mongodb
      - influxdb

  # mongodb
  mongodb:
    image: mongo:4
    container_name: "mongodb"
    environment:
      - MONGO_DATA_DIR=/data/db
      - MONGO_LOG_DIR=/dev/null
      - MONGO_INITDB_ROOT_USERNAME=thinger
      - MONGO_INITDB_ROOT_PASSWORD=PiIcgpUcl0r5xUb/cXP8WS5G
      - MONGO_INITDB_DATABASE=thinger
    volumes:
      - /data/mongodb:/data/db
    ports:
      - 127.0.0.1:27017:27017
    command: mongod --quiet
    restart: always
    logging:
      driver: "json-file"
      options:
        max-size: "200k"
        max-file: "10"

  # influxdb
  influxdb:
    image: influxdb:2.4
    container_name: influxdb2
    ports:
      # The API for InfluxDB is served on port 8086
      - 8086:8086
    volumes:
      - /data/influxdb2:/var/lib/influxdb2
    environment:
      - DOCKER_INFLUXDB_INIT_MODE=setup
      - DOCKER_INFLUXDB_INIT_USERNAME=thinger
      - DOCKER_INFLUXDB_INIT_PASSWORD=PiIcgpUcl0r5xUb/cXP8WS5G
      - DOCKER_INFLUXDB_INIT_ORG=thinger
      - DOCKER_INFLUXDB_INIT_BUCKET=thinger
      - DOCKER_INFLUXDB_INIT_ADMIN_TOKEN=ce84fc9f089227a4828f66c470d9319533611ee37adc41676ba1ef5a92bd1dca0f2258962d19627010525e437c2cbf48a
      - INFLUXDB_REPORTING_DISABLED=true
    restart: always
    logging:
      driver: "json-file"
      options:
        max-size: "200k"
        max-file: "10"

  # ouroboros
  ouroboros:
    container_name: ouroboros
    hostname: ouroboros
    image: pyouroboros/ouroboros
    environment:
      - CLEANUP=true
      - INTERVAL=600
      - LOG_LEVEL=info
      - SELF_UPDATE=true
      - MONITOR=thinger
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    logging:
      driver: "json-file"
      options:
        max-size: "200k"
        max-file: "10"
    depends_on:
      - thinger
```
{% endcode %}

Then, if everything seems to be correct, just run the following command to start all the processes defined in `docker-compose.yml` and run them in detached mode with `-d` option:

```yaml
docker-compose up -d
```

If everything goes fine, it should show something like the following information (it may take several minutes to complete depending on your network connection):

```bash
root@docker-s-1vcpu-1gb-fra1-01:~# docker-compose up -d
Creating network "root_default" with the default driver
Pulling thinger (thinger/server:latest)...
latest: Pulling from thinger/server
da6fc00e4d0b: Pull complete
c3c0be9d84b3: Pull complete
9c1dda927878: Pull complete
4b8880231fa0: Pull complete
ec7cf4588dfa: Pull complete
f03c87626902: Pull complete
2c927b4e662e: Pull complete
0c0ed4ba2578: Pull complete
db577de2586f: Pull complete
Digest: sha256:156bb95f155ce9d3706c6a2f17d2a6750cf62e91777a610625910c7ebf780894
Status: Downloaded newer image for thinger/server:latest
Pulling mongodb (mongo:4)...
4: Pulling from library/mongo
2746a4a261c9: Pull complete
4c1d20cdee96: Pull complete
0d3160e1d0de: Pull complete
c8e37668deea: Pull complete
fc3987a82b4c: Pull complete
c75f139e0836: Pull complete
4acc9c8680b4: Pull complete
fb02df30d947: Pull complete
ae725ef3d2ce: Pull complete
e30f54ed6b43: Pull complete
bca9e535ddb8: Pull complete
9c3edad81b2a: Pull complete
6dbcf78fe5ae: Pull complete
Digest: sha256:7a1406bfc05547b33a3b7b112eda6346f42ea93ee06b74d30c4c47dfeca0d5f2
Status: Downloaded newer image for mongo:4
Pulling influxdb (influxdb:1.7)...
1.7: Pulling from library/influxdb
146bd6a88618: Pull complete
9935d0c62ace: Pull complete
db0efb86e806: Pull complete
5dd32e36b488: Pull complete
750868d0ab2b: Pull complete
f4d98645d729: Pull complete
c8bd5f153b8d: Pull complete
f458001f5cb1: Pull complete
Digest: sha256:eae897c8ebf85ac3e2bdff8ba053d40a3df7598c41f4b63d42faf2603e2eef74
Status: Downloaded newer image for influxdb:1.7
Pulling ouroboros (pyouroboros/ouroboros:)...
latest: Pulling from pyouroboros/ouroboros
8e402f1a9c57: Pull complete
cda9ba2397ef: Pull complete
d7153c29df0e: Pull complete
cbbf0d0a5ee3: Pull complete
c142a4eca653: Pull complete
829de03e02e7: Pull complete
499113b78598: Pull complete
7a2dcb0f00c1: Pull complete
2cd0c4b889bd: Pull complete
01c721e4643e: Pull complete
Digest: sha256:cfa29916459fb8c578fce084ce839a0d3bee478b83a21b6b1d10c6b78bc4a372
Status: Downloaded newer image for pyouroboros/ouroboros:latest
Creating mongodb   ... done
Creating ouroboros ... done
Creating influxdb  ... done
Creating thinger   ... done
```

Then, the Thinger.io instance and the associated databases will be running:

```bash
root@docker-s-1vcpu-1gb-fra1-01:~# docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS                    PORTS                                                                          NAMES
c7075dd45e5d        mongo:4                 "docker-entrypoint.s…"   43 minutes ago      Up 43 minutes             127.0.0.1:27017->27017/tcp                                                     mongodb
ff9d6178773b        influxdb:2.4            "/entrypoint.sh infl…"   43 minutes ago      Up 43 minutes             127.0.0.1:8082->8082/tcp, 127.0.0.1:8086->8086/tcp, 127.0.0.1:8089->8089/udp   influxdb
8ee8b4c924dd        thinger/server:latest   "thinger -v1 --runpa…"   47 minutes ago      Up 43 minutes (healthy)                                                                                  thinger
eee1b9479368        pyouroboros/ouroboros   "ouroboros"              47 minutes ago      Up 47 minutes                                                                                            ouroboros
```

Then, you can access your on-premise instance by pointing your browser to your host IP address.

{% hint style="info" %}
The latest versions of Ubuntu come with `UFW` (the default firewall configuration tool for Ubuntu). It may be blocking Thinger.io ports by default. Configure it properly or disable it (not recommended)  with `sudo ufw disable`
{% endhint %}

## Steps After On-premise Deployment

To start working with your on-premise installation, just follow the next steps:

### First Login

1. Access the server by writing the local IP address of your host, for example: [https://1](https://acme.do.thinger.io)92.168.1.100. This step should show the Thinger.io login screen after accepting to use a self-signed certificate (the browser will ask you about a security issue with the certificate).
2. Note that this server has never been accessed before, and it is a completely isolated instance so there is not any user account created. Then, it is necessary to click on `Create an account`button, and fill the form to create a new user profile using the `Admin Email` address provided in the license configuration (any other address will not be authorized to sign up).
3. After creating the new account it is possible to access the new server. It is not necessary to confirm the mail address.

### Device Connection

When working with a private Thinger.io instance, it is necessary to point your devices to the newly created server. If you are using the [Arduino](../../arduino/) or [Linux](../../linux.md) client libraries, i.e., for Arduino, ESP8266, ESP32, Raspberry Pi, etc., you should add a definition on top of your code to point to your host. So, modify your sketch like this:

```
#define THINGER_SERVER "192.168.1.100"

// the rest of your code goes here
```

{% hint style="info" %}
If this host definition is not provided, your devices will try to connect with the public instance.&#x20;
{% endhint %}
