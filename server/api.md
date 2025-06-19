---
description: >-
  This section describes the basic messages that provide Thinger.io Server API
  to interact with its backend functionalities.
---

# SERVER API

## Thinger.io API

{% hint style="success" %}
[New API documentation is **available on Swagger!** Can be checked out at _this link_](https://console.thinger.io/swagger)
{% endhint %}

All the examples described in this documentation define URL endpoints based on a relative path, assuming the host is just the server IP, domain, or the default Thinger.io server. For all calls issued over the default Thinger.io cloud, the host address will be:

```
https://api.thinger.io
```

_Notice_ that if the Thinger.io server is being run on a private host or domain, a secure HTTPS request may fail without the configuration of an appropriate **SSL certificate**. While non-secure HTTP can be used for calls, it is not recommended in production environments.

## Authentication API

### REST API Authentication

All queries made to the API Rest interface must be signed in order to access the user resources. So, all requests must include an `Authorization` header that includes the access token to the account:

```
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODYwNDkxNTcsImlhdCI6MTQ4NjA0MTk1NywidXNyIjoianQifQ.pkyG43xiEhDtUHLxuycYv156FGuvNh6nDKQ07kGcaGk
```

The access token is a JWT Token that needs to be obtained from the user credentials, from a refresh token, or just from a user-defined access token. So there are two different concepts:

#### Access Token

Is the token used for granting access to API requests? It should be included in the HTTP `Authorization` header along with the keyword `Bearer`. It can also be included as a URL parameter in the HTTP request, with the key `authorization` (case-sensitive), and the token as the value.

_Notice_ that when this token is obtained from user credentials or a refresh token, it has a validity of **2 hours**. So it must be refreshed periodically in order to have a valid access token.

#### Refresh Token

The refresh token is a token that cannot provide access to the user resources, but can be used for getting a fresh access token in case it has expired.

This token, obtained with the user credentials or with a valid refresh token, has a validity of around **2 months** from the issue date and will be refreshed every time it is used to obtain a new pair of access and refresh tokens.

The idea behind the authentication is that user credentials are required to obtain both an access token and a refresh token. The refresh token can be kept in a secure place, and the access token can then be used to access user resources. Once the access token has expired. Then, use the refresh token to get a new access token and a new refresh token.

This way, if the account is used periodically, the user credentials are not required for the authentication process. If the access token is leaked in some way, then the attacker would have a short time span of access. If the refresh token is also leaked, it can be revoked manually to avoid its use for getting new tokens.

#### User Token

The tokens defined by the user in their account can be used just like any other access token to authenticate the request. However, contrary to the tokens obtained from the user credentials, this token does not expire by default, and the user can define the access level over the account resources. So, in this way, the user could define a token for accessing a single device or for writing to a data bucket without compromising other account resources.

This kind of token can be defined directly from the Thinger.io Console in the `Access Tokens` section.&#x20;

To add permissions to the token for writing to a single bucket, configuration can be performed to grant access to various resources and actions within the account:

![](../.gitbook/assets/token_permission.png)

### Getting Tokens From User Credentials

This method allows getting both the access token and refresh token from the user credentials. It should be used the first time the user logs into the application.

*   **URL**

    ```
      /oauth/token
    ```
*   **Method:**

    ```
      POST
    ```
*   **Content Type**

    ```
    Content-Type:application/x-www-form-urlencoded
    ```
*   **Body**

    ```
      grant_type=password&username=username&password=password
    ```
* **Success Response:**
  * **Code:** 200
  *   **Content:**

      ```javascript
      {  
         "access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODYwNDgzNzcsImlhdCI6MTQ4NjA0MTE3NywidXNyIjoianQifQ.A-Vh715P6GjFDBkbh6TmNGxiHWl0KjbDq8tM4qsmTaI",
         "expires_in":7200,
         "refresh_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0OTEzMTE1NzcsImlhdCI6MTQ4NjA0MTE3NywianRpIjoiNTg5MzMwNTkzOWNiZWY0YWEzMDE1YWJiIn0.5Voenem4D90zPNqiS1oVBfguDzygwNzgmcmEi-4N-8Q",
         "scope":null,
         "token_type":"bearer"
      }
      ```
* **Error Response:**
  * **Code:** 401 Unauthorized
  *   **Content:**&#x20;

      ```javascript
       {  
          "error":{  
             "message":"invalid username or password"
          }
       }
      ```

### Getting Tokens With Refresh Token

This method allows getting a fresh access token and refresh token from a valid refresh token. It should be called every time the access token has expired or the refresh token is likely to expire.

*   **URL**

    ```
      /oauth/token
    ```
*   **Method:**

    ```
      POST
    ```
*   **Content Type**

    ```
    Content-Type:application/x-www-form-urlencoded
    ```
*   **Body**

    ```
      grant_type=refresh_token&refresh_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0OTEzMTIzNTcsImlhdCI6MTQ4NjA0MTk1NywianRpIjoiNTg5MzMzNjUzOWNiZWY0YWEzMDE1YWJjIn0.BYwRii9eL7jeQQQqMbuBEZAvwmmVRAU8kWYCNZEDn0s
    ```
* **Success Response:**
  * **Code:** 200
  *   **Content:**&#x20;

      ```javascript
      {  
         "access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODYwNTY0MjYsImlhdCI6MTQ4NjA0OTIyNiwidXNyIjoianQifQ.H7G4N3MMHxUO2gPHzG0a9N1lZ5--Gt56CC4HOiFMKLE",
         "expires_in":7200,
         "refresh_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0OTEzMTk2MjYsImlhdCI6MTQ4NjA0OTIyNiwianRpIjoiNTg5MzMzNjUzOWNiZWY0YWEzMDE1YWJjIn0.dqxbZegv4oemeDK6czDzQLRfA3da6NShBcseNLTn33Q",
         "scope":null,
         "token_type":"bearer"
      }
      ```
* **Error Response:**
  * **Code:** 401 Unauthorized
  *   **Content:**&#x20;

      ```javascript
       {  
          "error":{  
             "message":"invalid refresh token"
          }
       }
      ```

### Detecting Access Token Expire

The access token expires in around 2 hours from its issue date. There are two ways to determine if the access token has expired in order to request a new one.

#### Checking the JWT contents

The first way of checking if an access token is expired is by parsing the JWT and decoding the payload data. An access token will have a payload:

```javascript
{
  "exp": 1486048377,
  "iat": 1486041177,
  "usr": "alvarolb"
}
```

The `exp` field is the Unix timestamp in seconds (UTC) when the token will expire. If the request is after this timestamp, then the request will fail.

#### Checking server response

It is possible to check the validity of an access token simply by trying to access any user resource. If the access token is expired, then the authentication will fail, and the API Request query will return a `401 Unautorized`.

