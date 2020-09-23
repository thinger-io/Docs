# ASSET TYPES & GORUPS

This section allows you to define sets of IoT devices, in order to group those that have common characteristics or purposes, so we will distinguish types of devices for those that are units with the same purpose and Groups to associate devices for OTHER REASONS. Note that a device can be of one type and belong to a group at the same time. 

These groupings improve the organization of projects, but also support the following functionalities: 

* Hereditary Properties. It is possible to define group properties and properties of a type, which will be inherited by the devices that have associated.
* Geofencing: Within the types and groups it is possible to define areas to monitor the location of devices and generate alerts without one of them entering or leaving the established area.
* Data processing \(Geofencing, alerts, data transformation\). 

The assets section has the overview tool that allows us to visualize on the map all the devices created in the selected project, allows us to quickly check the location and connection status of each device. The green circle will be drowned if the device is connected and the red one if the device is disconnected. 

![](../.gitbook/assets/image%20%28349%29.png)

This tool allows several display options to show only the connected or disconnected devices or, selecting the "clustering" option, the possible grouping by nearby locations. In the following sections, the use of asset types and groups will be explained in more detail.

## Asset types

There are devices deployed with the same objective, which perform the same function so they match the same configuration parameters. For example, we can create a device type "Temperature sensors", "freezers", "Smart Cars". 

###  Creating Asset Types

To create a new device type go to the `Assets > Types` tab, displaying the list of previously created asset types if any. Then clicking on the green "Add Asset Type" button, will open the "new Asset Type" form, in which the identifiers must be filled in: 

![](../.gitbook/assets/image%20%28340%29.png)

Once the form filled properly, clicking again on the "Add Asset Type" button will create the new Type and displays its configuration panel, in which the de overview devices map will be shown as well as Properties and Geofencing configuration tabs that can be used to define the properties and geofences for all the devices included into this Asset Type.

![](../.gitbook/assets/image%20%28341%29.png)

Note that no devices will be shown until anyone begins attached to this specific device Type.

### Attaching devices to an Asset Type

To start adding devices to the new Asset Type you must access the devices section of the main menu and display the Devices List. Note that each device can be selected by means of a checkbox on the left side of the list. 

![](../.gitbook/assets/image%20%28332%29.png)

Selecting one or several devices will display new buttons in the upper area of the list. Pressing the green "Set Type" button a new context will be displayed where the asset type can be selected by means of a drop-down list.

![](../.gitbook/assets/image%20%28352%29.png)

All the devices that were checked up will be associated with the selected Type, from then on they will inherit their properties and the alert and geofences configuration, so let's explain how to configure those features.

### Deleting an Asset Type 

Any resource can be deleted from the asset type by reasignandolos a un type vacío de la lista o bien accediendo a la sección settings del recurso y checking off the box, tras esto, las properties y los geofences heredados de su asset type serán borradas.

![](../.gitbook/assets/image%20%28327%29.png)

También es posible eliminar por completo un asset type seleccionando su perfil en la lista de Types y haciendo click en el botón "Delete Type" but note that ninguno de los recursos asignados serán eliminados, sólo dejarán de estar asociados a esta categoría. 

![](../.gitbook/assets/image%20%28330%29.png)

## Groups

Asset Groups: There are different devices with a common semantic characteristic for example the devices installed in the kitchen of a smart home can be grouped under the name "kitchen devices".

### Creating a new Asset Group

To create a new assets group go to the `Assets > Group` tab of the main menu, this will display the list of previously created Groups if any. Then, clicking on the green "Add Asset Type" button will open the "new Group" form,  in which the data must be introduced:

![](../.gitbook/assets/image%20%28339%29.png)

Once the form is completed, clicking again on the "Add Group" button will create a new Group profile and display its configuration panel in which the de devices overview map will be shown as well as Properties and Geofencing configuration tabs, that can be used to define the behavior of this features for all the devices included into this Assets Group.

![](../.gitbook/assets/image%20%28341%29.png)

Note that no devices will be shown until anyone begins attached to this specific device Group profile.

### Grouping devices

Start adding devices to the new Assets Group just require to get access to the devices section of the main menu and display the `Devices List`. Note that each device can be selected by means of a checkbox on the left side of the list. When doing this, new buttons appear on the top side of the menu, allowing to manage the device association into a specific Group, Asset or Project. 

![](../.gitbook/assets/image%20%28332%29.png)

Selecting one or several devices will display new buttons in the upper area of the list. Pressing the desired button allows to aggregate those devices to any of the previously created aggrupations. A new context will be displayed in order to select the Group from a drop-down list.

FOTO SET ASSET GROUP

Since the moment of the association, all the devices will inherit the properties and geofences configuration.

### Deleting Devices Group

## Asset Type Properties

Properties created in an Asset Type will be inherited by every associated device. This establishes a scalable way to configure large networks of devices in a short time and easily. The properties allow you to enter configuration parameters or any other context information you want to make available to the devices or other resources of the platform.

To create a new property on an Asset Type, go to `Asset > Types` section of the main menú, then acess the Type profile to be modified and select the properties tab on the right-side menu of the configuration panel. Then, creating a property only requires to press on `+Add` button and implement a Json file format to structure the data that wants to be included on the property.

![](../.gitbook/assets/image%20%28344%29.png)

Note that, on the device properties section, each property source will be identified on the "source" column.

![](../.gitbook/assets/image%20%28351%29.png)

## Geofencing  

Geofencing is a technique that allows defining a virtual perimeter over geographical areas, it can be defined as a radius around a point or tracing polygons. Thinger.io server will compare the location of any device included on an Asset Type and create an alert event when the devices enter or leave the indicated area.

![](../.gitbook/assets/image%20%28338%29.png)

Each defined geofence will create in the lower panel a profile that will allow to configure its behavior.

![](../.gitbook/assets/image%20%28331%29.png)



