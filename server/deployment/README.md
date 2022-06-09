---
description: >-
  This section explains how to perform the process of subscription and
  deployment of a private Thinger.io platform server.
---

# SERVER DEPLOYMENT

Freemium accounts are perfect for learning and testing Thinger.io platform with only few limitations, however, for getting the best performance and reliability of this platform and access to some advanced features that are essential for professional use, it is necessary to deploy a private Thinger.io Server.&#x20;

## Private Instance Benefits

Thinger.io supports private cloud deployments that can be automatically launched from the pricing page. Private instances are isolated servers for each customer, so the instance is not shared with other thousands of users from our community.&#x20;

Next list details every Thinger.io Private instances advantages:&#x20;

* 100% **Private Server**, hosted in AWS, Digital Ocean, Google Cloud, Microsoft Azure cloud providers, or on-premise host.
* **Unlimited** devices, Dashboards, Data Buckets, Access Token & sampling intervals
* **Plugins** System Deployment with different extensions available.&#x20;
* **File Storage System** that allows saving context data or any kind of files
* **Multiple User Support** allows to create and manage individual customer accounts in your server. &#x20;
* **Multi-Tenancy Support** with multiple web-console rebranding profiles and web domains hosted by just one server instance. &#x20;
* Support for multiple databases with real-time **data aggregation**

## Hosting options

Private instances can be deployed within minutes over the main cloud provider hosts but also in an On-premise server:

{% content-ref url="thinger.io-cloud-server.md" %}
[thinger.io-cloud-server.md](thinger.io-cloud-server.md)
{% endcontent-ref %}

{% content-ref url="on-premise.md" %}
[on-premise.md](on-premise.md)
{% endcontent-ref %}

## Managing subscription

The administrator of a private instance of Thinger.io can edit the preferences of the subscription using the management portal, for this you must access the website [**https://thinger.chargebeeportal.com/**](https://thinger.chargebeeportal.com/) **** obtaining a One Time Password that will be sent to your email when you execute the request.

![](<../../.gitbook/assets/image (316).png>)

This portal allows to manage every subscription preferences such as **changing the administration email** account**, modify payment details** and also editing the **subscription Addons** for each license that has been contracted on Thinger.io system:

![](<../../.gitbook/assets/image (373).png>)

### Addons contracting

The image below shows the different Addons that can be contracted for a Grow License. Note that Maker subscriptions will not be able to select these options:

![](<../../.gitbook/assets/image (315).png>)

* **Additional users**: This option allows to increase the amount of accounts (in addition to the admin account) in order to allow other users to log-in to your instance. It is possible to add as many accounts as required if no plugins are deployed, otherwise, each user plugin will increment the server computational load so it may require high a performance host in order to support their databases, plugins, etc.&#x20;
* **Extended Support**: This option is recommended in order to obtain Thinger.io engineers development support with a 24-48h response time. All accounts can use the [community discussion](https://community.thinger.io) forum to obtain support from other community developers.
* **Custom Domains**: It is the amount of different web domains that can be redirected to the same Private Instance. The instance will automatically provision and renew SSL certificates to support the new defined domains. Domains are required for different brands, as they are associated to the domain name.
* **Custom Brands**: It is the amount of different console brands that can be created over the same instance. In each brand it is possible to customize some accent colors, logos, copyrights, contact emails, title, etc. Notice that each brand requires a custom domain, as the customization is done for each domain name.

Once the desired Addons have been selected, the user can indicate the quantities in which he wishes to acquire these supplements and execute the confirmation. The increases in the amount of the subscription will be prorated in the next payment. &#x20;

{% hint style="warning" %}
After modifying any of the subscription parameters Thinger.io server instance needs to be reset in order to update de configuration file values.&#x20;
{% endhint %}

This process can be executed using the server administration panel as shown in the image below:&#x20;

![](<../../.gitbook/assets/image (313).png>)

### Manage Payment Methods

It is possible to modify the payment method by accessing the section "Payment Methods" where you can choose to make payments with a Credit Card or Direct Debit that allows the domiciliation of the payment with SEPA transferences.

![](<../../.gitbook/assets/image (377).png>)

### Cancel subscription

Any subscription can be cancelled, with changes being applied at the end of the contracted billing period. Please note that Thinger.io does not refund partial subscription fees unless otherwise agreed.&#x20;

To cancel a subscription perform the following steps:&#x20;

1. Log in to the management portal https://thinger.chargebeeportal.com/&#x20;
2. Select the subscription you wish to cancel
3. In the subscription administration panel click on the `cancel subscription` option.

![](<../../.gitbook/assets/image (448) (1) (1).png>)
