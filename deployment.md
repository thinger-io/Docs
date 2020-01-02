---
description: >-
  This section explains how to perform the process of subscription and
  deployment of a private Thinger.io platform server.
---

# SERVER DEPLOYMENT

Freemium accounts are perfect for learning and testing Thinger.io platform with only few limitations, however, for getting the best performance and reliability of this platform and access to some advanced features that are essential for professional use, it is necessary to deploy a private Thinger.io Server. 

## Private Instance Benefits

Thinger.io supports private cloud deployments that can be automatically launched from the pricing page. Private instances are isolated servers for each customer, so the instance is not shared with other thousands of users from our community. 

Next list details every Thinger.io Private instances advantages: 

* 100% **Private Server**, hosted in AWS, Digital Ocean, Google Cloud, Microsoft Azure cloud providers, or on-premise host.
* **Unlimited** devices, Dashboards, Data Buckets, Access Token & sampling intervals
* **Plugins** System Deployment with different extensions available. 
* **Multiple User Support** that allows to create and manage individual customer accounts in your server.  
* **Multi Tenancy Support** with multiple web-console rebranding profiles and web domains hosted by just one server instance.  
* Support for multiple databases with real-time **data aggregation**

## Subscribing and Deploying a Cloud Instance

This process allows launching a completely private and customized Thinger.io IoT Server within minutes just accessing out Pricing Page in the "pricing" button of our Web Home \(Also available at this [**link**](https://pricing.thinger.io)\). and following the next steps:

### 1. Select any cloud provider

Private instances can be started in different cloud providers like Digital Ocean, Amazon Web Services, Google Cloud, or Azure in different availability zones. In that order, if our customer has already been using any of these providers for their company cloud infrastructure, it is possible to run Thinger.io Private server in the same location. 

![](.gitbook/assets/image%20%28132%29.png)

### 2. Select the right license

Private instances can be deployed over four licensing possibilities, each one provides different privileges and is fitted with an appropriate hardware host to the size of each project, as shown in the image below:

![](.gitbook/assets/image%20%2897%29.png)

This pricing includes the software license and all cloud expenses. Note that it is possible to select monthly or yearly license with a great discount. 

The next table shows all different features that are provided by each license as well as a desirable purpose specification. It is possible to select one license and change it in the future using the administration account. 

|  | **MAKER** | **GROW** | **STARTUP** | **BUSINESS** |
| :--- | :--- | :--- | :--- | :--- |
| **Devices** | Unlimited | Unlimited | Unlimited | Unlimited |
| **Dashboards** | Unlimited | Unlimited | Unlimited | Unlimited |
| **Plugins** | 1 | 3 | 6 | Unlimited |
| **Rebranding** | \*\*\*\* | ✓ | ✓ | Included |
| **Custom Domain** | \*\*\*\* | ✓ | ✓ | Included |
| **Extended support** | \*\*\*\* | ✓ | ✓ | included |
| **Multiple Account** | \*\*\*\* | ✓ | ✓ | ✓ |
| **Cluster Server** | \*\*\*\* |  |  | ✓ |
| **High Availability**  | \*\*\*\* |  |  | ✓ |
| **Recommended network size** | Individual projects | &lt;10 user accounts. | 10 to 50 accounts |  &gt;50 user accounts |

### 3.  Configure preferences

Ones the license has been selected, it is possible to custom the service with some preferences such as:

* **Region**: each cloud provider has server farms in different locations around the world. This option allows you to select the closest in order to minimize latency or host the private instance on the same farm as the rest of the client's enterprise software.
* **Hostname**: enter here the grey-label name that will have the IoT server. This hostname will always be accompanied by the subdomain ".thinger.io" unless the "custom domains" option is selected.
* **Admin Email**: this account will be assigned for the administration of the private server. It will be the only one that will be able to register new users, domains, rebrandings, plugins etc.
* **Additional users**: Every private instance always has one user account, but this option allows to increase the amount of accounts in order to share the server with other collaborators or customers. However, note that the server could be overloaded if a large number of user accounts with plugins is added, affecting the proper functioning of the instance.
* **Extended Support**: This option is recommended in order to obtain Thinger.io engineers development support. It provides 24-48h response time, however all accounts can use the community discussion forum to obtain support from other community developers at: https://community.thinger.io
* **Custom Domains**: It is the amount of different web domains that can be redirected to the same Private Instance in order to create multiple rebrandings.
* **Custom Brands**: It is the amount of different console rebrandings that can be created over the same server. Each rebrand may have a different customization of colors, web-domain and logotypes.

### 4. Checkout and payment options

Ones everything has been configured, the checkout process is really simple, just introduce your billing email \(the one that will receive the invoices from our side\), that can also be the same as the "Admin Email". 

![](.gitbook/assets/image%20%28179%29.png)

Then include your billing address. VAT number will be required if the customer is in the European Union in order to calculate the right taxes and build the invoice.

![](.gitbook/assets/image%20%2890%29.png)

Finally, it is necessary to select the payment method between Credit Card or Direct Debit, that allows the domiciliation of the payment with SEPA tansferences. Once the payment process is finished, Thinger.io customer management system will automatically configure the cloud host and deploy the private server instance. 

## Steps After Contracting

The deployment process delays few minutes. As soon as it has been completed a confirmation mail will be sent to the "Admin Email", meaning that the server is completely ready. To start working with it, just follow the next steps:

### First Login

1\) Access the server by writing the selected web-domain in a web browser, for example "acme.do.thinger.io".  This allows accessing the login page of the private server instance.   

2\) Note that this server has never been accessed before, and it is a completely isolated instance so there is not any user account created, so it is necessary to click on "Create account" button, and fill the form to create a new user profile using the "Admin Email" address \(any other address will obtain an unauthorized message\).

3\) After creating the new account it is possible to access the new server. It is not necessary to confirm the mail address. 

### First Device Connection

When working with a private Thinger.io Server Instance, it is necessary to modify the devices server hostname in order to introduce the right one. To make the redirection the next code line needs to be introduced on the top of the devices source code: 

`#define THINGER_SERVER`

Not doing this will cause the device to try to establish a connection to the public Thinger.io Platform Community Server instead of working against the new private server.

## On premise instances

## Devices redirection

