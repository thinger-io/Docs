---
description: >-
  This section explains how to use the project management tool to classify
  resources and share them with other user accounts
---

# PROJECTS MANAGER

The **Projects** feature in Thinger.io is designed to enable secure and flexible **collaborative access to IoT resources** within a shared platform environment. It provides a mechanism to group and share devices, dashboards, data buckets, file storages, and endpoints with other user accounts under controlled permission rules.

## Creating a new project

To start working with projects, go to the main menu and click on the `Projects` tab, which gives access to the project list.  Then, press the `Add project`button and fill the "project details" form:

<figure><img src="../.gitbook/assets/image (664).png" alt=""><figcaption></figcaption></figure>

Once a new project is created, it is initialized as an empty container with no associated resources or collaborators. In order to make it functional, it is necessary to manually assign resources such as devices, dashboards, data buckets, or endpoints, as well as invite user accounts to collaborate within the project. These resources and users can be added through their respective management sections within the project interface, allowing precise control over what is shared and how it is accessed.

### Add members to a project

In this context, it is possible to assign access to existing user accounts already registered in the Thinger.io server, or to create new user accounts directly from the project environment. Newly created users are automatically assigned the default role of _project member_. For each user, access permissions can be configured individually, allowing fine-grained control over which actions the user can perform within the project. Alternatively, predefined roles can be used to apply a consistent set of permissions across multiple users, streamlining project access management.&#x20;

<figure><img src="../.gitbook/assets/image (657).png" alt=""><figcaption></figcaption></figure>

### Project roles

This section allows defining custom sets of permissions that are specific to the current project. Instead of assigning individual permissions to each member, create a named role (e.g., 'Viewer', 'Operator', 'Admin') with predefined access levels, and then assign that role to multiple project members. This simplifies permission management and ensures consistent access control within the project. Once `Add Role`is clicked and the desired options are selected, the new role will be displayed in the list:

<figure><img src="../.gitbook/assets/image (677).png" alt=""><figcaption></figcaption></figure>

### Global Roles

This option allows to create roles that will be available for every project, having the same permissions, so it's simple and faster to configure new project members. To create a new Global Role profile, it's required to go back to the _Projects List,_ hit the "Add Role" button, and complete the form, including permissions to the required feature&#x73;_._ Note that the Global Role configuration is the same as the one shown in the _Project Role_ or the individual permissions context explained in the section below.&#x20;

<figure><img src="../.gitbook/assets/image (661).png" alt=""><figcaption></figcaption></figure>

### Adding project members

Once the project is created, this tab will appear. It allows for adding members, refreshing the view, and accessing other functionalities such as project roles and the dashboard.

<figure><img src="../.gitbook/assets/image (666).png" alt="" width="563"><figcaption></figcaption></figure>

After clicking the `Add Member`button, this menu will appear:&#x20;

<figure><img src="../.gitbook/assets/image (667).png" alt=""><figcaption></figcaption></figure>

When configuring a project member, these options are available:

*   **Member Details**

    * **Existing User:** This switch allows either creating a new user account for the project or selecting an existing user from the current Thinger.io instance. If enabled, it typically reveals a field to search for an existing user.



    <figure><img src="../.gitbook/assets/image (671).png" alt=""><figcaption></figcaption></figure>

    * **Username:** Defines the unique username for a new member being added to the project.
    * **Email:** Specifies the email address associated with the new member's account.
    * **Password:** Sets the initial password for the new member's account.
* **Member Configuration**
  * **Enabled:** This switch allows enabling or disabling the member's access to the project without removing their shared configuration or permissions.
  * **Hide Menu:** When activated, this option hides the main navigation menu for the member when they access the project dashboard, often used for dedicated or kiosk-style views.
* **Member Permissions**
  * **Global Roles:** This field allows assigning predefined global roles to the member, which grant them a set of permissions across the entire Thinger.io instance, influencing their access within this and other projects.
  * **Project Roles:** Enables the assignment of specific roles defined _within this particular project_ to the member, controlling their permissions exclusively within the scope of the current project.
  * **Project Groups:** Allows adding the member to existing project groups, where they will inherit permissions previously defined for that group within the project.
* **Specific Permissions**
  * **Allow:** This section allows assigning specific permissions to the selected user account, explicitly granting them authorization for certain actions or resources within the project. It also displays all currently granted "allow" permissions.
  * **Deny:** This section explicitly denies specific permissions to the selected user account. This is a powerful feature for fine-grained control; for instance, if the "Allow" section provides broad authorization (e.g., full device capacities), this "Deny" section can be used to prevent specific actions, such as deleting a device, even if the general permission is granted.

Additionally, it is important to note that the user who creates the project has the power to select the roles of each member, defining their specific permissions. Each role offers various configurable options, which in turn unlock new possibilities. There's a wide array of configurations possible!&#x20;

{% hint style="info" %}
Be sure to click the `Add Member`button at the end to save the process.
{% endhint %}

This will open the project's member list, in which the user accounts that belong to the same project will be displayed, allowing to modify permissions in the future. It will be empty the first time we access a recently created project, but as soon as a Member is added, it will appear in the list:

<figure><img src="../.gitbook/assets/image (673).png" alt=""><figcaption></figcaption></figure>

#### Managing member permissions&#x20;

Permissions can be added individually for each resource, but it is also possible to select the Admin Access option, allowing the other user full power over the administration of all the resources we share in the project. To add a new permission, press the green **`+Add`** button will open the Add token permission context in which the resource whose authorization is going to be managed can be selected:

<figure><img src="../.gitbook/assets/image (672).png" alt="" width="456"><figcaption></figcaption></figure>

When a resource type is selected on the list (for example: buckets), the interface will show additional options to provide permissions to  all profiles `Any Bucket` or to an individual profile from the existing buckets list `Specific Bucket` :&#x20;

![](<../.gitbook/assets/image (435).png>)

Finally, the same logic is applied to the `Actions` section, allowing to provide full permissions to the selected resource profile `Any Action` or to select only one specific permission `Select specific action` .

![](<../.gitbook/assets/image (456).png>)

### Project Dashboard

Each project can have its own dashboard in order to display data from multiple resources on the same screen.  This dashboard will replace the default "Statistics" section in the main navigation menu of the Thinger.io platform, adopting the name of the corresponding project. It serves as the primary interface for visualizing and interacting with project-specific data.

By enabling project-level dashboards, Thinger.io allows full customization of the platform’s look and feel, tailoring the visual experience to the specific context and requirements of each use case. This approach enhances clarity, usability, and stakeholder engagement by presenting only the relevant metrics, widgets, and controls associated with the selected project.

The operation is the same as that of any common dashboard, so for further details, refer to the [Dashboard section](../features/dashboards.md).&#x20;

Clicking the 3-bars icon will reveal 'Settings' and 'Inspector' options:

<figure><img src="../.gitbook/assets/image (668).png" alt=""><figcaption></figcaption></figure>

**Project Settings**

It is possible to rename the project, change its description, or limit the data bucket:

<figure><img src="../.gitbook/assets/image (670).png" alt=""><figcaption></figcaption></figure>

* **Inspector**: Enables users to monitor and troubleshoot project events and data in real-time.

<figure><img src="../.gitbook/assets/image (669).png" alt=""><figcaption></figcaption></figure>



The Event Inspector is a vital tool for real-time monitoring and debugging within a project. It provides a continuous live feed and a historical log of all events and data being generated or processed by devices, assets, and rules. Its primary purpose is to verify correct data transmission, expected rule triggering, and proper functioning of all system communications. If something isn't behaving as it should, the Inspector is the first place to look to see what data is arriving, while also maintaining a running tally of recent events for historical review.

To manage this event stream, the Inspector offers several control options: a 'Connected' indicator that shows the live-streaming status, a 'Filter Events' option to narrow down the displayed events by specific types or criteria, a 'Pause' function to temporarily halt the live feed for detailed analysis, and a 'Clear' button to empty the current display for a fresh monitoring session. In essence, the Event Inspector functions as a comprehensive "console log" or "debug window" for an IoT project, providing immediate visibility into the flow of information and enabling effective troubleshooting of any issues.

### Projects Navigation&#x20;

The same user account can participate in many projects, that's why the Thinger console has a tool on the top bar that allows selecting the project:

<figure><img src="../.gitbook/assets/image (662).png" alt="" width="347"><figcaption></figcaption></figure>

This tool allows moving between different projects, displaying only the devices associated with each one. It also allows disabling the project's filtering functions, allowing to show every resource created in the user account, i.e. other users' resources will no longer be visible.

<figure><img src="../.gitbook/assets/image (675).png" alt="" width="472"><figcaption></figcaption></figure>

{% hint style="warning" %}
NOTE that by default, the member does not log in with any project opened, so it is necessary to select after the first login (we are working on it, so a default project can be established for members)
{% endhint %}

### Adding project resources

Once the project is created, we can start to associate resources such as device profiles, data buckets or representation dashboards, allowing them to be shared with other developers of the same project. There are two different ways to attach resources to a specific project:

#### Attach when creating new resources

The resources are associated with the project in which they were created. So if a project is selected during the creation of any resource, this new profile will be automatically attached to the project:

1. Enabling projects as shown in the section above&#x20;
2. Selecting a project from the drop-down list
3. Creating a new device / data bucket / Endpoint / dashboard to be shared with project members&#x20;

#### Aggregating existing resources to a new project

The lists allow the administration of resources to associate them with projects, asset types or groups. It is possible to assign previously created resources to a new project by selecting them in the list and pressing the black `Set Projects` button that will be shown on the top of the list:&#x20;

<figure><img src="../.gitbook/assets/image (654).png" alt=""><figcaption></figcaption></figure>

## Working with projects

#### Shared resources identification

The "project" column of the resources list (devices, dashboards, endpoints or data buckets) allows identifying the project in which the resource has been attached. Those items that appear in the lists with a user icon, together with the name, are resources shared by another user.&#x20;

Some devices are shown, most of which are shared devices from other accounts, and two of them have been created by their own account.

<figure><img src="../.gitbook/assets/image (653).png" alt=""><figcaption></figcaption></figure>

The association of any resource to a project can be checked using the REST API, which will show the project it belongs to:

![](<../.gitbook/assets/image (350).png>)

Note that in the API of a third-party device, in addition to the project identifier, we can display the account it belongs to from that device:

![](<../.gitbook/assets/image (394).png>)

