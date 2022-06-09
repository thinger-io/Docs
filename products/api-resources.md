# API Resources

One of the most interesting features of products is the possibility to create custom API resources for devices, that is, create custom API endpoints associated to each device. This is specially useful for MQTT devices, as they will behave exactly the same as IOTMP devices, so they can be used directly on dashboards, use the API explorer, etc.

Following our previous examples with the Shelly Smart Plug S, makes sense to create an endpoint to switch on/off the relay. The approach for building the API Resources is quite similar to the previous sections regarding properties and buckets. Just click on the 'Add' button on the API Resources section and start setting an identifier, like `relay`.

Then, there are two sections inside the API Resource. The first one is the `Request`, where it is configured the request that will be sent to the device, and the other one is the `Response`, that configures the response that will be sent to the API Rest client that originates the request.

In this case, we want to send a command to the Shelly Device, so, it is required to configure the Request section with `Device Topic` in Target field, as we are interacting with a MQTT device. Then, in the topic field, we can set the following topic, as described in the [vendor documentation](https://shelly-api-docs.shelly.cloud/gen1/#shelly-plug-plugs-overview):&#x20;

```
shellies/{{device}}/relay/0/command
```

Then we must set the message payload, that must be 'on' or 'off' depending on the desired state. If we configure `{{payload}}` as the Request payload, then, any body sent to the the API will be transmitted to the device. The standard 'on'/'off' representation in JSON is true/false, so, we can configure a processing function to convert an input JSON boolean to required values by the device. In the Product Script section we can add a function like:

```javascript
function toOnOff(value){
    return value ? "on" : "off";
}
```

As we did on [Properties](properties.md) and [Buckets](buckets.md), we can configure the API Resource payload to call this function to transform the incoming payload.

```
{{payload:toOnOff}}
```

After saving the new API Resource, we can go to the device API, and the new resource will appear.

![Device API for resource created over Product ](<../.gitbook/assets/image (462).png>)

However, there seems to be something wrong there! The API can detect that the resource is expecting an input (Resource Input), but nothing more appears. We are expecting here a true/false input, or just a switch to turn the relay on and off.&#x20;

This can be solved by assigning a default payload value on the API definition, which let the API explorer know the expected values by default. This can be done changing the API Resource payload to something like this:

```
{{payload:toOnOff=false}}
```

Now, refreshing the API explorer will display the relay resource expecting a boolean that can be easily changed over the GUI. At this moment, it is possible to turn on and off the switch button. It will call the API Resource on the product, that will convert the incoming payload to 'on' and 'off', and then will be transmitted to the required topic for the Shelly Plug S device. Cool! :sunglasses:

![Device API for resource created over Product ](<../.gitbook/assets/image (453).png>)

However, this use case can be improved a little bit. For example, suppose that it is required to display the current relay state to avoid showing always the default 'false' value set before. For this purpose we can reference other properties, or even API responses. To reference a property, i.e., the 'relay' one configured on the [Properties](properties.md) section, we can set a default value with `=property.<property_name>` like in the following example:

```
{{payload:toOnOff=property.relay}}
```

Then, the next time the API explorer, or a dashboard, 'inspect' the device resource, will obtain the value stored on the property. In this case, the property is updated periodically with the current relay status, or when the status is changed, even with the psychical button.
