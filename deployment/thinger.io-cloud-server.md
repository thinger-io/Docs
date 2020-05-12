# Thinger.io Cloud

## Subscribing and Deploying a Cloud Instance

This section describres the proccess to deploy a private Thinger.io Cloud instance within minutes by just accessing the [**Pricing Page**](https://pricing.thinger.io) \(Cloud Pricing section\) in the "PRICING" link from our [Web Home](https://thinger.io). Just follow the next steps:

### 1. Select a cloud provider

Private instances can be deployed in different cloud providers like Digital Ocean, Amazon Web Services, Google Cloud, or Azure in different availability zones. In that order, if our customer has already been using any of these providers for their company cloud infrastructure, it is possible to run Thinger.io Private server in the same location. 

![Cloud provider selection between Digital Ocean, Amazon Web Services, Google Cloud and Azure](../.gitbook/assets/image%20%28167%29.png)

### 2. Select a license

Private cloud instances can be deployed with different licenses, depending on the project requriements, like host performance, bandwidth; or platform features like rebrands, custom domains, additional suppport,  plugins, etc. Once the cloud provider is selected, then it is necessary to select required license, as shown in the image below:

![](../.gitbook/assets/image%20%28120%29.png)

This pricing includes the software license and all cloud expenses. Note that it is possible to select monthly or yearly license with a great discount. 

The next table shows all different features that are provided by each license as well as a desirable purpose specification. It is possible to select one license and change it in the future using the [subscription management portal](https://thinger.chargebeeportal.com). 

|  | **MAKER** | **GROW** | **STARTUP** | **BUSINESS** |
| :--- | :--- | :--- | :--- | :--- |
| **Devices** | Unlimited | Unlimited | Unlimited | Unlimited |
| **Plugins** | 1 | 3 | 6 | Unlimited |
| **Rebranding** | \*\*\*\* | ✓ | ✓ | Included |
| **Custom Domain** | \*\*\*\* | ✓ | ✓ | Included |
| **Multiple Account** | \*\*\*\* | ✓ | ✓ | Included |
| **Extended support** | \*\*\*\* | ✓ | ✓ | ✓ |
| **HA Cluster** | \*\*\*\* |  |  |  |
| **Recommended network size** | Individual projects | &lt;10 user accounts. | 10 to 50 accounts |  &gt;50 user accounts |

### 3.  Configure license

Once the license has been selected, it is possible to customize the service with some options like additional users, support, custom domains, brands, etc. These options appers after selecting a plan, like shown in the figure below:

![Instance license preferences](../.gitbook/assets/image%20%28195%29.png)

These options are described in more detail in the following:

* **Region**: Cloud providers provides servers in different geographic locations. This option allows to select the closest region to your business or project in order to minimize latency between the instance and the devices, users consuming information, etc. It is recommended to select the closest region to the project location.
* **Hostname**: Enter the hostname for your private IoT instance. This hostname will always be accompanied by the subdomain ".thinger.io" to access your host. It is possible to add custom domains selecting the additional domains option.
* **Admin Email**: This is the email address that must be used when creating the Thinger.io account in the private instance deployed. It will be the main account with admin privileges, allowing to create \(if contracted\) new users, domains, brands, etc.
* **Additional users**: This option allows to increase the amount of accounts \(in addition to the admin account\) in order to allow other users to log-in to your instance. It is possible to add as many accounts as required if no plugins are deployed, otherwise, each user plugin will increment the server computational load so it may require high a performance host in order to support their databases, plugins, etc. 
* **Extended Support**: This option is recommended in order to obtain Thinger.io engineers development support with a 24-48h response time. All accounts can use the [community discussion](https://community.thinger.io) forum to obtain support from other community developers.
* **Custom Domains**: It is the amount of different web domains that can be redirected to the same Private Instance. The instance will automatically provision and renew SSL certificates to support the new defined domains. Domains are required for different brands, as they are associated to the domain name.
* **Custom Brands**: It is the amount of different console brands that can be created over the same instance. In each brand it is possible to customize some accent colors, logos, copyrights, contact emails, title, etc. Notice that each brand requires a custom domain, as the customization is done for each domain name.

### 4. Checkout and payment options

After configuring the selected license, the checkout process is really simple, just click on the "Deploy Instance" button, and wait for the checkout pop-up. In the new form, it is necessary to introduce your billing email \(it can be different from the admin email\) where the invoices will be sent: 

![Billing email](../.gitbook/assets/image%20%28228%29.png)

After introducing the billing email, it is necessary to set the billing address. It is possible to set a VAT number if you are a registered company from the European Union in order to calculate the right taxes and build the invoice.

![Billing address configuration](../.gitbook/assets/image%20%28109%29.png)

Finally, it is necessary to select the payment method between Credit Card or Direct Debit, that allows the domiciliation of the payment with SEPA tansferences. Once the payment process is finished, Thinger.io customer management system will automatically configure the cloud host and deploy the private server instance. 

## Steps After Cloud Deployment

The deployment process delays few minutes. As soon as it has been completed, a confirmation email will be sent to the `Admin Email`configured in the checkout process, meaning that the server is completely ready to be used. To start working with it, just follow the next steps:

### First Login

1. Access the server by writing the configured domain in a web browser, for example: [https://acme.do.thinger.io](https://acme.do.thinger.io). This step should show you the Thinger.io login screen.
2. Note that this server has never been accessed before, and it is a completely isolated instance so there is not any user account created. Then, it is necessary to click on `Create an account`button, and fill the form to create a new user profile using the `Admin Email` address provided while configuring the instance \(any other address will not be authorized to sign up\).
3. After creating the new account it is possible to access the new server. It is not necessary to confirm the mail address.

### Device Connection

When working with a private Thinger.io Cloud Instance, it is necessary to point your devices to the newly created hostname. If you are using the [Arduino](../devices/arduino.md) or [Linux](../devices/linux.md) client libraries, i.e., for Arduino, ESP8266, ESP32, Raspberry Pi, etc., you should add a definition on top of your code to point to your host. So, modify your sketch like this:

```text
#define THINGER_SERVER "acme.do.thinger.io"

// the rest of your code goes here
```

{% hint style="info" %}
If this host definition is not provided, your devices will try to connect with the public instance. 
{% endhint %}

