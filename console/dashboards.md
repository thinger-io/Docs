# DASHBOARDS

A dashboard is a graphical user interface that allows displaying your information in different figures and charts. You can configure the dashboards with different widgets, configure its layout, dimension, color, and data sources to generate valuable information for your business or processes.

The dashboards can **display information in real-time** from your devices \(using websockets over the server for minimum latency\), or use historic information stored in data buckets that is polled periodically. It is possible to configure the data source for each dashboard widget independently. For devices connected to the platform, it is even possible to dynamically configure the sampling interval for each resource, i.e., in a resource defined from sensor readings, it will allow adjust its physical sampling interval and transmission over the wire. The dashboards are not just only for displaying data, but can also **actuate in real-time** over your connected devices, so you can use some control widgets like on/off values or sliders.

Here is a sample dashboard with some widgets defined, like time series charts, donut charts, maps, or single values, but you can use many other ones

![](https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/c05197985d9ee92a9e12aaa71ab7508682bc3fbc.gif)

Ready to create your own dashboard?

## Create Dashboard

To manage all your dashboards, it is necessary to access to the `Dashboards` section, by clicking in the following menu item:

![](../.gitbook/assets/dashboardtab.PNG)

Then click on the `Add Dashboard` button that will open a new interface for entering the dashboard details, like in the following screenshot:

![](../.gitbook/assets/createdashboard.png)

Here it is necessary to configure different parameters:

* **Dashboard Id**: Unique identifier for your dashboard. 
* **Dashboard name**: A representative name of your dashboard, in a more friendly way than its identifier.
* **Dashboard description**: Fill here any description or detailed information you need to keep about the dashboard.

After this process, it is possible to access to the new dashboard, that will appear empty by default.

## Edit Dashboard

By default, the dashboard appears in viewing mode, where you cannot modify or configure he dashboard, however, there is an edit mode that can be easily enabled by clicking on the upper-right switch of the dashboard. So, enable the edit mode every time you need to add, move, or resize a widget. The edit mode also enables different options like sharing dashboards.

![](../.gitbook/assets/emptydashboard.PNG)

## Add a Display Widget

When the edit mode is enabled in the dashboard, a new button called `Add Widget` will appear. Clicking on it will show a popup where it is possible to select the widget type to add in the dashboard. There are different widgets both for displaying information, or control connected devices, just like in the following picture:

![](../.gitbook/assets/widgettypes.PNG)

The following subsections describes the different parameters for each widget type.

### Time Series Chart

A time series chart is a graph that can display values over time. In this sense, this is quite useful when it is required to display time series data, like temperature variable that changes over time. It is possible to plot a single variable or multiple values in the same chart. The initial configuration of this widget is like shown in the following figure:

![](../.gitbook/assets/timeserieschart.png)

The configurable parameters are the following:

![](../.gitbook/assets/timeserieschartwidget.png)

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\).
* **Chart Input**: Configure how to feed the values to the time series chart. It is possible to feed the information from a connected **device** or from a **data bucket**
  * **From Device**: With this option it is necessary to select a device \(that must be connected to provide information\) and specify the resources to plot. The following figure is an example that is selecting the device `deviceA`, and the resource `millis` from the device. Notice that when a time series widget is feed from a device, it will not keep the information if the dashboard is closed or refreshed, as it is just real-time data from your device to your dashboard. You can also select between different refresh modes, like sampling at different intervals \(that can be updated online\), or the chart is updated by the device.

  * **From Data Bucket**: With this option, the widget will take the information from a given bucket to plot the historic information on it. So, it is necessary to just select the bucket identifier created in your account. If the bucket is composed by multiple variables, it will allow selecting the variables to plot, like in the following picture. When the information is selected from the data bucket, you will require to establish a data timeframe to be displayed, that can be relative to the current time, or an absolute period between two dates.

![](../.gitbook/assets/datasource.PNG)

* **Options**: It is possible to configure some graph features like splines, legends, axis, etc.
* **Chart Color**: Both on data selected from a device or from a data bucket, it is possible to configure series colors, depending on the information available in the resource, it will show only one configurable color, or a color for each series, like in the previous screenshot.

![](../.gitbook/assets/multiplevariable.PNG)

* **Data Aggregation**: 

Show raw data directly from a Bucket could be tricky when there is a lot of data-points, specially if the measures are very noisy or irregular. This feature allows aggregating data using different statistics such as medians, means, minimum and maximum values, a counter of data points per period and a data sumatory. The aggregation can be applied over different intervals that goes from five minutes to one week, by using the next configuration inputs in the widget form, and also using the upside right parameters on each time series chart widgets.

![](../.gitbook/assets/iot-data-aggregation.PNG)

The next image shows four different representations of the same dataset and time interval, aggregated using different algorithms:  

![](../.gitbook/assets/image%20%28130%29.png)

{% hint style="warning" %}
Note that Data Aggregation system is only available in **private server** instances with **InfluxDB** 
{% endhint %}

### Tachometer Chart

It is a quite visual widget that allows showing device data in a traditional "dial gauge" representation, that can be customized with different value ranges and color marcs, making it more accurate or simplifying the simpection with just a glance.

![](../.gitbook/assets/iot-tachometer.PNG)

The configurable parameters are the following:

![](../.gitbook/assets/image%20%2858%29.png)

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\). This widget has a particularity behavior in relation to this parameter. Pressing into the green "+" button, It is possible to select different background colors depending on the real time value that is being shown: 

![](../.gitbook/assets/image%20%2866%29.png)

This image is representing an example in which the measured variable is reaching dangerous pressure values. According to this situation, the background color is changing to red, so it will be easier to identify and manage the event if there is not any automatic system in the product.  

![](../.gitbook/assets/image%20%2851%29.png)

* **Chart Input**: Configure how to feed the values to the tachometer chart. It is possible to feed the information from a connected **device** or from a **data bucket**
  * **From Device Resource**: With this option it is necessary to select a device \(that must be connected to provide information\) and specify the resources to plot. The following figure is an example that is selecting the device `deviceA`, and the resource `millis` from the device. Notice that when a time series widget is feed from a device, it will not keep the information if the dashboard is closed or refreshed, as it is just real-time data from your device to your dashboard. You can also select between different refresh modes, like sampling at different intervals \(that can be updated online\), or the chart is updated by the device.
  * **From Device Property:** This option allows retrieving data from device properties, which is really useful to show device configuration data, but also is the better way to show real time \(or last received\) data from HTTP devices. 
  * **From Data Bucket**: With this option, the widget will take the information from a given bucket to plot the historic information on it. So, it is necessary to just select the bucket identifier created in your account. If the bucket is composed by multiple variables, it will allow selecting the variables to plot, like in the following picture. When the information is selected from the data bucket, you will require to establish a data timeframe to be displayed, that can be relative to the current time, or an absolute period between two dates.
  * **Manual Data**: It is always possible to manually introduce values in order to create simulate the behavior of the widget.

The last tab shows all the display options. This is probably the most customizable widget of Thinger.io Platform. It allows selecting a lot of different parameters as shown in the image below: 

![](../.gitbook/assets/image%20%28161%29.png)

* **Display options:**
  * **Units**: Optional information that will display the variable unit, like ºC.
  * **Value Ranges**: This parameter configures the total data range that will be shown at the chart, and also allows adding sub-ranges that can be configured with different colors in order to simplify the visual checking.
  * **Plate Color**: Configure the background plate color.
  * **Text Color**: Configure the text color.
  * **Tick Color**: Configure the division ticks color. 
  * **Major Ticks**: Allows to configure the range of each tick
  * **Show Value**: To display or hide the numeric representation of the value in a digital textbox.

### Virtual LED

Using LED spots is a common way  to create simple graphical interfaces in electronic projects in order to represent system status, alerts, etc. This widget has been included in Thinger.io Platform with the same purpose, so it can be used to show binary status by changing its color, create alerts by setting blink behavior or show multiple data by including more than one color range in a kind of RGB simulation. 

![](../.gitbook/assets/image%20%28175%29.png)

This widget can be configured in many different ways though the three steps form. first of all selecting "Led indicator" in the Widget menu tab, and then indicating:

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\). This widget has a particularity behavior in relation to this parameter. Pressing into the green "+" button, It is possible to select different background colors depending on the real time value that is being shown: 

Then, the Led indicator menu tab allows selecting the data source, that can be a connected device or a data bucket:

* **Chart Input**: Configure how to feed the values to the tachometer chart. It is possible to feed the information from a connected **device** or from a **data bucket**
  * **From Device Resource**: With this option it is necessary to select a device \(that must be connected to provide information\) and specify the resources to plot. The following figure is an example that is selecting the device `deviceA`, and the resource `millis` from the device. Notice that when a time series widget is feed from a device, it will not keep the information if the dashboard is closed or refreshed, as it is just real-time data from your device to your dashboard. You can also select between different refresh modes, like sampling at different intervals \(that can be updated online\), or the chart is updated by the device.
  * **From Device Property:** This option allows retrieving data from device properties, which is really useful to show device configuration data, but also is the better way to show real time \(or last received\) data from HTTP devices. 
  * **From Data Bucket**: With this option, the widget will take the information from a given bucket to plot the historic information on it. So, it is necessary to just select the bucket identifier created in your account. If the bucket is composed by multiple variables, it will allow selecting the variables to plot, like in the following picture. When the information is selected from the data bucket, you will require to establish a data timeframe to be displayed, that can be relative to the current time, or an absolute period between two dates.
  * **Manual Data**: It is always possible to manually introduce values in order to create simulate the behavior of the widget.

Finally, the "Display Options" tab allows to custom the led behavior in the next parameters:

* **Led Size**: Configure the diameter of the led spot pixels
* **Color**: configures the led default color, and also allows creating color ranges by pressing the green "+" button on the right side.
  * **Color ranges**: Each time that the "+" button is pressed, a new color range is included, allowing to define a new range and the color that will be shown when the selected input value belongs to this range.  
  * **Blinking led option:** The right side switches allows adding a blinking behavior to the led when this range profile begins active. It is also possible to disable the blinking by pressing over the led widget. 

![](../.gitbook/assets/image%20%286%29.png)

### Donut Chart

A donut chart is a graph that can display a value, normally in form of a rounded percentage. In this sense, this is quite useful when you have a know variable that oscillates between a maximum and minimum value. In this case, it is only possible to only represent a single variable, that can be both updated in real-time from a device, or from a data bucket.

![](../.gitbook/assets/donutchart.png)

The configurable parameters are the following:

![](../.gitbook/assets/donutchartwidget.png)

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\).
* **Donut Value**: Configure how to feed the donut char value. It is possible to feed the information from a connected **device** or from a **data bucket**, in a similar way as the time series chart.
* **Units**: Optional information that will display the variable unit, like ºC.
* **Min Value**: The expected minimum value of the variable.
* **Max Value**: The expected maximum value of the variable.
* **Donut Color**: The color to display inside the donut.

### Progressbar

A progressbar is a graph that can easily represent a progress on some action or process. In this sense, this is quite useful when you have any process that is being completed over time and needs to be monitored. In this case, it is only possible to only represent a single variable, that can be both updated in real-time from a device, or from a data bucket.

![](../.gitbook/assets/progressbar.png)

The configurable parameters are the following:

![](../.gitbook/assets/progressbarwidget.png)

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\).
* **Progressbar Value**: Configure how to feed the progressbar value. It is possible to feed the information from a connected **device** or from a **data bucket**, in a similar way as the time series chart.
* **Units**: Optional information that will display the variable unit, like %.
* **Min Value**: The expected minimum value of the variable.
* **Max Value**: The expected maximum value of the variable.

### Google Maps

A map can be used to represent, at this moment, a single location in a map. It is quite convenient to track devices in real-time as the chart can be feed in real-time from a connected device, like over a GPRS connection. It is also possible to plot locations from a data bucket, so devices like Sigfox can be also be tracked.

![](../.gitbook/assets/googlemap.png)

Here is an example of this widget working in real-time with a connected device:

 \[!\[Real-Time GPS location over GPRS using IoT Solution\]\(https://img.youtube.com/vi/3QDDOPMg22g/0.jpg\)\]\(https://www.youtube.com/watch?v=3QDDOPMg22g\)

The configurable parameters are the following:

![](../.gitbook/assets/googlemapwidget.png)

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\).
* **Location**: Configure how to feed the location in the map. It is possible to feed the information from a connected **device** or from a **data bucket**. When feeding the plot from a data bucket or a device, it is required to match the required latitude and longitude \(in degrees\) with the variables present in the bucket, or in the device resource.

![](../.gitbook/assets/locationvalue.png)

* **Center**: Force the map to automatically keep the location in the center.

### Image/MJPEG

The image/MJPEG widget can be used to represent both a still image, like your business logo, or a live stream from a MJPEG source, like a surveillance camera. To feed this widget it is necessary the image/MJPEG url.

![](../.gitbook/assets/cameramjpeg.png)

The configurable parameters are the following:

![](../.gitbook/assets/imagewidget.png)

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\).
* **Image Source**: Configure if the image source is a still image, or a MJPEG stream. In both cases it is required to provide the source URL, like in the following screenshot:

![](../.gitbook/assets/mjpegcamera.png)

### Text/Value

The text/value widget is an useful widget to display any arbitrary data, specially text values that cannot be represented with other widgets. As any other widget, can display data both from connected devices or data buckets.

![](../.gitbook/assets/textvalue.png)

The configurable parameters are the following:

![](../.gitbook/assets/textwidget.png)

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\).
* **Text Value**: As any other widget, it is possible to select a resource from a connected device, or a value from a data bucket.
* **Units**: Optional information to display the units of the displayed information.
* **Text Color**: Configure the text color.

### Clock

This widget is just a clock widget that can display the current time both in the local time zone or in UTC, which can be useful when monitoring processes in real-time. Note that this widget takes the current time just from your computer.

![](../.gitbook/assets/clock.png)

The configurable parameters are the following:

![](../.gitbook/assets/clockwidget.png)

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\).
* **Color**: Color for the text.
* **UTC**: Display the clock in UTC or in the local timezone.

## Add a Control Widget

In Thinger.io it is possible to not just display information in dashboard, but also control devices in real-time. In this section are described some available widgets that can be used control connected devices.

### On/Off State

The On/Off widget allows controlling a boolean state of a connected device, like turning on/off a light, a motor, a relay, or any other element. The device should expose a boolean input, just like those examples for controlling a led. The resource is then mapped to this widget, that can change the device state in real-time. If the input resource is defined properly [implemented](http://docs.thinger.io/arduino/#coding-adding-resources-input-resources), this widget is also able to show the current device state.

 ![](../.gitbook/assets/switchbutton.png) 

The configurable parameters are the following:

![](../.gitbook/assets/booleanwidget.png)

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\).
* **Device Resource**: Determines the specific device and resource to control. Use a connected device for an easy config, as you can automatically select the device and resource.

![](../.gitbook/assets/deviceresource.png)

This widgect has te posibility to be shown in two different appearances, that can be specified in the **Switch Style** parameter: **Switch** is the standard configuration with a little non-configurable switch, and **Button** which is an improved face that can be configured with different colors and icons. When this option is selected, next paremeters will be shown:

![](../.gitbook/assets/image%20%28176%29.png)

* **On Color**: The color that will be displayed when the boolean value of this resource is true.
* **Off Color**: The color that will be displayed when the boolean value of this reource is false.
* **Icon**: This button is able to show a customizable icon from favicon library or any other icon library URL.
* **Icon Color**: Icon color is also configurable with an hexadecimal value. Note that there are different color options for both button status, so you can customize it as you want.

  ![](../.gitbook/assets/image%20%28111%29.png)

### Slider

The slider widget allows controlling a numeric state of a connected device, like setting a threshold, a target temperature, or any other internal device state that is likely to be controlled remotely. The device should expose a numeric input. The resource is then mapped to this widget, that can change the target value in real-time. If the input resource is defined properly [implemented](http://docs.thinger.io/arduino/#coding-adding-resources-input-resources), this widget is also able to show the current device state.

![](../.gitbook/assets/slider.png)

The configurable parameters are the following:

![](../.gitbook/assets/sliderwidget.png)

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\).
* **Device Resource**: Determines the specific device and resource to control. Use a connected device for an easy config, as you can automatically select the device and resource.
* **Min Value**: Maximum value of the slider.
* **Max Value**: Minimum value of the slider.
* **Step Width**: Slider precision.

## Share Dashboard

By default, any dashboard is private to the account owner. However, it is possible to share the dashboard so others can access your information. To share a dashboard, just enter in the dashboard config and enable the `Share` switch. After enabling the dashboard sharing, an URL will be generated, which can be publicly shared.

**Note:** The generated authorization \(appended to the end of the URL\) can be used to partially access your resources, like devices or buckets used to feed the charts. Review the generated access token for more details.

**Note:** If you change your dashboards by adding new data sources \(devices or buckets\), it is necessary to disable an re-enable the dashboard sharing to update the authorization. It will not share new data sources automatically for security reasons.

![](../.gitbook/assets/sharedashboard.png)

## 

