# ACCESS TOKENS

All Thinger.io Platform features can be accessed using REST API Calls in order to integrate our service as a back-end server for any project. In fact, the console is just an Angular REST client interacting with the API to manage devices, buckets, endpoints, dashboards, and so on. Every REST API request must be authenticated in order to take effect, so any client needs to provide an authorization code in every call. This way, access tokens are the way to provide authorization to third-party services or applications to make API Requests, without having to share the username and password. Moreover, with access tokens, it is possible to grant access to specific resources and actions of the account, like reading a specific device or writing to a custom bucket (like in [this example](http://docs.thinger.io/sigfox/#steps-in-thingerio-create-an-access-token)).

{% hint style="info" %}
**Note:** Using an access token via API is covered in more detail [here](http://docs.thinger.io/api/#authentication-api-rest-api-authentication).
{% endhint %}

All Tokens can be easily managed by going to the "Access Tokens" section of the main menu.

<figure><img src="../.gitbook/assets/image (97).png" alt="" width="169"><figcaption></figcaption></figure>

## Create Token Profile

Clicking the green `Add Token` button will open the "Token details context", which allows creating a new token profile and managing the associated permissions:

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

The configurable parameters are:

* **Token ID**: Unique Token ID within the user account.
* **Token Name**: The Token name to easily recognize the token scope.
* **Enabled**: Controls whether the token is enabled or disabled.
* **Token Permissions**: In this section, it is possible to define the scope, or the level of access of the token. Depending on the given permissions, the token will have access to different parts of the account. Adding a new permission will normally require selecting the permission type (access a device, bucket, dashboard, etc.), the level of access (some specific resource or all of their type), and the allowed actions (all or some of them). This configuration is handled by the "Add Token Permission" interface:&#x20;

<figure><img src="../.gitbook/assets/image (100).png" alt="" width="548"><figcaption></figcaption></figure>

When all the parameters are filled, a new Access Token Profile will be added by pressing again the green "New Access Token" button, then, the authorization stream will be displayed in a blue text box at the bottom of the interface:

![](<../.gitbook/assets/image (168).png>)

This authorization can be added as bearer auth. to allow a third-party system to work with Thinger.io Platform features.&#x20;

### Adding Permissions to an Access Token

Each Access Token profile can contain authorization to many different features. The next list shows all the available permission types and the actions that can be defined for each one:

* **Admin Access**: Provides access to the whole account.
* **Device**: Provides access to a single device or all devices. It is possible to define the action between:
  * `AccessDeviceResources`: Grants access to executing device resources, like reading a sensor variable.
  * `ListDeviceResources`: Grants access to list the device resources' names.
  * `GetDeviceStats`: Grants access to read device statistics like public IP, connected time, bandwidth, etc.
  * `CreateDeviceToken`: Grants access to create device tokens.
  * `ListDeviceTokens`: Grants access to read device tokens.
  * `DeleteDeviceToken`: Grants access to delete device tokens.
  * `ListDevices`: Grants access to list the account devices.
  * `DeleteDevice`: Grants access to delete devices.
  * `CreateDevice`: Grants access to create a new device.
  * `UpdateDevice`: Grants access to modify a device, like its description or credentials.
  * `ListDeviceLocations`: Grants access to fetch the locations of the connected devices.
* **Bucket**: Provides access to a single bucket or all buckets. It is possible to define the action between:
  * `ReadBucket`: Grants access to read information stored in a bucket.
  * `WriteBucket`: Grants access to write information to a bucket.
  * `ExportBucket`: Grants access to export the information stored in a bucket.
  * `ClearBucket`: Grants access to clear the information stored in a bucket.
  * `ListBuckets`: Grants access to list the account buckets.
  * `DeleteBucket`: Grants access to delete buckets.
  * `CreateBucket`: Grants access to create a new bucket.
  * `UpdateBucket`: Grants access to modify the bucket configuration, like data sources, description, etc.
  * `ReadBucketConfig`: Grants access to read the current bucket config.
* **Endpoint**: Provides access to a single endpoint or all endpoints. It is possible to define the action between:
  * `ListEndpoints`: Grants access to list the account endpoints.
  * `CreateEndpoint`: Grants access to create a new endpoint.
  * `ReadEndpointConfig`: Grants access to read the current endpoint config.
  * `UpdateEndpoint`: Grants access to modify the endpoint configuration.
  * `DeleteEndpoint`: Grants access to delete endpoints.
  * `CallEndpoint`: Grants access to execute an endpoint.
* **Dashboard**: Provides access to a single dashboard or all dashboards. It is possible to define the action between:
  * `ListDashboards`: Grants access to list the account dashboards.
  * `CreateDashboard`: Grants access to create a new dashboard.
  * `ReadDashboardConfig`: Grants access to read the current dashboard config.
  * `UpdateDashboard`: Grants access to modify the dashboard configuration.
  * `DeleteDashboard`: Grants access to delete dashboards.
* **Token**: Provides access to a single token or all tokens. It is possible to define the action between:
  * `ListTokens`: Grants access to list the account tokens.
  * `CreateToken`: Grants access to create a new token.
  * `ReadTokenConfig`: Grants access to read the current token config.
  * `UpdateToken`: Grants access to modify the token configuration.
  * `DeleteToken`: Grants access to delete tokens.

## Modifying Permissions

The Access Token profile permissions can be modified anytime from the "Edit Token" interface, by clicking the profile identifier in the token list and pressing the green  "+Add" button of the Token Permissions section. This will open the "Add Token Permission" interface again so additional permissions can be added in the same way as in the initial configuration. &#x20;

## Remove Access Token

One or more Access Token profiles can be deleted from the Access Token list by selecting them in the left-side checkbox and pressing the "Remove" button. Also, a Token could be cloned, as it is shown:

<figure><img src="../.gitbook/assets/image (119).png" alt="" width="563"><figcaption></figcaption></figure>
