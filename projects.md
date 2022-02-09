---
description: >-
  This section explains how to use the project management tool to classify
  resources and share them with other user accounts
---

# PROJECTS MANAGER

A "Project" on Thinger.io is an abstract concept created to create resource groups (devices, dashboards, buckets, endpoints, or file systems) allowing better management of their data and configuration. The Project Manager is a tool that also allows sharing these resources with other user accounts of the same Thinger.io Platform Instance, allowing them to access, show, or use other user resources according to certain permissions. This means that we can authorize other users to work with our account resources as if there were their proprietaries or just with read-only access, depending on the permissions that were managed. &#x20;

## Creating a new project

To start working with projects, go to the main menu and click into the `Projects` tab, getting access to the projects list. Then press the `Add project`button that will open the new project definition form as shown in the image below:

![](<.gitbook/assets/image (180).png>)

The form parameters will help to identify the project. Once that the form is completed with appropriate values, pressing the `Add project`button will create the new project profile that will not be shared by any other user. This way it can be used to organize the resources of different projects, allowing easy navigation when working with large networks, just using the projects navigation menu, on the workspace top bar:

![](<.gitbook/assets/image (164).png>)

After creating a new project it will not be selected by default, so before start creating resources, the little folder icon of the top bar must be pressed in order to select the desired one.

### Adding project members

Once a new project profile has been created, it is possible to include project members with which all resources will be shared according to specific authorizations provided by the project creator. To start adding members, open the configuration menu by pressing project's identification on the list, then press into the blue `Project Members` button as shown in the image below:

![](<.gitbook/assets/image (122).png>)

This will open the project's member list, in which the user accounts that belong to the same project will be displayed allowing to modify permissions in the future. It will be empty the first time we access to a recently created project as shown in the image below:

![](<.gitbook/assets/image (50).png>)

To include the first member, just press the green `Add Member` button, that will open the next form, allowing to introduce the user account identification and permission preferences.&#x20;

![](<.gitbook/assets/image (20).png>)

* **User ID:** Note that it must necessarily belong to a user account of the same Thinger.io instance. It is not possible to share projects with users from other instances or from the community server, since they are not in the same database.
* **Enabled:** This switch allows to enable or disable the user on the project without destroying it's shared configuration.&#x20;
* **Allow:** This section allows assigning specific permissions to the selected user account and also displays current permissions.&#x20;
* **Deny:** This section denies specific permissions to the selected user account, so for example if in the `Allow` section we provided full authorization to device capacities, this ones can be used to deny only the ability to delete the device.

#### Managing member permissions&#x20;

Permissions can be added individually for each resource, but it is also possible to select the Admin Access option, allowing the other user full power over the administration of all the resources we share in the project. To add a new permission, pressing the green **`+Add`** button will open the Add token permission context in which the resource whose authorization is going to be managed can be selected:

![](<.gitbook/assets/image (231).png>)

When a resource type is selected on the list (for example buckets), the interface will show additional options to provide permissions to  all profiles `Any Bucket` or to an individual profile from the existent buckets list `Specific Bucket` :&#x20;

![](<.gitbook/assets/image (55).png>)

Finally, the same logic is applied to the `Actions` section, allowing to provide full permissions to the selected resource profile `Any Action` or to select only one specific permission `Select specific action` .

![](<.gitbook/assets/image (79).png>)

### Projects Navigation&#x20;

The same user account can participate in many projects, that's why the thinger console has a tool on the top bar that allows you to select the project you want to work with at any time:

![Enabling Projects mode](<.gitbook/assets/image (359).png>)

This tool allows moving between different projects, displaying only the devices associated with each one. It also allows disabling the projects filtering functions, allowing to show every resource created into the user account, i.e. other users' resources will no longer be visible.

![project "ExampleProject" selected](<.gitbook/assets/image (387).png>)

{% hint style="warning" %}
NOTE that by default, the member does not log in with any project opened, so it is necessary to select after the first login (we are working on it, so a default project can be established for members)
{% endhint %}

###

### Adding project resources

Once the project is created we can start to associate resources such as device profiles, data buckets or representation dashboards, allowing them to be shared with other developers of the same project. There are two different ways to attach resources to a specific project:

#### Attach when creating new resources

The resources are associated with the project in which they were created. So if a project is selected during the creation of any resource, this new profile will be automatically attached to the project:

1. Enabling projects as shown in the section above&#x20;
2. Selecting a project from the drop-down list
3. Creating a new device / data bucket / Endpoint / dashboard to be shared with project members&#x20;

#### Aggregating existent resources to a new project

The lists allow the administration of resources to associate them to projects, asset types or groups. It is possible to assign previously created resources to a new project by selecting it in the list and pressing the black `Set Projects` button that will be shown on the top of the list:&#x20;

![](<.gitbook/assets/image (389).png>)

## Working with projects

#### Shared resources identification

The "project" column of the resources list (devices, dashboards, endpoints or data buckets) allows to identify the project in which the resource has been atached. Those items that appear in the lists with a user icon together with the name are resources shared by another user.&#x20;

The image below shows some devices, most of them are shared devices from other accounts, and two of them have been created by their own account.

![](<.gitbook/assets/image (349).png>)

The association of any resource to a project can be checked using the REST API, which will show the project it belongs to:

![](<.gitbook/assets/image (118).png>)

Note that in the API of a third-party device, in addition to the project identifier we can display the account it belongs to from that device:

![](<.gitbook/assets/image (97).png>)

