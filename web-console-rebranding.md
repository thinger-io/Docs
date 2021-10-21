---
description: >-
  This feature allows to custom the appearance of Thinger.io web console to a
  different brand.
---

# WHITE-LABELING

Thinger.io instances support multi-tenant web console customizations. By means of a management tool that is available at the main menu "Rebranding" section, it is possible to create and manage multiple web customizations, in order to custom the aspect of the web console to different customers or projects by changing some elements such as:

* [x] Branding Colors
* [x] Web, Favicon and Main Menu logotypes
* [x] Links, Email accounts, copyright

{% hint style="info" %}
Note that each web console rebrand needs to be supported by an individual web domain, which can be managed in the "Domain" section of the main menu.
{% endhint %}

## Create a new Web Console Rebranding Profile

Clicking into "Add Brand" button of the "Rebranding" section allows creating a new branding profile. The process starts by introducing a web domain name, that will be the identification for the brand profile:

![](<.gitbook/assets/image (178).png>)

If the instance subscription doesn't include any rebranding addon, the next message will be shown in the web console: &#x20;

![](<.gitbook/assets/image (277).png>)

To solve this situation visit "Subscribe Rebranding License" below on this documentation

### Adding Branding Details

If the Domain Name is valid, the form context will expand allowing to complete the branding details sections by editing the standard thinger.io values:

![](<.gitbook/assets/image (25).png>)

All the next elements are non-mandatory, so can be left empty and the system will remove their buttons from the main menu:

* **Domain Name: **Is the URL of the new rebranded web console. This Web Domain needs to be introduced in the system as is explained in the "Custom Web Domain" section.&#x20;
* **Description: **Place here some additional information about the rebranding profile in order to identify it from the others.
* **Page title:** Identificator for the web browser tabs
* **Page URL: **To introduce link to the company, customer or project website
* **Company Name:** Name of the project, customer or company this rebranding belongs to.&#x20;
* **Contact Email:** address for the main menu"Email" button** **that the users are going to contact for support.
* **Twitter Account:  **Social media profile can be added by placing here the complete URL of the twitter profile.
* **Copyright: **The bottom of the website includes a copyright declaration that can be customized here to protect the rebranding rights. &#x20;

### Custom Logotypes

What really makes the difference when creating a rebranding is the use of custom logotypes. The second section of the branding editor allows changing each web console logotype separately. To obtain nice results, it is important to take care about the background color of each logotype in order to obtain enough contrast.

![](<.gitbook/assets/image (102).png>)

{% hint style="info" %}
Logotypes need to be introduced in SVG or PNG file format with transparent background&#x20;
{% endhint %}

### **Custom Top Bar**

The top bar has also a big impact on the website aspect. The branding menu allows changing it aspect in two ways: the Top Bar Color and the text color:

![](<.gitbook/assets/image (80).png>)

It is important also to take care of selected colors in order to obtain a nice contrast between the texts and background.&#x20;

### **PWA**

Thinger.io web console has been prepared with PWA Smartphone responsive features, allowing to create web apps with web console custom preferences that when used on the smartphone allow a user experience very similar to a common APP thanks to the creation of a custom logo in the main menu and hiding the web browser navigation bar.

![](<.gitbook/assets/image (308).png>)

To use the PWA on a smartphone, simply click on the "add to the main menu" option of the browser menu. this functionality can be applied in any of the web console interfaces, even in shared dashboards

![](<.gitbook/assets/image (311).png>)

The PWA function parameters will be automatically configured based on the preferences of the other tabs of the web console rebranding. So it is not possible to introduce different ones. &#x20;

![](<.gitbook/assets/image (312).png>)

## **Modify Console Rebranding**

When the Console Rebranding profile is finished, a new entry will appear into the rebranding administration list as shown in the image below:

![](<.gitbook/assets/image (1).png>)

It is possible to access the configuration form and edit all parameters by clicking into the brand profile identificator, which is the associated web domain.

## Remove a Console Rebranding Profile

A rebranding profile can be easily deleted just selecting it in the Brand List and clicking into the red "Remove Brand" button.

![](<.gitbook/assets/image (100).png>)

## Increase Rebranding Limits&#x20;

This feature is reserved for professional uses, so only Grow, Startup, and Business licenses can create custom rebranding profiles when the rebranding license is acquired during the subscription of the license or later using the subscription administration console. If the license didn't include any branding Addon, the next error will be shown during the creation of the new brand:&#x20;

![](<.gitbook/assets/image (282).png>)

The next sections explain how to include the rebranding Addon with a Thinger.io private instance in both situations:  &#x20;

### During the deployment

Our deployment tool allows configuring the license with multiple Add-ons such as custom web domains, custom brands, or additional user accounts. It can be added to the license before the deployment process by means of the configuration table&#x20;

![](<.gitbook/assets/image (290).png>)

Note that more than one branding or web domain profile can be added, as it is possible to create as many brandings as needed in order to adapt the look and feel of the web console to different IoT projects or customers. When the limit is reached, the web console will show the next error message "cannot create more domains" to inform that it is required to upgrade the number of custom domain profiles as explained in the section below.&#x20;

### After the deployment

If the amount of contracted brand profiles is reached, (or if no additional brands were contracted), it is possible to upgrade the threshold by going to the management portal using the link below.&#x20;

{% hint style="success" %}
****[**Click here to access the subscription administration portal**](https://thinger.chargebeeportal.com)****
{% endhint %}

The administration portal only can be accessed by the instance administrator using his email address to obtain a one-time password. Once logged in, the administration portal will show all the licenses that have been subscribed to this e-mail address. Just select the license that wants to be modified and press the option "Edit Subscription", finally, pressing the option "Addons", the portal will show all the available options and pricing in the Add Add-ons menu:

![](<.gitbook/assets/image (272).png>)

After selecting any of them, it is possible to select the amount just before checking out, note that the price  will be fixed per each unit, but it is possible to introduce a discount coupon that will be provided by Thinger.io team if your business model is quite intensive on any of these features in order to hold a cost-effective solution&#x20;

![](<.gitbook/assets/image (303).png>)

When the subscription begins updated, a confirmation email will be received with an extract of the subscription cost variations.&#x20;

{% hint style="warning" %}
After modifying any of the subscription parameters Thinger.io server instance needs to be reset in order to update de configuration file values.&#x20;
{% endhint %}

This process can be executed using the server administration panel as shown in the image below:&#x20;

![](<.gitbook/assets/image (313).png>)
