# CLAIM RESOURCES

The **Claim** feature is designed for developers who have built a **commercial IoT product** and need to **simplify and automate management tasks**. Instead of manually assigning resources to each customer, the claim process allows the **end user (or their customer)** to complete the onboarding and assignment themselves just using a link or QR code.

This approach significantly reduces operational overhead and enables scalable device and customer management.

#### What does a Claim do?

The Claim feature allows end users to **automatically associate platform resources** (such as devices, dashboards, etc.) into their own account, without the intervention of the developer or server administrator.&#x20;

The resources will be **Shared from the developer’s account** (the one that created the claim), and assigned through a **Project (workspace)** that is **automatically created** with **predefined access permissions and roles.**&#x20;

This ensures that each customer only has access to their own resources, while the developer retains centralized control.

## Adding a New Claim

The **Claim Resources** section defines which **existing resources owned by the developer** will be shared with the user when the claim is executed.

Claims do not create new resources. Instead, they **share selected resources** with the end user through the automatically created project.

#### Adding resources

To create a new claim, go to the **Claims** section from the main menu.

<figure><img src="../.gitbook/assets/image (1).png" alt="" width="262"><figcaption></figcaption></figure>

This view will display the list of existing claims, together withthe number of times each claim has been executed, along with the corresponding dates.

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

By clicking the `+Add Claim` button, you will access the claim configuration panel, where you can define a unique **identifier**, as well as a **name** and **description** for the claim in the **Claim Information section.**

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

