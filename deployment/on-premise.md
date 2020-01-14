# On-Premise

## Subscribing and Deploying On-premise Instances

This section describres the proccess to deploy a private Thinger.io On-premise instance within minutes by just accessing the [**Pricing Page**](https://pricing.thinger.io) \(On-premise section\) in the "PRICING" link from our [Web Home](https://thinger.io). Just follow the next steps:

### 1. Select the right licence

On-premise instances can be deployed with different licenses, depending on the project requriements, mainly in terms of platform features like rebrands, custom domains, additional suppport, plugins, etc. On-premise licenses starts for free with the Maker license, but limited with some advanced features enabled in the following tiers, most notably, custom domains with automatic SSL support, rebranding, plugins, multiple server accounts, or extend the server capabilities with plugins like Node-Red. So, the first step is to select the required license, as shown in the image below:

![On-premise license selection](../.gitbook/assets/image%20%28154%29.png)

This pricing includes the software license to deploy an on-premise instance. Note that it is possible to select monthly or yearly license with a great discount. 

The next table shows all different features that are provided by each license as well as a desirable purpose specification. It is possible to select one license and change it in the future using the administration account. 

|  | **MAKER** | **GROW** | **STARTUP** | **BUSINESS** |
| :--- | :--- | :--- | :--- | :--- |
| **Devices** | Unlimited | Unlimited | Unlimited | Unlimited |
| **Plugins** |  | 3 | 6 | Unlimited |
| **Rebranding** | \*\*\*\* | ✓ | ✓ | Included |
| **Custom Domain** | \*\*\*\* | ✓ | ✓ | Included |
| **Multiple Account** | \*\*\*\* | ✓ | ✓ | Included |
| **Extended support** | \*\*\*\* | ✓ | ✓ | ✓ |
| **HA Cluster** | \*\*\*\* |  |  |  |
| **Recommended network size** | Individual projects | &lt;10 user accounts. | 10 to 50 accounts |  &gt;50 user accounts |

### 2.  Configure preferences

Ones the license has been selected, it is possible to customize the service with some preferences such as:

* **Region**: each cloud provider has server farms in different locations around the world. This option allows you to select the closest in order to minimize latency or host the private instance on the same farm as the rest of the client's enterprise software.
* **Hostname**: enter here the grey-label name that will have the IoT server. This hostname will always be accompanied by the subdomain ".thinger.io" unless the "custom domains" option is selected.
* **Admin Email**: this account will be assigned for the administration of the private server. It will be the only one that will be able to register new users, domains, rebrandings, plugins etc.
* **Additional users**: Every private instance always has one user account, but this option allows to increase the amount of accounts in order to share the server with other collaborators or customers. However, note that the server could be overloaded if a large number of user accounts with plugins is added, affecting the proper functioning of the instance.
* **Extended Support**: This option is recommended in order to obtain Thinger.io engineers development support. It provides 24-48h response time, however all accounts can use the community discussion forum to obtain support from other community developers at: https://community.thinger.io
* **Custom Domains**: It is the amount of different web domains that can be redirected to the same Private Instance in order to create multiple rebrandings.
* **Custom Brands**: It is the amount of different console rebrandings that can be created over the same server. Each rebrand may have a different customization of colors, web-domain and logotypes.

Becoming a super hero is a fairly straight forward process:

```
$ give me super-powers
```

{% hint style="info" %}
 Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

Once you're strong enough, save the world:

{% code title="hello.sh" %}
```bash
# Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```
{% endcode %}



