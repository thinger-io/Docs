# USER ACCOUNTS

Professional and Enterprise Thinger.io Server Instances have been equipped with support for multiple user accounts with different authorization levels, which can be managed from the administration account. This feature has been developed to allow organizations to create project working groups and provide support for B2C or B2B2C products, in which it is necessary to create a network of end-user accounts with read-only permissions.

Each user account is isolated from the rest of the accounts of the instance. This means that they will not share devices or other resources that have not been explicitly shared through a project. The different roles explained below only limit the action capabilities of each account.

{% hint style="info" %}
Note that the use of user accounts is closely related to project management. If unfamiliar with this feature, please go to [**Projects Manager**](projects.md) section of the documentation.&#x20;
{% endhint %}

## User Roles

Three different user account roles are available, depending on the usage privileges to be granted to a user. This section describes each of them and the considerations to be taken into account when deploying them.

### Administrator account

Each instance can have more than one administrator account. Only the original account indicated during the subscription will have permission to modify the parameters of the subscription or the Addons amount, but the administrator accounts will have full permissions to:

* [x] Create and manage IoT resources
* [x] Work with server plugins
* [x] Projects and Assets aggrupation
* [x] Accounts Administration
* [x] Domains or Rebrandings

An Administrator user must be someone the main developer trusts since he will be able to perform all kinds of operations on the branding and other resources of the instance. In order to deploy these accounts, the subscription must be extended with an additional Add-on.

### **Domain admin**

The "domain admin" role has administrator role capacities within a specific web domain that is specified in the. `domain` section of the "new account" form. This role typically grants broad privileges for performing administrative tasks in a multitenant hierarchy such as user management, device configuration, access control, and project monitoring across the entire domain.&#x20;

Some typical functions associated with the "domain admin" role in Thinger.io might include:

* [x] Create and manage IoT resources
* [x] Work with server plugins
* [x] Projects and Assets aggrupation
* [x] Accounts Administration
* [ ] Domains or Rebrandings

In summary, the "domain admin" role in Thinger.io provides a high level of control and responsibility for administering the platform and resources within a specific domain.

### **Developer account**

This account is aimed at other collaborators of the organization to develop on the instance with full development capacities but some limitations on administration privileges:

* [x] Create and manage IoT resources
* [x] Work with server plugins
* [x] Projects and Assets aggrupation
* [ ] Accounts Administration
* [ ] Domains or Rebrandings

### **User account**

Guest accounts are created for displaying data or work only with the IoT resources that has been specifically shared with him  by an administrator or developer account. They will not be able to create new resources or use the plugins.&#x20;

* [ ] Create and manage IoT resources
* [ ] Work with server plugins
* [ ] Projects and Assets aggrupation
* [ ] Accounts Administration
* [ ] Domains or Rebrandings

The creation of this User accounts are free of charge for Thinger.io privated instances, as they have been included to allow the creation of large B2C and B2B2C projects. As they are lightweight accounts, they do not place a high load on the server, so a large network of users can be created without overloading the host.

## Create new user accounts

To start working with the user's network, scroll down to the "Administration" section of the main menu and click the "User Accounts" tab to access the accounts management interface, which will display each user account profile and the `Add User` button that allows creating them as explained in the sections below.

<figure><img src="../.gitbook/assets/image (682).png" alt=""><figcaption></figcaption></figure>

Then, pressing the **`+ Add User`** button of the user administration list opens the `User details` form context in which the new user properties can be introduced:&#x20;

<figure><img src="../.gitbook/assets/image (679).png" alt="" width="563"><figcaption></figcaption></figure>

* **Username**: Account username, this parameter also works as a user identifier.&#x20;
* **Role**: Allows setting the new profile as administrator, User or Project member.
* **Email**: needs to be a valid email account. Only emails introduced on this list can create a user account in order to prevent intruders.
* **Password**: Is the security key for logging into this new user account.
* **Enabled**: Each user account can be enabled/disabled just by clicking this switch
* **Email Verified**: Put this off will send a mail verification to the user when signing up in order to confirm the authenticity of the email.

When a user account is created in Thinger.io, it is possible to impose specific **Account Limits** to control their access and resource consumption. These limits allow administrators to define the maximum number or capacity of various resources—such as Access Tokens, Buckets, Dashboards, Devices, Endpoints, Plugins, Projects, Project Members, Alarm Rules, Alarm Instances, Syncs, Storages, Asset Types, and Asset Groups—that a user can create or manage.

This feature provides granular control over resource allocation, ensuring efficient system management and helping to manage overall platform load. Limits can be set to 'Default', adhering to the system's predefined configurations, or customized to specific numerical values based on individual user needs or subscription tiers. Additionally, 'Storage Limits' like 'Bucket Retention' can be configured to control data storage duration for the account.

When the form is completed, a new user profile will be added to the IoT server by pressing the `+ Add user` button.

### Managing project member authorizations:&#x20;

Project members are light accounts created to share IoT resources with customers or other guests who only require read or display capabilities. Therefore, once a new member profile has been created, it is required to associate it with at least one project using the project manager and set the privileges that it will obtain.

<figure><img src="../.gitbook/assets/image (684).png" alt=""><figcaption></figcaption></figure>

Go to **`Projects`** menu tab and select a previously created project. A project with one member already added:

<figure><img src="../.gitbook/assets/image (683).png" alt=""><figcaption></figcaption></figure>

Then click the **`Add Member`**   button. This new menu drops down, and the new user account created will appear:&#x20;

<figure><img src="../.gitbook/assets/image (685).png" alt=""><figcaption></figcaption></figure>

Once a project member has been associated with the project, select the profile and then use the **`Allow`** section to provide permissions to the user or the **`Deny`** section to manage explicitly forbidden actions, it is possible to select the specific resources and operations that can be used by each account.&#x20;

<figure><img src="../.gitbook/assets/image (687).png" alt=""><figcaption></figcaption></figure>

Upon clicking this button, **`Add Member` ,** the new member will appear in the project's member list.

<figure><img src="../.gitbook/assets/image (686).png" alt=""><figcaption></figcaption></figure>

### Subscribe developer accounts amount

Only Admin accounts are able to create other user accounts. The number of user accounts that can be created in an instance is defined during the contracting and deployment of the server. If this value is reached (or if no additional user account was contracted), an error message will appear:

![](<../.gitbook/assets/image (270).png>)

This threshold can be upgraded on "Grow" or "Startup" licenses by going to the management portal, which is only accessible with the administration account using the link below. To access this portal, a one-time password will be required. It can be obtained by introducing the appropriate administration e-mail account in the text box.

{% hint style="success" %}
[**Click here to access the subscription administration portal**](https://thinger.chargebeeportal.com/)
{% endhint %}

The administration portal will display all licenses subscribed using this e-mail address, allowing for the selection of the one to be updated. Just select the one that wants to be modified and press the option "Edit Subscription", finally pressing the option "Addons", the portal will show all the available options and pricing in the Add-ons menu:

![](<../.gitbook/assets/image (480).png>)

After selecting any of them, it is possible to select the amount just before checking out, note that the price  will be fixed per each unit, but it is possible to introduce a discount coupon that will be provided by Thinger.io team if the business model is quite intensive on any of these features in order to hold a cost-effective solution&#x20;

![](<../.gitbook/assets/image (417).png>)

When the subscription begins updated, a confirmation email will be received with an extract of the subscription cost variations. &#x20;

{% hint style="warning" %}
After modifying any of the subscription parameters Thinger.io server instance needs to be reset in order to update de configuration file values.&#x20;
{% endhint %}

This process can be executed using the server administration panel:&#x20;

First, select Cluster Hosts in the Thinger menu.

<figure><img src="../.gitbook/assets/image (690).png" alt=""><figcaption></figcaption></figure>

For example, an existing one has been selected:

<figure><img src="../.gitbook/assets/image (689).png" alt=""><figcaption></figcaption></figure>

In the top right corner, a settings button is visible. When being clicked on, this will appear:

<figure><img src="../.gitbook/assets/image (691).png" alt=""><figcaption></figcaption></figure>

Finally, go to Restar Server and click the "Restart Now" button.

<figure><img src="../.gitbook/assets/image (692).png" alt=""><figcaption></figcaption></figure>

## Remove User Account

User accounts can also be deleted from the Server Instance, just by selecting the checkbox on the left side of the user account profile and clicking the "Remove" button:

<figure><img src="../.gitbook/assets/image (693).png" alt=""><figcaption></figcaption></figure>

{% hint style="danger" %}
Note that when a user account is removed, all its IoT devices, buckets, configurations and personal information will be deleted. It won't be possible to restore them.
{% endhint %}
