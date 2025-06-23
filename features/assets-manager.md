# ASSET TYPES & GROUPS

This section allows you to define sets of IoT devices, in order to group those that have common characteristics or purposes, so we will distinguish types of devices for those that are units with the same purpose and Groups to associate devices for OTHER REASONS. Note that a device can be of one type and belong to a group at the same time.&#x20;

These groupings improve the organization of projects, but also support the following functionalities:&#x20;

* Hereditary Properties. It is possible to define group properties and properties of a type, which will be inherited by the devices that are associated.
* Geofencing: Within the types and groups, it is possible to define areas to monitor the location of devices and generate alerts without one of them entering or leaving the established area.
* Data processing (Geofencing, alerts, data transformation).&#x20;

The assets section has the overview tool that allows us to visualize on the map all the devices created in the selected project, and allows us to quickly check the location and connection status of each device. The green circle will be displayed if the device is connected and the red one if the device is disconnected.

&#x20;

<figure><img src="../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

This tool allows several display options to show only the connected or disconnected devices or, by selecting the "clustering" option, the possible grouping by nearby locations. In the following sections, the use of asset types and groups will be explained in more detail.

## Asset types

They are devices deployed with the same objective, which perform the same function, so they match the same configuration parameters. For example, we can create a device type "Temperature sensors", "freezers", "Smart Cars".&#x20;

### &#x20;Creating Asset Types

To create a new device type, go to the `Assets > Types` tab, displaying the list of previously created asset types, if any. Then, clicking on the green "Add Asset Type" button will open the "new Asset Type" form, in which the identifiers must be filled in:&#x20;

<figure><img src="../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

Once the form filled properly, clicking again on the "Add Asset Type" button will create the new Type and displays its configuration panel, in which the de overview devices map will be shown as well as Properties and Geofencing configuration tabs that can be used to define the properties and geofences for all the devices included into this Asset Type.

<figure><img src="../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

Note that no devices will be shown until anyone begins attaching to this specific device Type.

### Attaching devices to an Asset Type

To start adding devices to the new Asset Type, you must access the devices section of the main menu and display the Devices List. Note that each device can be selected by means of a checkbox on the left side of the list.&#x20;

<figure><img src="../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>

Selecting one or several devices will display new buttons in the upper area of the list. Pressing the green "Set Type" button, a new context will be displayed where the asset type can be selected by means of a drop-down list.&#x20;

<figure><img src="../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>

![](<../.gitbook/assets/image (512).png>)

All the devices that were checked will be associated with the selected Type, from then on they will inherit their properties and the alert and geofences configuration, so let's explain how to configure those features.

### Deleting an Asset Type&#x20;

Any resource can be deleted from the asset type by reassigning it to an empty type in the list or by accessing the settings section of the resource and checking off the box, after this, the properties and geofences inherited from your asset type will be deleted.

![](<../.gitbook/assets/image (398).png>)

It is also possible to completely delete an asset type by selecting its profile in the Types list and clicking on the "Delete Type" button, but note that none of the assigned resources will be deleted, they will only cease to be associated with this category.

![](<../.gitbook/assets/image (516).png>)

## Groups

They are different devices with a common semantic characteristic, for example, the devices installed in the kitchen of a smart home can be grouped under the name "kitchen devices".

### Creating a new Asset Group

To create a new assets group, go to the `Assets > Group` tab of the main menu, this will display the list of previously created Groups, if any. Then, clicking on the green "Add Asset Type" button will open the "new Group" form,  in which the data must be introduced:

<figure><img src="../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>

Once the form is completed, clicking again on the "Add Group" button will create a new Group profile and display its configuration panel in which the de devices overview map will be shown as well as Properties and Geofencing configuration tabs, that can be used to define the behavior of this features for all the devices included into this Assets Group.

<figure><img src="../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

Note that no devices will be shown until anyone begins attaching to this specific device Group profile.

### Grouping devices

Start adding devices to the new Assets Group just require getting access to the devices section of the main menu and displaying the `Devices List`. Note that each device can be selected by means of a checkbox on the left side of the list. When doing this, new buttons appear on the top side of the menu, allowing you to manage the device association into a specific Group, Asset or Project.&#x20;

<figure><img src="../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

Selecting one or several devices will display new buttons in the upper area of the list. Pressing the desired button allows you to aggregate those devices to any of the previously created aggregations. A new context will be displayed in order to select the Group from a drop-down list.

<figure><img src="../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

Since the moment of the association, all the devices will inherit the properties and geofence configuration.

### Deleting Devices Group

Any resource can be deleted from the Asset Group by reassigning it to an empty type in the list or by accessing the settings section of the resource and checking off the box, after this, the properties and geofences inherited from the Asset Group will be deleted.

![](<../.gitbook/assets/image (500).png>)

It is also possible to completely delete an asset type by selecting its profile in the Groups list and clicking on the "Delete Group" button, but note that none of the assigned resources will be deleted, they will only cease to be associated with this category.

![](<../.gitbook/assets/image (378).png>)

## Asset Type Properties

Properties created in an Asset Type will be inherited by every associated device. This establishes a scalable way to configure large networks of devices in a short time and easily. The properties allow you to enter configuration parameters or any other context information you want to make available to the devices or other resources of the platform.

To create a new property on an Asset Type, go to `Asset > Types` section of the main menú, then access the Type profile to be modified and select the properties tab on the right-side menu of the configuration panel. Then, creating a property only requires pressing on `+Add` button and implementing a JSON file format to structure the data that is to be included in the property.

![](<../.gitbook/assets/image (412).png>)

Note that, on the device properties section, each property source will be identified in the "source" column.

<figure><img src="../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

## Geofencing &#x20;

Geofencing is a technique that allows defining a virtual perimeter over geographical areas. It can be defined as a radius around a point or tracing polygons. This is a very useful functionality to monitor automatically and in real-time a fleet of devices, allowing the creation of customized alerts based on geolocation.

Thinger.io server will compare the location of the device, create alerts according to the custom configuration that can be managed below. To start creating Geofence areas, the edition switch on the right-top-side of the map must be clicked ON. Then a new menu allows selecting the geofence shape that can be drawn circular, square or free.&#x20;

![](<../.gitbook/assets/image (357).png>)

Each defined geofence will create in the lower panel a profile that will allow you to configure its behavior. The next image shows the configuration options for the geofences created above for each situation in which any device can be located:

![](<../.gitbook/assets/image (373).png>)

* **name**: Identification of the geofence area
* **On Enter**: Allows selecting an endpoint that will be called automatically when the device just **entered** the area, i.e. when the last position was out of the border and the new position is within the border.&#x20;
* **On Exit**: Allows selecting an endpoint that will be automatically called when the devices are just **leaving** the area, i.e when the last position was within the geofence area and the new position is out.&#x20;
* **While Inside**: This endpoint will always be called when the device is **within the area**. So, it is preferable not to use this option if the device sampling interval is short
* **While Outside**: This endpoint will always be called when the device is **out of the area**. So, it is preferable not to use this option if the device sampling interval is short
* **Color**: To select the color of the representation on the geofences map.
* **Enabled**: Each geofence can be disabled/enabled using the right-side switch.&#x20;



