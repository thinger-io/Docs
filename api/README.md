# API HOST

All the examples described in this documentation defines URL endpoints based on an relative path, assuming the host is just your server IP, domain, or just the default Thinger.io server. For all calls issued over the default Thinger.io cloud, the host address will be the following:

```
https://api.thinger.io
```

*Notice* that if you are running the Thinger.io server in your own host or domain, a secure HTTPS request may fail if you do not configure the appropriate SSL certificate. You can also use non-secure HTTP for your calls, but it is not recommended in production environments.

# Authentication API

## REST API Authentication

All queries done to the API Rest interface must be signed in order to access the user resources. So, all request must include an `Authorization` header that includes the access token to the account:

```
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODYwNDkxNTcsImlhdCI6MTQ4NjA0MTk1NywidXNyIjoianQifQ.pkyG43xiEhDtUHLxuycYv156FGuvNh6nDKQ07kGcaGk
```

The access token is a JWT Token that needs to be obtained from the user credentials, from a refresh token, or just a from a user-defined access token. So there are two different concepts:

### Access Token
Is the token used for granting access to API requests. It should be included in the HTTP `Authorization` header along with the keyword `Bearer`. It can be also included as a URL parameter in the HTTP request, with the key `authorization` (case-sensitive), and the token as the value.

*Notice* that when this token is obtained from user credentials, or from a refresh token, it has a validity of **2 hours**. So it must be refreshed periodically in order have a valid access token.

### Refresh Token
The refresh token is a token that cannot provide access to the user resources, but can be used for getting fresh access token in case they expired.

This token, obtained with the user credentials, or with a valid refresh token, has a validity of around **2 months** from issue date, and will be refreshed every time you use it for getting a new pair of access, and refresh tokens.

So, the idea behind the authentication is that you need to use the user credentials for getting both an access token and a refresh token. You can keep the refresh token in a secure place, and use the access token for accessing user resources. Once the access token has been expired, then use the refresh token for getting a new access token and a new refresh token. 

This way, if the account is used periodically, the user credentials are not required for the authentication process. If the access token is leaked in some way, then the attacker would have a short timespan of access. If the refresh token is also leaked, it can be revoked manually to avoid its use for getting new tokens.

### User Token
The tokens defined by the user in their account, can be used just like any other access token to authenticate the request. However, as contrary as the tokens obtained from the user credentials, this token does not expire by default, and the user can define the access level over the account resources. So, in this way, the user could define a token for accessing a single device, or for writing to a data bucket without comprising other account resources.
  
This kind of tokens can be defined directly from the Thinger.io Console in the `Access Tokens` section. The following picture illustrates how to add permissions to the token for writing to a single bucket, but you can configure them for granting access to different resources and actions of your account.

<p align="center">
<img src="./assets/token_permission.png" width="400px" style="border: 1px solid #000000;"> 
</p>

## Getting Tokens From User Credentials

This method allows getting both the access token and refresh token from the user credentials. It should be used the first time the user logs into the application.

* **URL**

    ``` 
    /oauth/token
    ```

* **Method:**

    ```
    POST
    ```

* **Content Type**

  ```
  Content-Type:application/x-www-form-urlencoded
  ```
  
* **Body**

    ```
    grant_type=password&username=username&password=password
    ```

* **Success Response:**

  * **Code:** 200 <br />
   **Content:** 
   
   ```json
    {  
       "access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODYwNDgzNzcsImlhdCI6MTQ4NjA0MTE3NywidXNyIjoianQifQ.A-Vh715P6GjFDBkbh6TmNGxiHWl0KjbDq8tM4qsmTaI",
       "expires_in":7200,
       "refresh_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0OTEzMTE1NzcsImlhdCI6MTQ4NjA0MTE3NywianRpIjoiNTg5MzMwNTkzOWNiZWY0YWEzMDE1YWJiIn0.5Voenem4D90zPNqiS1oVBfguDzygwNzgmcmEi-4N-8Q",
       "scope":null,
       "token_type":"bearer"
    }
   ```    
* **Error Response:**

  * **Code:** 401 Unauthorized <br />
    **Content:** 
    ```json
     {  
        "error":{  
           "message":"invalid username or password"
        }
     }
    ```    
    
## Getting Tokens With Refresh Token

This method allows getting fresh access token and refresh token from a valid refresh token. It should be called every time the access token has been expired or the refresh token is likely to expire.

* **URL**

    ``` 
    /oauth/token
    ```

* **Method:**

    ```
    POST
    ```
    
* **Content Type**

  ```
  Content-Type:application/x-www-form-urlencoded
  ```
  
* **Body**

    ```
    grant_type=refresh_token&refresh_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0OTEzMTIzNTcsImlhdCI6MTQ4NjA0MTk1NywianRpIjoiNTg5MzMzNjUzOWNiZWY0YWEzMDE1YWJjIn0.BYwRii9eL7jeQQQqMbuBEZAvwmmVRAU8kWYCNZEDn0s
    ```

* **Success Response:**

  * **Code:** 200 <br />
   **Content:** 
   ```json
    {  
       "access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODYwNTY0MjYsImlhdCI6MTQ4NjA0OTIyNiwidXNyIjoianQifQ.H7G4N3MMHxUO2gPHzG0a9N1lZ5--Gt56CC4HOiFMKLE",
       "expires_in":7200,
       "refresh_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0OTEzMTk2MjYsImlhdCI6MTQ4NjA0OTIyNiwianRpIjoiNTg5MzMzNjUzOWNiZWY0YWEzMDE1YWJjIn0.dqxbZegv4oemeDK6czDzQLRfA3da6NShBcseNLTn33Q",
       "scope":null,
       "token_type":"bearer"
    }
   ```    
* **Error Response:**

  * **Code:** 401 Unauthorized <br />
    **Content:** 
    ```json
     {  
        "error":{  
           "message":"invalid refresh token"
        }
     }
    ```  

## Detecting Access Token Expire

The access token expires in around 2 hours from its issue date. There are two ways to determine if the access token has been expired in order to request a new one.

### Checking the JWT contents

The first way of checking if an access token is expired, is by parsing the JWT and decoding the payload data. An access token will have a payload like the following:

```json
{
  "exp": 1486048377,
  "iat": 1486041177,
  "usr": "alvarolb"
}
```

The `exp` field is the unix timestamp in seconds (UTC) when the token will expire. If your request is after this timestamp, then, the request will fail.

### Checking server response

It is possible to check the validity of an access token simply by trying to access any user resource. If the access token is expired, then the authentication will fail, and the API Request query will return a `401 Unautorized`.

# Devices API

## List User Devices

This method allows getting user devices.

* **URL**

    ```
    /v1/users/{USER_ID}/devices
    ```

* **Method:**

  ```
  GET
  ```
  
    
*  **URL Params**

   **Optional:** Search devices by its identified by adding the following url parameter:
 
   ```
   id=[device id]
   ```

* **Success Response:**

  * **Code:** 200 <br />
   **Content:** Array of devices, with the device identifier, description, and connection status.
   ```json
   [  
      {  
         "device":"nodemcu",
         "description":"NodeMCU With ESP8266",
         "connection":{  
            "active":true,
            "ts":1486047553711
         }
      }
   ]
   ```
    
* **Error Response:**

  * **Code:** 401 Unauthorized <br />
  * **Code:** 400 Bad request if the search device name is not valid.<br />
  * **Code:** 404 Not Found <br />
  

## Add User Device
    
This method allows add a device to an user

* **URL**

    ```
    /v1/users/{USER_ID}/devices
    ```

* **Method:**

  ```
  POST
  ```
  
* **Content Type**

  ```
  Content-Type:application/json;charset=UTF-8
  ```
  
* **Body**
  
   ```json
   {  
      "device_id":"nodemcu",
      "device_description":"NodeMCU With ESP8266",
      "device_credentials":"BN8RbpRKfxhm"
   }
   ```
      
* **Success Response:**

  * **Code:** 200 <br />
        
* **Error Response:**

  * **Code:** 401 Unauthorized. If the auth is not success.<br /> 
  * **Code:** 400 Bad request. If there are missing fields, the device is id is not valid (only [a-zA-Z0-9_]{1,25} is allowed), the device already exists, or the user account is limited.<br /> 
  
 ## Delete User Device
  
  This method allows removing an specific device.
  
  * **URL**
  
    ```
    /v1/users/{USER_ID}/devices/{DEVICE_ID}
    ```
  
  * **Method:**
  
    ```
    DELETE
    ```
  
  * **Success Response:**
  
    * **Code:** 200 <br />
          
  * **Error Response:**
  
    * **Code:** 401 Unauthorized. If the auth is not success.<br />
    * **Code:** 400 Bad request. If the device is connected.<br />
    * **Code:** 404 Not Found. If the device does not exists.<br />
    
## Get Device Stats

This method allows getting information about the device statistics in real-time from its latest connection, like connection timestamp, remote IP address, or transmitted data.

* **URL**

    ```
    /v1/users/{USER_ID}/devices/{DEVICE_ID}/stats
    ```

* **Method:**

  ```
  GET
  ```

* **Success Response:**

  * **Code:** 200 <br />
   **Content:** Information about the device, like its connection state and timestamp, its current IP address, and transmitted data.
   ```json
   {  
      "connected":true,
      "connected_ts":1486040468781,
      "ip_address":"83.52.36.133",
      "rx_bytes":810,
      "tx_bytes":854
   }
   ```
    
* **Error Response:**

  * **Code:** 401 Unauthorized <br />
  * **Code:** 400 Bad request if the search device name is not valid.<br />
  * **Code:** 404 Not Found <br />
  
*  **Note** 

    This method can be used with Server Sent Events (SSE) to get real-time updates about device stats, like tx and rx bytes. If you cannot add the Authorization header to the SSE client, you can add the access token to the url like `?authorization=eyJhbGci...`.

## Get Device Tokens

This method allows getting information about the tokens issued to providing access to a particular device and resource.

* **URL**

    ```
    /v1/users/{USER_ID}/devices/{DEVICE_ID}/tokens
    ```

* **Method:**

    ```
    GET
    ```

* **Success Response:**

  * **Code:** 200 <br />
   **Content:** Array with information for each device token.
   ```json
    [  
       {  
          "id":"57fa7149114f032fe00f9787",
          "name":"Door Access",
          "token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkZXYiOiJwdWVydGEiLCJpYXQiOjE0NzYwMzA3OTMsImp0aSI6IjU3ZmE3MTQ5MTE0ZjAzMmZlMDBlOTc4NyIsInVzciI6Imp0In0.OISg5la0jZbWIYRY5KypzSHLVjVKyFLL3I1hjzYV-_A"
       }
    ]
   ```
    
* **Error Response:**

  * **Code:** 401 Unauthorized <br />
  * **Code:** 404 Not Found <br />

## Add Device Token
    
This method allows creating a device token so it can be used later to access the authorized device and resources. The generated token can also expire automatically at some given time.

Use the generated token in the Authorization header when accessing the device resources, in the same way as an access token. You can pass also the device token as an url parameter, like `?authorization=eyJhbGciOi...`

* **URL**

    ```
    /v1/users/{USER_ID}/devices/{DEVICE_ID}/tokens
    ```

* **Method:**

  ```
  POST
  ```
  
* **Content Type**

  ```
  Content-Type:application/json;charset=UTF-8
  ```
  
* **Body**
  
   ```json
    {
        "token_name": "DoorAccess",
        "token_resources": ["open", "close"],
        "token_expiration": 1487894400000
    }
   ```
   
   **Note:** `token_resources` and `token_expiration` fields are optional. Use this fields only if you need to limit the access to the device resources, or make the token expire at a given time in UTC.
      
* **Success Response:**
  * **Code:** 200 <br />
  **Body:** Information about the created token, like its identifier, name and token itself.
    ```json
    {
        "id": "58938c016f789e15ee15b583",
        "name": "DoorAccess",
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJkZXYiOiJkZXZpY2UyIiwiZXhwIjoxNDg3ODk0NDAwLCJpYXQiOjE0ODYwNjQ2NDEsImp0aSI6IjU4OTM4YzAxNmY3ODllMTVlZTE1YjU4MyIsInJlcyI6WyJvcGVuIiwiY3xvc2UiXSwidXNyIjoiYWx2YXJvbGIifQ.lAYQcdMD92RbYzhCfgvkIb2R15DqcWGmdb07cxgE8bM"
    }
    ```
        
* **Error Response:**

  * **Code:** 401 Unauthorized. If the auth is not success.<br /> 
  * **Code:** 400 Bad request. If there are missing fields.<br /> 
  
  
 ## Delete Device Token
  
  This method allows removing a given device token.
  
  * **URL**
  
    ```
    /v1/users/{USER_ID}/devices/{DEVICE_ID}/tokens/:token_id
    ```
  
  * **Method:**
  
    ```
    DELETE
    ```
  
  * **Success Response:**
  
    * **Code:** 200 <br />
          
  * **Error Response:**
  
    * **Code:** 401 Unauthorized. If the auth is not success.<br />
    * **Code:** 404 Not Found. If the token cannot be found.<br />
  

## Access Device Resources

You can access all your device resources by calling API endpoints according to your account id, your device id, and the resource you define in your own devices. This section describes how to access different types of resources, like output, input, input/output, and callback resources.

### Output Resources

  Device output resources are the way external processes or services can query information to connected devices, like sensed parameters, reading the current device state, or any other value it is required to extract from the device.
  
  This method allows accessing an output resource defined in your device, so you can read the contents provided by each resource in real-time. This way, every API call will execute your output resource to fill the information defined in the resource.
  
  * **URL**
  
    ```
    /v2/users/{USER_ID}/devices/{DEVICE_ID}/{RESOURCE_ID}
    ```
  
  * **Method:**
  
    ```
    GET
    ```
  
  * **Success Response:**
  
    * **Code:** 200
    * **Content-type**: application/json
    * **Body**: A JSON document with the key `out` that stores your resource definition.
          
  * **Error Response:**
  
    * **Code:** 401 Unauthorized. If the auth is not success.<br />
    * **Code:** 404 Not Found. If the device or resource is not available.<br />
    
#### Example 1
  
If your account is `alvarolb`, your device identifier is `nodemcu`, and you have a resource in your device defined like this:

```cpp
thing["location"] >> [](pson& out){
    out["lat"] = gps.getLatitude();
    out["lon"] = gps.getLongitude();
};
```

You can send a HTTP GET request over https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/location to get the latitude and longitude from the device:

```json
{
  "out" : {
    "latitude" : 40.125698,
    "longitude" : -5.466911
  }
}
```
    
#### Example 2
  
If your account is `alvarolb`, your device identifier is `nodemcu`, and you have a resource in your device defined like this:

```cpp
thing["temperature"] >> [](pson& out){
    out = dht.readTemperature();
};
```

You can send a HTTP GET request over https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/temperature to get the current temperature:

```json
{
  "out" : 21.00
}
```

### Input Resources

  Device input resources are the way the connected devices can receive information from the cloud, like actuating over them to activate a motor, switch a relay, move a servo, or setting a different logical configuration.
  
  This method allows pushing data to an input resource defined in your device, so you can send data to your resources in real-time. Therefore, this call will execute your device resource with some information.
  
  * **URL**
  
    ```
    /v2/users/{USER_ID}/devices/{DEVICE_ID}/{RESOURCE_ID}
    ```
  
  * **Method:**
  
    ```
    POST
    ```
    
      
  * **Content Type**
    ```
    Content-Type:application/json;charset=UTF-8
    ```
  
  * **Body**
   ```json
   {  
      "in": <any json value, json document, or json array>
   }
   ```
  
  * **Success Response:**
  
    * **Code:** 200 OK. If your device is connected and the resource was called successfully.
          
  * **Error Response:**
  
    * **Code:** 401 Unauthorized. If the auth is not success.<br />
    * **Code:** 404 Not Found. If the device or resource is not available.<br />
    
#### Example 1
  
If your account is `alvarolb`, your device identifier is `nodemcu`, and you have an input resource in your device defined like this, i.e., for turning on/off a relay:

```cpp
thing["relay"] << [](pson& in){
    digitalWrite(RELAY_PIN, (bool)in);
};
```

You can send a HTTP POST request over https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/relay to change the current state:

```json
{
  "in" : true
}
```

```bash
curl \
  -H "Content-Type: application/json;charset=UTF-8" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Accept: application/json, text/plain, */*" \
  -X POST \
  -d '{"in":true}' \
  https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/relay
```
    
#### Example 2
  
If your account is `alvarolb`, your device identifier is `nodemcu`, and you have an input resource in your device defined like this, i.e., for changing an RGB light color:

```cpp
thing["rgb"] << [](pson& in){
    int r = in["r"];
    int g = in["g"];
    int b = in["b"];
    // use here r, g, and b values to change the light color
};
```

You can send a HTTP POST request over https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/rgb to set the current color:

```json
{
  "in" : {
    "r" : 0,
    "g" : 255,
    "b" : 0
  }
}
```

Curl example:

```bash
curl \
  -H "Content-Type: application/json;charset=UTF-8" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Accept: application/json, text/plain, */*" \
  -X POST \
  -d '{"in":{"r":0,"g":255,"b":0}}' \
  https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/rgb
```

#### Example 3
  
If your account is `alvarolb`, your device identifier is `nodemcu`, and you have an input resource in your device defined like this, i.e., for executing a linux command, changing a text on a screen, etc:

```cpp
thing["command"] << [](pson& in){
    String command = in;
    // work here with the string
};
```

You can send a HTTP POST request over https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/command to set the current color:

```json
{
  "in" : "New customer: 101 today!"
}
```

Curl example:

```bash
curl \
  -H "Content-Type: application/json;charset=UTF-8" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Accept: application/json, text/plain, */*" \
  -X POST \
  -d '{"in":"New customer: 101 today!"}' \
  https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/command
```

### Input/Output Resources

  Device resources can be defined also as input/output resources, that is, they can provide an output based on an input.
  
  
  * **URL**
  
    ```
    /v2/users/{USER_ID}/devices/{DEVICE_ID}/{RESOURCE_ID}
    ```
  
  * **Method:**
  
    ```
    POST
    ```
      
  * **Content Type**
    ```
    Content-Type:application/json;charset=UTF-8
    ```
  
  * **Body**
   ```json
   {  
      "in": <any json value, json document, or json array>
   }
   ```
  
  * **Success Response:**
  
    * **Code:** 200
    * **Content-type**: application/json
    * **Body**: A JSON document with the key `out` that stores your resource output.
          
  * **Error Response:**
  
    * **Code:** 401 Unauthorized. If the auth is not success.<br />
    * **Code:** 404 Not Found. If the device or resource is not available.<br />
    
#### Example 1
  
If your account is `alvarolb`, your device identifier is `nodemcu`, and you have an input/output resource `io` in your device defined like this, i.e., for returning the sum and multiplication:

```cpp
thing["io"] = [](pson& in, pson& out){
    out["sum"] = (long)in["value1"] + (long)in["value2"];
    out["mult"] = (long)in["value1"] * (long)in["value2"];
};
```

You can send a HTTP POST request over https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/io to get the result based on the input:

**Input**

```json
{
  "in" : {
    "value1" : 20,
    "value2" : 10
  }
}
```

**Output**

```json
{
  "out" : {
    "sum" : 30,
    "mult" : 200
  }
}
```

**Curl Example**

```bash
curl \
  -H "Content-Type: application/json;charset=UTF-8" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Accept: application/json, text/plain, */*" \
  -X POST \
  -d '{"value1":20,"value2":10}' \
  https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/io
```

### Resources without parameters

This kind of resources does not provide or requires any information to be executed. They are just like functions defined in the device that can be called as required, for example, to force a device update, reboot the system, or any other action. 

This API method allows calling a resource defined in your device, so you can execute the associated function. 

  * **URL**
  
    ```
    /v2/users/{USER_ID}/devices/{DEVICE_ID}/{RESOURCE_ID}
    ```
  
  * **Method:**
  
    ```
    GET
    ```
  
  * **Success Response:**
  
    * **Code:** 200 OK. If your device is connected and the resource was called successfully.
          
  * **Error Response:**
  
    * **Code:** 401 Unauthorized. If the auth is not success.<br />
    * **Code:** 404 Not Found. If the device or resource is not available.<br />

#### Example 1
  
If your account is `alvarolb`, your device identifier is `nodemcu`, and you have a resource `reset` in your device defined like this, i.e., for rebooting the ESP8266 device.

```cpp
thing["reset"] = [](){
    ESP.reset();
};
```
You can send a HTTP GET request over https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/reset to reboot the device:

```bash
curl \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  https://api.thinger.io/v2/users/alvarolb/devices/nodemcu/reset
```

# User API

Work in progress...

# Endpoints API

Work in progress...

# Dashboards API

Work in progress...

# Buckets API

Work in progress...

