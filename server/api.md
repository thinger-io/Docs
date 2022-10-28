---
description: >-
  This section describes the basic messages that provide Thinger.io Server API
  to interact with its backend functionalities.
---

# SERVER API

## Thinger.io API

{% hint style="success" %}
****[New API documentation is **available on Swagger!** Can be checked out at _this link_](https://console.thinger.io/swagger/index.html)
{% endhint %}

All the examples described in this documentation defines URL endpoints based on a relative path, assuming the host is just your server IP, domain, or just the default Thinger.io server. For all calls issued over the default Thinger.io cloud, the host address will be the following:

```
https://api.thinger.io
```

_Notice_ that if you are running the Thinger.io server in your own host or domain, a secure HTTPS request may fail if you do not configure the appropriate SSL certificate. You can also use non-secure HTTP for your calls, but it is not recommended in production environments.

## Authentication API

### REST API Authentication

All queries done to the API Rest interface must be signed in order to access the user resources. So, all request must include an `Authorization` header that includes the access token to the account:

```
Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE0ODYwNDkxNTcsImlhdCI6MTQ4NjA0MTk1NywidXNyIjoianQifQ.pkyG43xiEhDtUHLxuycYv156FGuvNh6nDKQ07kGcaGk
```

The access token is a JWT Token that needs to be obtained from the user credentials, from a refresh token, or just a from a user-defined access token. So there are two different concepts:

#### Access Token

Is the token used for granting access to API requests. It should be included in the HTTP `Authorization` header along with the keyword `Bearer`. It can be also included as a URL parameter in the HTTP request, with the key `authorization` (case-sensitive), and the token as the value.

_Notice_ that when this token is obtained from user credentials, or from a refresh token, it has a validity of **2 hours**. So it must be refreshed periodically in order have a valid access token.

#### Refresh Token

The refresh token is a token that cannot provide access to the user resources, but can be used for getting fresh access token in case they expired.

This token, obtained with the user credentials, or with a valid refresh token, has a validity of around **2 months** from issue date, and will be refreshed every time you use it for getting a new pair of access, and refresh tokens.

So, the idea behind the authentication is that you need to use the user credentials for getting both an access token and a refresh token. You can keep the refresh token in a secure place, and use the access token for accessing user resources. Once the access token has been expired, then use the refresh token for getting a new access token and a new refresh token.

This way, if the account is used periodically, the user credentials are not required for the authentication process. If the access token is leaked in some way, then the attacker would have a short timespan of access. If the refresh token is also leaked, it can be revoked manually to avoid its use for getting new tokens.

#### User Token

The tokens defined by the user in their account, can be used just like any other access token to authenticate the request. However, as contrary as the tokens obtained from the user credentials, this token does not expire by default, and the user can define the access level over the account resources. So, in this way, the user could define a token for accessing a single device, or for writing to a data bucket without comprising other account resources.

This kind of tokens can be defined directly from the Thinger.io Console in the `Access Tokens` section. The following picture illustrates how to add permissions to the token for writing to a single bucket, but you can configure them for granting access to different resources and actions of your account.

![](../.gitbook/assets/token\_permission.png)

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

This method allows getting fresh access token and refresh token from a valid refresh token. It should be called every time the access token has been expired or the refresh token is likely to expire.

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

The access token expires in around 2 hours from its issue date. There are two ways to determine if the access token has been expired in order to request a new one.

#### Checking the JWT contents

The first way of checking if an access token is expired, is by parsing the JWT and decoding the payload data. An access token will have a payload like the following:

```javascript
{
  "exp": 1486048377,
  "iat": 1486041177,
  "usr": "alvarolb"
}
```

The `exp` field is the unix timestamp in seconds (UTC) when the token will expire. If your request is after this timestamp, then, the request will fail.

#### Checking server response

It is possible to check the validity of an access token simply by trying to access any user resource. If the access token is expired, then the authentication will fail, and the API Request query will return a `401 Unautorized`.

