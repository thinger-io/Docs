# CUSTOM WEB DOMAIN

The private instance's default web domain can be replaced with different custom web domains, providing an additional white labeling feature and supporting multi-tenant deployments in which each customer can use its own URL to access a different rebrand of the cloud console. This feature can be easily managed using the Domain Administration Manager, available at the Thinger.io main menu.

{% hint style="warning" %}
This feature is only available in professional licenses, so only Medium, Large and Unlimited licenses can create custom web domain profiles. &#x20;
{% endhint %}

## Create a new Web Domain

Select "Domains" on the Thinger menu:

<figure><img src="../.gitbook/assets/image (699).png" alt=""><figcaption></figcaption></figure>

Pressing the "Add Domain" button in the Domains List interface allows access to a domain creation form context, in which it is possible to introduce the new web domain for the instance and description as shown in the image below:&#x20;

<figure><img src="../.gitbook/assets/image (700).png" alt="" width="563"><figcaption></figcaption></figure>

After adding the new web domain, it is necessary to verify the availability and create a secure certificate that will provide secure communications with the web console. To make this, press the "Verify Domain" button:

<figure><img src="../.gitbook/assets/image (703).png" alt=""><figcaption></figcaption></figure>

### Redirecting the CNAME Entry&#x20;

An important part of this process is to resolve the redirection between the new web domain and the private server's original web domain by going to the domain administration service.&#x20;

![](<../.gitbook/assets/image (159) (1).png>)

Once the redirection has been made and the DNS service has propagated the A record, it is possible to verify the domain using the Domain Details button. If it is not validated, an error message will pop up. If it is validated, the following will be shown:

<figure><img src="../.gitbook/assets/image (702).png" alt=""><figcaption></figcaption></figure>

After this process, the domain will be ready to be used in a rebranding profile, allowing users to access the private instance with the custom URL.

## Modify Web Domain

In the Domain List, clicking on the domain name opens the web domain parameters. It is not possible to change the URL, if it needs to be modified, it is necessary to remove the profile and create a new one.&#x20;

## Remove Web Domain

Any Web Console profile can be easily deleted just selecting it in the Domain List and clicking the "Remove" button.

<figure><img src="../.gitbook/assets/image (704).png" alt=""><figcaption></figcaption></figure>

Note that if a web domain is associated with a web console rebranding, removing it will prevent access to the console.



