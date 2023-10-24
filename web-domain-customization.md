# CUSTOM WEB DOMAIN

The private Instance default web domain can be replaced with different custom web domains, providing an additional white labeling feature, and supporting for multi-tenant deployments in which each customer can use its own URL to access a different rebrand of the cloud console. This feature can be easily managed using the Domain Administration Manager, available at Thinger.io main menu.

{% hint style="warning" %}
This feature is only available in professional licenses, so only Medium, Large and Unlimited licenses can create custom web domain profiles. &#x20;
{% endhint %}

## Create a new Web Domain

Pressing "Add Domain" button in the Domains List interface allows to access a domain creation form context, in which it is possible to introduce the new web domain for the instance and description as shown in the image below:&#x20;

![](<.gitbook/assets/image (101).png>)

Before adding the new web domain, it is necessary to verify the disponibility and create a secure certificate that will provide secure communications with the web console. To make this, press the "Verify Domain" button.&#x20;

![](<.gitbook/assets/image (165).png>)

### Redirecting the CNAME Entry&#x20;

An important part of this process is to resolve the redirection between the new web domain and the private server's original web domain by going to the domain administration service.&#x20;

![](<.gitbook/assets/image (159).png>)

Once the redirection has been made and the DNS service has propagated the A register, it is possible to verify the domain using the Domain Details button.&#x20;

![](<.gitbook/assets/image (138).png>)

After this process the domain will be ready to be used in a rebranding profile, allowing users to access the private instance with the custom URL.

## Modify Web Domain

In the Domain List, clicking over the domain name allows opening the web domain parameters. It is not possible to change the URL, if needs to be modified, it is necessary to remove the profile and create a new one.&#x20;

## Remove Web Domain

Any Web Console profile can be easily deleted just selecting it in the Domain List and clicking into the red "Remove Domain" button.

![](<.gitbook/assets/image (115).png>)

Note that if a web domain is associated to a web console rebranding, removing it will prevent access to the console.



