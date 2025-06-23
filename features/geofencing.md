# GEOFENCING

## Geofencing &#x20;

Geofencing is a technique that allows defining a virtual perimeter over geographical areas, it can be defined as a radius around a point or tracing polygons. This is a very useful functionality to monitor automatically and in real-time a fleet of devices, allowing the creation of customized alerts based on geolocation.

Thinger.io server will compare the location of the device, create alerts according to the custom configuration that can be managed below. To start creating Geofence areas, the edition switch on the right-top-side of the map must be clicked ON. Then a new menu allows selecting the geofence shape that can drown circular, square, or free.&#x20;

### Creating a new geofence

![](<../.gitbook/assets/image (357).png>)

Each defined geofence will create in the lower panel a profile that will allow configuring its behavior. The next image shows the configuration options for the geofences created above for each situation in which any device can be located

![](<../.gitbook/assets/image (373).png>)

* **name**: Identification of the geofence area
* **On Enter**: Allows selecting an endpoint that will be called automatically when the device just **entered** the area, i.e. when the last position was out of the border and the new position is within the border.&#x20;
* **On Exit**: Allows selecting an endpoint that will be automatically called when the devices are just **leaving** the area, i.e when the last position was within the geofence area and the new position is out.&#x20;
* **While Inside**: This endpoint will always be called when the device is within the area. So, it is preferable not to use this option if the device sampling interval is short
* **While Outside**: This endpoint will always be called when the device is out of the area. So, it is preferable not to use this option if the device sampling interval is short
* **Color**: To select the color of the representation on the geofences map.
* **Enabled**: Each geofence can be disabled/enabled using the right-side switch.&#x20;

Note that, when a new geofence is created, the device (or each device of the type/group) will obtain two properties from the geofences configuration. These properties can be checked or edited in the device (Asset/group) properties section:

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

### Configuring Geofence output

Geofences output can be configured for each of the four possible states (in, out, in, out) through the lower menu, where the endpoint profiles created in the account can be selected. The endpoints are very versatile tools that allow configuring messages that leave the platform in a simple way, allowing the programming of automatic behaviors. Learn much more about the endpoints feature in the [Endpoints section](endpoints-1.md) of this documentation.&#x20;

![](<../.gitbook/assets/image (349).png>)

To configure a geofence in a certain state, click on the checkbox located on the left side and display the menu to choose the desired endpoint profile.

### Configuring Devices to work with Geofences

The device code must be prepared to make use of the platform's location services. It is only necessary to include among the variables sent to the platform the corresponding cardinal coordinates with the identifiers in their payload `"lat", "lng"`, `"lat", "long"` or `"latitude", "longitude"` as shown in the example of the callback of the HTTP devices:

![](<../.gitbook/assets/image (379).png>)

{% hint style="warning" %}
The first release of this feature doesn't work with Thinger.io software client devices and MQTT devices.&#x20;
{% endhint %}
