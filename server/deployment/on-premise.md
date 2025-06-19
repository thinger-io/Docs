# On-Premise

## Subscribing and Deploying On-premise Instances

Thinger.io IoT instances can be deployed on-premise or on any kind of cloud or local host, providing users with full control over the entire infrastructure. This license is particularly well-suited for enterprises that need to host their own data. This section outlines the process of obtaining an on-premise license and deploying a private Thinger.io on-premise instance within minutes.

### 1. Select the right license

On-premise instances can be deployed with different licenses, depending on the project requirements, mainly in terms of platform features like rebrands, custom domains, additional support, plugins, etc. License codes can be purchased [here](https://thinger.io/pricing).\


| Features                  | MEDIUM                             | LARGE                                | PERPETUAL                         |
| ------------------------- | ---------------------------------- | ------------------------------------ | --------------------------------- |
| **Devices**               | 1000                               | 2500                                 | Unlimited                         |
| **Plugins**               | 3                                  | 5                                    | 5                                 |
| **Multi-tenant**          | Up to 5                            | Up to 15                             | Single                            |
| **Extended** **Features** | Business                           | Business                             | Business                          |
| **White-labels**          | 1                                  | 5                                    | 1                                 |
| **MQTT Support**          | ✓                                  | ✓                                    | ✓                                 |
| **Guest accounts**        | Unlimited                          | Unlimited                            | Unlimited                         |
| **Support**               | Extended Support Available (Paid)  | Extended Support Available (Paid)    | Extended Support Available (Paid) |
| **Recommended use**       | Business B2B or B2B2C IoT product  | Consultancies with multiple projects | Companies without limits          |

### 2.  Checkout and payment options

***

After payment is processed, an email will be received containing a link to begin the setup and installation process, along with the license token.

### 3.  On-premise install

Once the license token has been received by email, Thinger.io can be easily deployed on the host with a few commands. Before starting this guide, please install [Docker Engine](https://docs.docker.com/install/) and [Docker Compose](https://docs.docker.com/compose/install/) on the computer or server.&#x20;

{% hint style="success" %}
Install [Docker Engine](https://docs.docker.com/install/) and [Docker Compose ](https://docs.docker.com/compose/install/)before following this guide.
{% endhint %}

This guide assumes Thinger.io is being installed on a fresh Linux host with Docker support, as it will run databases like `MongoDB`, and will start listening on several ports: `80`, `443`, `1883`,`8883`, `25200`, `25202`, `25204` and `25206` . It will also create a root directory in `/data` where all the Thinger.io data and database information will be stored.

To start, just launch this command that will download the `docker-compose` file associated with the license:

```bash
curl https://subscriptions.thinger.io/v1/docker-compose.yml?token={LICENSE} -o docker-compose.yml
```

{% hint style="warning" %}
Never share or publish the LICENSE key as it may pose a security risk for the host. License keys are issued per host, so do not reuse them between hosts. &#x20;
{% endhint %}

Ensure that the `docker-compose` file has been downloaded correctly:

```bash
cat docker-compose.yml
```

It should display:

{% code title="docker-compose.yml" %}
```yaml
networks:
  backend:
    name: backend

services:

  # Thinger.io server
  thinger:
    image: thinger/server:alpha
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
      - TOKEN={{TOKEN}}
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

  # mongodb
  mongodb:
    image: mongo:8.0
    container_name: "mongodb"
    environment:
      - MONGO_DATA_DIR=/data/db
      - MONGO_LOG_DIR=/dev/null
      - MONGO_INITDB_ROOT_USERNAME=thinger
      - MONGO_INITDB_ROOT_PASSWORD={{SALT}}
      - MONGO_INITDB_DATABASE=thinger
    volumes:
      - /data/mongodb:/data/db
    networks:
      - backend
    ports:
      - 127.0.0.1:27017:27017
    command: mongod --maxConns 10000 --quiet
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

Then, if everything seems to be correct, just run this command to start all the processes defined in `docker-compose.yml` and run them in detached mode with `-d` option:

```yaml
docker-compose up -d
```

If everything goes fine, it should show something like this information (it may take several minutes to complete depending on the network connection):

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
Pulling mongodb (mongo:8)...
8: Pulling from library/mongo
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
Status: Downloaded newer image for mongo:8
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
Creating thinger   ... done
```

Then, the Thinger.io instance and the associated databases will be running:

```bash
root@docker-s-1vcpu-1gb-fra1-01:~# docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS                    PORTS                                                                          NAMES
c7075dd45e5d        mongo:8                 "docker-entrypoint.s…"   43 minutes ago      Up 43 minutes             127.0.0.1:27017->27017/tcp                                                     mongodb
8ee8b4c924dd        thinger/server:latest   "thinger -v0 --runpa…"   47 minutes ago      Up 43 minutes (healthy)                                                                                  thinger
eee1b9479368        pyouroboros/ouroboros   "ouroboros"              47 minutes ago      Up 47 minutes                                                                                            ouroboros
```

Then, the on-premise instance can be accessed by pointing a browser to the host's IP address.

{% hint style="info" %}
The latest versions of Ubuntu come with `UFW` (The default firewall configuration tool for Ubuntu). It may be blocking Thinger.io ports by default. Configure it properly or disable it (not recommended)  with `sudo ufw disable`
{% endhint %}

## Steps After On-premise Deployment

To start working with the on-premise installation, just follow the next steps:

### First Login

1. Access the server by writing the local IP address of the host, for example: [https://1](https://acme.do.thinger.io)92.168.1.100. This step should show the Thinger.io login screen after accepting to use a self-signed certificate (The browser will prompt a security issue regarding the certificate).
2. Note that this server has never been accessed before, and it is a completely isolated instance, so no user account has been created. Then, it is necessary to click on `Create an account`button, and fill the form to create a new user profile using the `Admin Email` address provided in the license configuration (any other address will not be authorized to sign up).
3. After creating the new account, it is possible to access the new server. It is not necessary to confirm the email address.

### Device Connection

When working with a private Thinger.io instance, it is necessary to point devices to the newly created server. If the [Arduino](../../arduino/) or [Linux](../../linux.md) client libraries are being used (e.g., for Arduino, ESP8266, ESP32, Raspberry Pi, etc.), a definition should be added at the top of the code to point to the host. The sketch should be modified as follows:

```
#define THINGER_SERVER "192.168.1.100"

// the rest of the code goes here
```

{% hint style="info" %}
If this host definition is not provided, the devices will try to connect with the public instance.&#x20;
{% endhint %}
