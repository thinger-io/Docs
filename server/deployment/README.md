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
* **File Storage System** that allows saving context data or any kind of files
* **Multiple User Support** allows to create and manage individual customer accounts in your server.  
* **Multi-Tenancy Support** with multiple web-console rebranding profiles and web domains hosted by just one server instance.  
* Support for multiple databases with real-time **data aggregation**

## Hosting options

Private instances can be deployed within minutes over the main cloud provider hosts but also in an On-premise server:

{% page-ref page="thinger.io-cloud-server.md" %}

{% page-ref page="on-premise.md" %}

## Managing subscription

The administrator of a private instance of Thinger.io can edit the preferences of the subscription using the management portal, for this you must access the website [**https://thinger.chargebeeportal.com/**](https://thinger.chargebeeportal.com/) ****obtaining a One Time Password that will be sent to your email when you execute the request.

![](../../.gitbook/assets/image%20%28316%29.png)

This portal allows to manage every subscription preferences such as **changing the administration email** account**, modify payment details** and also editing the **subscription Addons** for each license that has been contracted on Thinger.io system:

![](../../.gitbook/assets/image%20%28372%29.png)

### Addons contracting

The image below shows the different Addons that can be contracted for a Grow License. Note that Maker subscriptions will not be able to select these options:

![](../../.gitbook/assets/image%20%28315%29.png)

* **Additional users**: This option allows to increase the amount of accounts \(in addition to the admin account\) in order to allow other users to log-in to your instance. It is possible to add as many accounts as required if no plugins are deployed, otherwise, each user plugin will increment the server computational load so it may require high a performance host in order to support their databases, plugins, etc. 
* **Extended Support**: This option is recommended in order to obtain Thinger.io engineers development support with a 24-48h response time. All accounts can use the [community discussion](https://community.thinger.io) forum to obtain support from other community developers.
* **Custom Domains**: It is the amount of different web domains that can be redirected to the same Private Instance. The instance will automatically provision and renew SSL certificates to support the new defined domains. Domains are required for different brands, as they are associated to the domain name.
* **Custom Brands**: It is the amount of different console brands that can be created over the same instance. In each brand it is possible to customize some accent colors, logos, copyrights, contact emails, title, etc. Notice that each brand requires a custom domain, as the customization is done for each domain name.

Once the desired Addons have been selected, the user can indicate the quantities in which he wishes to acquire these supplements and execute the confirmation. The increases in the amount of the subscription will be prorated in the next payment.  

{% hint style="warning" %}
After modifying any of the subscription parameters Thinger.io server instance needs to be reset in order to update de configuration file values. 
{% endhint %}

This process can be executed using the server administration panel as shown in the image below: 

![](../../.gitbook/assets/image%20%28313%29.png)

#### Manage Payment Methods

It is possible to modify the payment method by accessing the section "Payment Methods" where you can choose to make payments with a Credit Card or Direct Debit that allows the domiciliation of the payment with SEPA transferences.

![](../../.gitbook/assets/image%20%28374%29.png)

