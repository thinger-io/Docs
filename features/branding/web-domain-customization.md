# CUSTOM WEB DOMAIN

Private Instances web-domain can be replaced with different custom web domains, providing an additional white labeling feature, and supporting for multi-tenant deployments in which each customer can use it's own URL to access a different rebrand of the cloud console. This feature can be easily managed using the Domain Administration Manager, available at Thinger.io main menu.

## Create a new Web Domain

Pressing "Add Domain" button in the Domains List interface allows accessing to a domain creation form context, in which it is possible to introduce the new web domain for the instance and description as shown in the image below: 

![](../../.gitbook/assets/image%20%28209%29.png)

Before adding the new web domain, it is necessary to verify the disponibility and create a secure certificate that will provide secure communications with the web console. To make this, press the "Verify Domain" button. 

![](../../.gitbook/assets/image%20%28228%29.png)

### Redirecting the CNAME Entry 

An important part of this process is to resolve the redirection between the new web domain and the private server's original web domain by going to the domain administration service. 

![](../../.gitbook/assets/image%20%28211%29.png)

Once the redirection has been made and the DNS service has propagated the A register, it is possible to verify the domain using the Domain Details button. 

![](../../.gitbook/assets/image%20%2849%29.png)

After this process the domain will be ready to be used in a rebranding profile, allowing users to access the private instance with the custom URL.

## Modify Web Domain

In the Domain List, clicking over the domain name allows opening the web domain parameters. It is not possible to change the URL, if needs to be modified, it is necessary to remove the profile and create a new one. 

## Remove Web Domain

Any Web Console profile can be easily deleted just selecting it in the Domain List and clicking into the red "Remove Domain" button.

![](../../.gitbook/assets/image%20%28104%29.png)

Note that if a web domain is associated to a web console rebranding, removing it will prevent access to the console.

