# USER ACCOUNTS

Professional and Enterprise Thinger.io Server Instances han sido dotadas de un support para multiple user accounts with different authorization levels, that can be managed from the administration account. This feature has been developed to allow organizations to create project working groups and provide support for B2C or B2B2C products in which is necessary to create a network of end user accounts with read-only permissions.

Each user account is isolated from the rest of the accounts of the instance. This means that they will not share devices or other resources that have not been explicitly shared through a project. The different roles explained below only limit the action capabilities of each account.

{% hint style="info" %}
Note that the use of user accounts is closely related to project management. If you are not familiar with this feature, please go to [**Projects Manager**](projects.md) section of the documentation. 
{% endhint %}

## User Roles

Depending on the usage priviledges you wish to grant a user, there are three different user account roles. This section describes each of them and the considerations to be taken into account when deploying them.

### Administrator account

Each instance can have more than one administrator account. Only the original account indicated during the subscription will have permission to modify the parameters of the subscription or the Addons amount, but the administrator accounts will have full permissions to:

* [x] Create and manage IoT resources
* [x] Work with server plugins
* [x] Projects and Assets aggrupation
* [x] Accounts Administration
* [x] Domains or Rebrandings

An Administrator user must be someone the main developer trusts, since he will be able to perform all kinds of operations on the branding and other resources of the instance. In order to deploy these accounts, the subscription must be extended with an additional Addon.

### **Developer account**

This account is aimed for other collaborators of the organization to develop on the instance with full developement capacities but some limitations on administration priviledges:

* [x] Create and manage IoT resources
* [x] Work with server plugins
* [x] Projects and Assets aggrupation
* [ ] Accounts Administration
* [ ] Domains or Rebrandings

### **User account**

Guest accounts created for displaying data or work only with the IoT resources that has been specifically shared with him  by an administrator or developer account. They will not be able to create new resources or use the plugins. 

* [ ] Create and manage IoT resources
* [ ] Work with server plugins
* [ ] Projects and Assets aggrupation
* [ ] Accounts Administration
* [ ] Domains or Rebrandings

The creation of this User accounts are free of charge for Thinger.io privated instances, as they have been included to allow the creation of large B2C and B2B2C projects. As they are lightweight accounts, they do not place a high load on the server, so a large network of users can be created without overloading the host.

## Create new user accounts

To start working with user's network, scroll down to the "Administration" section of the main menu and click the "User Accounts" tab to access the accounts management interface, that will display each user account profile and the `Add User` button that allows to create them as explained in the sections below.

![](.gitbook/assets/image%20%28369%29.png)

Then, pressing the **`+ Add User`** button of the user administration list opens the `User details` form context in which the new user properties can be introduced:

![](.gitbook/assets/image%20%28371%29.png)

* **Username**: Account username, this parameter also works as user identificator. 
* **Role**: Allows to set the new profile as administrator, User or Project member.
* **Email**: needs to be a valid email account. Only emails introduced on this list can create a user account in order to prevent intruders.
* **Password**: Is the security key for login into this new user account.
* **Enabled**: Each user account can be enabled/disabled just by clicking this switch
* **Email Verified**: Put this off will send a mail verification to the user when signing up in order to confirm the authenticity of the email.

When the form is completed, a new user profile will be added to the IoT server by pressing the `+ Add user` button.

### Managing project member authorizations: 

Project members are light accounts created to share IoT resources with customers or other kind of  guests that only requires read or display capacities. That way, once a new member profile has been created, it is required to associate it with at least one project using the projects manager and set the priviledges that it will obtain. 

![](.gitbook/assets/image%20%28367%29.png)

1\) go to **`Projects`** menu tab and select a previously created project to open the project details panel. 

![](.gitbook/assets/image%20%28370%29.png)

2\)Then click the  blue  **`Project Members`**   button to show the project member list. Next image shows an empty project list in which a new member can be added: 

![](.gitbook/assets/image%20%28366%29.png)

3\) Clicking the green **`+ Add member`** button allows selecting a user account profile from the server users network list. 

![](.gitbook/assets/image%20%28368%29.png)

4\) Once a project member has been associated to the project, select the profile and then using the **`Allow`** section to provide permissions to the user or the **`Deny`** section to manage explicitly forbidden actions, it is possible to select the specific resources and operations that can be used by each account. 

### Subscribe developer accounts amount

The amount of user accounts that can be created in an instance is defined during the contracting and deployment of the server. If this value is reached \(or if no additional user account was contracted\) an error message will appear as shown in the image below:

![](.gitbook/assets/image%20%2854%29.png)

This threshold can be upgraded on "Grow" or "Startup" licenses by going to the management portal, which is only accessible with the administration account using the link below. To access this portal a one-time password will be required. It can be obtained by introducing the appropriate administration e-mail account in the text box.

{% hint style="success" %}
\*\*\*\*[**Click here to access the subscription administration portal**](https://thinger.chargebeeportal.com/)\*\*\*\*
{% endhint %}

The administration portal will show all the licenses that have been subscribed using this e-mail address, allowing to select the one that wants to be updated. Just select the one that wants to be modified and press the option "Edit Subscription", finally pressing the option "Addons", the portal will show all the available options and pricing in the Add Addons menu:

![](.gitbook/assets/image%20%28272%29.png)

After selecting any of them, it is possible to select the amount just before checking out, note that the price  will be fixed per each unit, but it is possible to introduce a discount coupon that will be provided by Thinger.io team if your business model is quite intensive on any of these features in order to hold a cost-effective solution 

![](.gitbook/assets/image%20%28302%29.png)

When the subscription begins updated, a confirmation email will be received with an extract of the subscription cost variations.  

{% hint style="warning" %}
After modifying any of the subscription parameters Thinger.io server instance needs to be reset in order to update de configuration file values. 
{% endhint %}

This process can be executed using the server administration panel as shown in the image below: 

![](.gitbook/assets/image%20%28313%29.png)

## Remove User Account

User accounts can also be deleted from the Server Instance, just selecting the checkbox in the left side of the user account profile and clicking the "Remove User" button. 

![](.gitbook/assets/image%20%28111%29.png)

{% hint style="danger" %}
Note that, when a user account is removed, all its IoT devices, buckets, configurations and personal information will be deleted and it won't be possible to restore them.
{% endhint %}

