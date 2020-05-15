---
description: >-
  This section explains how to use the project management tool to classify
  resources and share them with other user accounts
---

# PROJECTS MANAGER

A "Project" on Thinger.io is an abstract concept created to create resources agrupations \(devices, dashboards, buckets, endpoints or file systems\) allowing better management of their data and configuration. The Project Manager is a tool that also allows to share these resources with other user accounts of the same Thinger.io Platform Instance, allowing them to access, show or use other user resources according to certain permissions. This means that we can authorize other users to work with our account resources as if there were their proprietaries or just with read-only access, depending on the permissions that were managed.  

## Creating new project

To start working with projects, go to the main menu and click into the `Projects` tab, getting access to the projects list. Then press the `Add project`button that will open the new project definition form as shown in the image below:

![](../.gitbook/assets/image%20%28180%29.png)

The form parameters will help to identify the project. Onces that the form is completed with appropriate values, pressing the `Add project`button will create the new project profile that will not be shared by any other user. This way it can be used to organize the resources of different projects, allowing easy navigation when working with large networks, just using the projects navigation menu, on the workspace top bar:

![](../.gitbook/assets/image%20%28164%29.png)

After creating a new project it will not be selected by default, so before start creating resources, the little folder icon of the top bar must be pressed in order to select the desired one.

### Adding project members

Ones a new project profile has been created, it is possible to include project members with which all resources will be shared according to specific authorizations provided by the project creator. To start adding members, open the configuration menu by pressing project's identificator on the list, then press into the blue `Project Members` button as shown in the image below:

![](../.gitbook/assets/image%20%28122%29.png)

This will open the projects member list, in which the user accounts that belong to the same project will be displayed allowing to modify permissions in the future. It will be empty the first time we access to an recently created project as shown in the image below:

![](../.gitbook/assets/image%20%2850%29.png)

To include the first member, just press the green `Add Member` button, that will open the next form, allowing to introduce the user account identificator and permission preferences. 

![](../.gitbook/assets/image%20%2820%29.png)

* **User ID:** Note that it must necessarily belong to a user account of the same Thinger.io instance. It is not possible to share projects with users from other instances or from the community server, since they are not in the same database.
* **Enabled:** This switch allows to enable or disable the user on the project without destroying it's shared configuration. 
* **Allow:** This section allows assigning specific permissions to the selected user account and also displays current permissions. 
* **Deny:** This section denies specific permissions to the selected user account, so for example if in the `Allow` section we provided full authorization to device capacities, this ones can be used to deny only the ability to delete the device.

#### Managing member permissions 

Permissions can be added individually for each resource, but it is also possible to select the Admin Access option, allowing the other user full power over the administration of all the resources we share in the project. To add a new permission, pressing the green **`+Add`** button will open the Add token permission context in which the resource whose authorization is going to be managed can be selected:

![](../.gitbook/assets/image%20%28231%29.png)

When a resource type is selected on the list \(for example buckets\), the interface will show additional options to provide permissions to  all profiles `Any Bucket` or to an individual profile from the existen buckets list `Specific Bucket` : 

![](../.gitbook/assets/image%20%2855%29.png)

Finally, the same logic is applied into the `Actions` section, allowing to provide full permissions to the selected resource profile `Any Action` or to select only one specific permission `Select specific action` .

## Working with projects

### Projects Navigation 

The same user account can participate in many projects, that's why the thinger console has a tool on the top bar that allows you to select the project you want to work with at any time:

![](../.gitbook/assets/image%20%28219%29.png)

This tool allows moving between different projects, displaying only the devices associated with each one. It also allows disabling the projects filtering functions, allowing to show every resources created into the user account, i.e. other users' resources will no longer be visible.

#### Shared resources

Items that appear in the account lists with a user's icon on the left side are resources shared by another user. In this way it is easy to identify in a list which devices are owned and which are not. The image below shows three devices, two of them are shared devices from other accounts, being the "example\_device"  a profile created on the own account. 

![](../.gitbook/assets/image%20%2879%29.png)

### Creating project resources 

The association of device profiles, data buckets or dashboards to a specific project is done during creation. When a new resource is added with a project selected in the top bar, the resource is associated with that project. This situation can be displayed in the device's api, which will show the project it belongs to:

![](../.gitbook/assets/image%20%28118%29.png)

Note that in the API of a third party device, in addition to the project identifier we can display the account it belongs to from that device:

![](../.gitbook/assets/image%20%2897%29.png)

{% hint style="info" %}
Currently it is not possible to aggregate an existent resource to a project, it is important take this in consideration specially in relation with Data buckets, that can't be deleted to make a new profile.
{% endhint %}

