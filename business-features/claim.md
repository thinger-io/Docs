# CLAIM

The Claim feature is designed for developers who have built a commercial IoT product and need to **simplify and automate management tasks**. Instead of manually assigning resources to each customer, the claim process allows the end user (or their customer) to complete the onboarding and assignment themselves just using a link or QR code.

This approach significantly reduces operational overhead and enables scalable device and customer management.

#### What does a Claim do?

The Claim feature allows end users to **automatically associate platform resources** (such as devices, dashboards, etc.) into their own account, without the intervention of the developer or server administrator.&#x20;

The resources will be Shared from the developer’s account (the one that created the claim), and assigned through a Project (workspace) that is automatically created with predefined access permissions and roles.&#x20;

This ensures that each customer only has access to their own resources, while the developer retains centralized control.

## Configure a New Claim

Configuring a claim requires completing steps across two different features of the platform. While the claim profile must first be created, additional permissions must be granted to allow the end user to access the shared resources.

#### Required preconfiguration steps

1.  **Create a Global Member Role**\
    A global role must be created and assigned to provide appropriate access permissions to the user during the claim process.  [_**learn how to create Global Roles here.**_](projects.md#global-roles)

    > Optionally, it is also possible to request the user to provide configuration variables during the claim process (e.g., thresholds, calibration values, etc.).
2. **Enable Public Sign-Up** _(if required)_\
   If new users are expected to self-register in the platform, public sign-up must be enabled on the server instance.

<figure><img src="../.gitbook/assets/image (772).png" alt=""><figcaption></figcaption></figure>

3. **Assign Claim Resources**\
   The **Claim Resources** section defines which existing resources, owned by the developer, will be shared with the user when the claim is executed.\
   Claims do not create new resources — they **share selected ones** with the end user via a **project** that is created automatically during the claim execution.

#### Adding resources

To create a new claim, go to the **Claims** section from the main menu.

<figure><img src="../.gitbook/assets/image (1).png" alt="" width="262"><figcaption></figcaption></figure>

This view will display the list of existing claims, together withthe number of times each claim has been executed, along with the corresponding dates.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

By clicking the `+Add Claim` button, you will access the claim configuration panel, where you can define a unique **identifier**, as well as a **name** and **description** for the claim profile in the **Claim Information section.**

Then, there are some configuration options that must be used to custom de claim behavior:

* **Enabled switch**: Allows controlling whether a claim is active, without the need to delete it.
* **Max Claims**: Defines the number of times the claim can be executed.\
  A value of `0` or an empty field allows **unlimited claims**, as indicated in the interface:\
  &#xNAN;_“Number of maximum claims that can be done. 0 or empty for unlimited claims.”_
* **Claim resources:** Select the **Resource Type,** it can be anyone of Thinger.io resources and choose **one or more resources** from the list.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

The selected resources will be available to the user once the claim process is completed.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Finally, the advanced options section allows defining additional capacities for the claim, which are not mandatory to be filled but become quite interesting when managing complex infrastructures.&#x20;

* **Claim Domain**: Optional setting to define the domain where the claim code can be used. If no domain is specified, the platform will use the request’s hostname by default. This is useful in multi-tenant or branded environments where claims need to be restricted to specific domains.
* **Additional Project**: Useful to organize claimed devices under a specific project, allowing for centralized access and management.
* **Additional Members**: Users from the account’s member list who will also gain access to the shared resources through the claim.
* **Members Roles**: Optional global roles that will be applied to the additional members included in the claim’s project. These roles define their permissions over the assigned resources.

Once a Claim profile has been configured, the platform generates a claim URL and a QR code that can be used to start the claiming process:&#x20;

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>







## Claiming a resource

This section describes the process of executing a claim to obtain access to a resource. It is the procedure intended for **end users**, starting from a QR code or URL provided in advance. The process involves a few simple steps, including resource verification and optional configuration of parameters.

#### Step 1: Account Authentication

The first step requires logging into the platform or creating a new user account.&#x20;

{% hint style="warning" %}
Public sign-up must be enabled on the server in order to allow new users to register.
{% endhint %}

<figure><img src="../.gitbook/assets/image (773).png" alt="" width="375"><figcaption></figcaption></figure>

Once the account is created and access granted, the **Claim Resources** process begins. This consists of three steps. In the first step, the **Claim Identifier** is displayed, allowing verification that the correct claim process is being used.

<figure><img src="../.gitbook/assets/image (774).png" alt=""><figcaption></figcaption></figure>

#### Step 2: Resource Review

The second step presents an overview of all resources that will be assigned to the user account, along with the project where they will be placed.

> If the user account already contains other resources claimed through previous claim executions, the same project will be reused to group them consistently.

<figure><img src="../.gitbook/assets/image (775).png" alt=""><figcaption></figcaption></figure>

#### Step 3: Optional Resource Configuration

Before completing the process, it is possible to configure certain **resource parameters**. This is done by clicking the **"Configure"** button shown next to the claimed resource.

<figure><img src="../.gitbook/assets/image (776).png" alt=""><figcaption></figcaption></figure>

This functionality is especially useful for requesting contextual input from the end user, such as sensor calibration values, alert thresholds, or notification email addresses.

<figure><img src="../.gitbook/assets/image (777).png" alt=""><figcaption></figcaption></figure>

Once the claim process is completed, the assigned resource (e.g., a device) will appear in the **user’s resource list** (e.g., under _Devices_). However, ownership of the resource remains under the administrator’s account. This structure enables scalable management of large device networks while maintaining centralized control.

