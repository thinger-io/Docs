# SERVER CONFIGURATION

## Web Console Rebranding



## Custom  Web Domain Redirection

## User Accounts Management

Thinger.io Server Instances can support multiple user accounts that can be managed from the administration account. This feature allows creating a user network in which each user account has its own resources such as devices, dashboards, data buckets or even it own plugins.

{% hint style="info" %}
Note that each additional account increases the RAM occupation and CPU load, so it is important to supervise the remaining computational resources when creating new user accounts, specially when using Node-RED plugin.
{% endhint %}

![](.gitbook/assets/image%20%28103%29.png)

First step to manage users network is clicking on the "User Accounts" tab of the main menu. This interface allows showing user accounts list and manage each profile individually as explained in the next sections

### Creating new User Account

Pressing "Add User" button of the user administration list opens a form context in which the new user parameters can be introduced:

![](.gitbook/assets/image%20%28124%29.png)

* **Username**: The account name, this parameter works as user identificator. 
* **Email**: needs to be a valid email account. Only emails introduced on this list can create a user account in order to prevent intruders.
* **Pasword**: Is the security key for login into this new user account
* **Enabled**: Each user account can be enabled/disabled just clicking this switch
* **Email Verified**: Put this off will send a mail verification to the user when sign up in order to confirm the authenticity of the email.

When the form is completed, pressing "add user" button will create a new profile in the list. 

![](.gitbook/assets/image%20%2830%29.png)

### Remove User

![](.gitbook/assets/image%20%2867%29.png)



