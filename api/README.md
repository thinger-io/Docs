# Authentication

## REST API Authentication

All queries done to the API Rest interface must be signed in order to access the user resources. So, all request must include an `Authorization` header that includes the access token to the account:

```
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODYwNDkxNTcsImlhdCI6MTQ4NjA0MTk1NywidXNyIjoianQifQ.pkyG43xiEhDtUHLxuycYv156FGuvNh6nDKQ07kGcaGk
```

The access token is a JWT Token that needs to be obtained from the user credentials or from a refresh token. So there are two different concepts:

**Access Token**
Is the code used for signing all API requests. It should be included in the HTTP `Authorization` header along with the keyword `Bearer`.

This token, once obtained, has a validity of **2 hours**. So it must be refreshed periodically in order have a valid access token.

**Refresh Token**
The refresh token is a token that cannot provide access to the user resources, but can be used for getting fresh access token in case they expired.

This token has a validity of around **2 months** from issue date, and will be refreshed every time you use it for getting a new access token.

So, the idea behind the authentication is that you need to use the user credentials for getting both an access token and a refresh token. You can keep the refresh token in a secure place, and use the access token for accessing user resources. Once the access token has been expired, then use the refresh token for getting a new access token and a new refresh token. 

This way, if the account is used periodically, the user credentials are not required for the authentication process. If the access token is leaked in some way, then the attacker would have a short timespan of access. If the refresh token is also leaked, it can be revoked manually to avoid its use for getting new tokens.

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
  
* **Content**

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
  
* **Content**

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
    /v1/users/:user_id/devices
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
    /v1/users/:user_id/devices
    ```

* **Method:**

  ```
  POST
  ```
  
* **Content Type**

  ```
  Content-Type:application/json;charset=UTF-8
  ```
  
* **Content**
  
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
    /v1/users/:user_id/devices/:device_id
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
    /v1/users/:user_id/devices/:device_id/stats
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
    /v1/users/:user_id/devices/:device_id/tokens
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
    /v1/users/:user_id/devices/:device_id/tokens
    ```

* **Method:**

  ```
  POST
  ```
  
* **Content Type**

  ```
  Content-Type:application/json;charset=UTF-8
  ```
  
* **Content**
  
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
  **Content:** Information about the created token, like its identifier, name and token itself.
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
    /v1/users/:user_id/devices/:device_id/tokens/:token_id
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
  
 
Work in progress...

# User API

Work in progress...

# Endpoints API

Work in progress...

# Dashboards API

Work in progress...

# Buckets API

Work in progress...

