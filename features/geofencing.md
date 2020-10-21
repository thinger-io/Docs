# GEOFENCING

## Geofencing  

Geofencing is a technique that allows defining a virtual perimeter over geographical areas, it can be defined as a radius around a point or tracing polygons. This is a very useful functionality to monitor automatically and in real-time a fleet of devices, allowing to create customized alerts based on geolocation.

Thinger.io server will compare the location of the device, create alerts according to the custom configuration that can be managed below. To start creating Geofence areas, the edition switch on the right-top-side of the map must be clicked ON. Then a new menu allows selecting the geofence shape that can drown circular, square, or free. 

### Creating new geofence

![](../.gitbook/assets/image%20%28341%29.png)

Each defined geofence will create in the lower panel a profile that will allow to configure its behavior. The next image shows the configuration options for the geofences created above for each situation in which any device can be located

![](../.gitbook/assets/image%20%28331%29.png)

* **name**: Identification of the geofence area
* **On Enter**: Allows selecting an endpoint that will be called automatically when the devices just **entered** the area, I.e. when the last position was out of the border and the new position is into the border. 
* **On Exit**: Allows selecting an endpoint that will be automatically called when the devices are just **leaving** the area, i.e when the last position was into the geofence area and the new position is out. 
* **While Inside**: This endpoint will be always called when the device is **within the area**. So it is preferable not to use this option if the device sampling interval is short
* **While Outside**: This endpoint will be always called when the device is **out of the area**. So it is preferable not to use this option if the device sampling interval is short
* **Color**: To select the color of the representation on the geofences map.
* **Enabled**: Each geofence can be disabled/enabled using the right-side switch. 

Note that, when a new geofence is created, the device \(or each device of the type/group\) will obtain two properties with the geofences configuration. These properties can be checked or edited in the device \(Asset/group\) properties section as shown below:

![](../.gitbook/assets/image%20%28363%29.png)

### Configuring Geofence output

Geofences output can be configured for each of the four possible states \(in, out, in, out\) through the lower menu, where the endpoint profiles created in the account can be selected. The endpoints are very versatile tools that allow configuring messages that leave the platform in a simple way, being able to program automatic behaviors. You can learn much more about the endpoints feature on the [Endpoints section](endpoints-1.md) of this documentation. 

![](../.gitbook/assets/image%20%28361%29.png)

To configure a geofence in a certain state you only need to click on the checkbox located on the left side and display the menu to choose the desired endpoint profile.

### Configuring Devices to work with Geofences

The device code must be prepared to make use of the platform's location services. It is only necessary to include among the variables sent to the platform the corresponding cardinal coordinates with the identifiers in their payload `"lat", "lng"`, `"lat", "long"` or `"latitude", "longitude"` as shown in the example of the callback of the HTTP devices:

![](../.gitbook/assets/image%20%28362%29.png)

{% hint style="warning" %}
The first release of this feature doesn't work with Thinger.io software client devices and MQTT devices. 
{% endhint %}

