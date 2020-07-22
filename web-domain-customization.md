# CUSTOM WEB DOMAIN

Private Instances web-domain can be replaced with different custom web domains, providing an additional white labeling feature, and supporting for multi-tenant deployments in which each customer can use it's own URL to access a different rebrand of the cloud console. This feature can be easily managed using the Domain Administration Manager, available at Thinger.io main menu.

## Create a new Web Domain

Pressing "Add Domain" button in the Domains List interface allows accessing to a domain creation form context, in which it is possible to introduce the new web domain for the instance and description as shown in the image below: 

![](.gitbook/assets/image%20%28209%29.png)

Before adding the new web domain, it is necessary to verify the disponibility and create a secure certificate that will provide secure communications with the web console. To make this, press the "Verify Domain" button. 

![](.gitbook/assets/image%20%28228%29.png)

### Redirecting the CNAME Entry 

An important part of this process is to resolve the redirection between the new web domain and the private server's original web domain by going to the domain administration service. 

![](.gitbook/assets/image%20%28211%29.png)

Once the redirection has been made and the DNS service has propagated the A register, it is possible to verify the domain using the Domain Details button. 

![](.gitbook/assets/image%20%2849%29.png)

After this process the domain will be ready to be used in a rebranding profile, allowing users to access the private instance with the custom URL.

## Modify Web Domain

In the Domain List, clicking over the domain name allows opening the web domain parameters. It is not possible to change the URL, if needs to be modified, it is necessary to remove the profile and create a new one. 

## Remove Web Domain

Any Web Console profile can be easily deleted just selecting it in the Domain List and clicking into the red "Remove Domain" button.

![](.gitbook/assets/image%20%28104%29.png)

Note that if a web domain is associated to a web console rebranding, removing it will prevent access to the console.

## Subscribe Web Domain License.

This feature is reserved for professional uses, so only Grow, Startup, and Business licenses can create custom web domain profiles when they were subscribed during the deployment of the license or later using the subscription administration console. The next sections explain how to include Web Domain Addons with a Thinger.io private instance in both situations:   

### During the deployment

Our deployment tool allows configuring the license with multiple Add-ons such as custom web domains, custom brands, or additional user accounts. It can be added to the license before the deployment process by means of the configuration table 

![](.gitbook/assets/image%20%28290%29.png)

Note that more than one branding or web domain profile can be added, as it is possible to create as many brandings as needed in order to adapt the look and feel of the web console to different IoT projects or customers. When the limit is reached, the web console will show the next error message "cannot create more domains" to inform that it is required to upgrade the number of custom domain profiles as explained in the section below. 

### After the deployment

If the amount of contracted brand profiles is reached, \(or if no additional brands were contracted\), it is possible to upgrade the threshold by going to the management portal using the link below. 

{% hint style="success" %}
\*\*\*\*[**Click here to access the subscription administration portal**](https://thinger.chargebeeportal.com/)\*\*\*\*
{% endhint %}

The administration portal only can be accessed by the instance administrator using his email address to obtain a one-time password. Once logged in, the administration portal will show all the licenses that have been subscribed to this e-mail address. Just select the license that wants to be modified and press the option "Edit Subscription", finally, pressing the option "Addons", the portal will show all the available options and pricing in the Add Add-ons menu:

![](.gitbook/assets/image%20%28272%29.png)

After selecting any of them, it is possible to select the amount just before checking out, note that the price  will be fixed per each unit, but it is possible to introduce a discount coupon that will be provided by Thinger.io team if your business model is quite intensive on any of these features in order to hold a cost-effective solution 

![](.gitbook/assets/image%20%28303%29.png)

When the subscription begins updated, a confirmation email will be received with an extract of the subscription cost variations. 



