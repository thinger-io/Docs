# USERS MANAGEMENT

Thinger.io Server Instances can support multiple user accounts that can be managed from the administration account. This feature allows creating a user network in which each user account has its own resources such as devices, dashboards, data buckets or even its own plugins.

{% hint style="info" %}
Note that each additional account increases the RAM occupation and CPU load, so it is important to supervise the remaining computational resources when creating new user accounts, especially when using Node-RED plugin.
{% endhint %}

![](.gitbook/assets/image%20%28168%29.png)

The first step to manage the user's network is clicking on the "User Accounts" tab of the main menu. This interface allows showing user accounts list and manages each profile individually as explained in the next sections.

## Create new User Account

Pressing "Add User" button of the user administration list opens a form context in which the new user parameters can be introduced:

![](.gitbook/assets/image%20%28208%29.png)

* **Username**: Account username, this parameter also works as user identificator. 
* **Email**: needs to be a valid email account. Only emails introduced on this list can create a user account in order to prevent intruders.
* **Pasword**: Is the security key for login into this new user account.
* **Enabled**: Each user account can be enabled/disabled just clicking this switch
* **Email Verified**: Put this off will send a mail verification to the user when sign up in order to confirm the authenticity of the email.

When the form is completed, a new user profile will be added to the IoT server by pressing "add user" button.

### User accounts limit

The amount of user accounts that can be created in an instance is defined during the contracting and deployment of the server. If this value is reached \(or if no additional user account was contracted\) an error message will appear as shown in the image below:

![](.gitbook/assets/image%20%2854%29.png)

This threshold can be upgraded on "Grow" or "Startup" licenses by going to the management portal, which is only accessible with the administration account using the link below. To access this portal a one-time password will be required. It can be obtained by introducing the appropriate administration e-mail account in the text box.

{% hint style="success" %}
\*\*\*\*[**Click here to access the subscription administration portal**](https://thinger.chargebeeportal.com/)\*\*\*\*
{% endhint %}

The administration portal will show all the licenses that have been subscribed using this e-mail address, allowing to select the one that wants to be updated. Just select the one that wants to be modified and press the option "Edit Subscription", finally pressing the option "Addons", the portal will show all the available options and pricing in the Add Addons menu:

![](.gitbook/assets/image%20%28267%29.png)

After selecting any of them, it is possible to select the amount just before checking out, note that the price  will be fixed per each unit, but it is possible to introduce a discount coupon that will be provided by Thinger.io team if your business model is quite intensive on any of these features in order to hold a cost-effective solution 

![](.gitbook/assets/image%20%28287%29.png)

When the subscription begins updated, a confirmation email will be received with an extract of the subscription cost variations. 

## Remove User Account

User accounts can also be deleted from the Server Instance, just selecting the checkbox in the left side of the user account profile and clicking the "Remove User" button. 

![](.gitbook/assets/image%20%28111%29.png)

{% hint style="danger" %}
Note that, when a user account is removed, all its IoT devices, buckets, configurations and personal information will be deleted and it won't be possible to restore them.
{% endhint %}

