# DASHBOARDS

Thinger.io dashboard system is a feature that allows creating nice data representation interfaces within minutes in a very simple way. No coding is required, just selecting different widgets from a list and using drag&drop technology to configure the layout of the dashboard, then using the configuration forms it is possible to set the data sources, sampling interval, and other behaviors of each widget. The main types of these widgets are: 

* **Real-time** data representation
* **Historical** data representation from buckets 
* **Control** device functions or change values with On/Off buttons or sliders

Here is an example dashboard with some widgets defined, like time series charts, donut charts, maps, or single values, but you can use many other ones

![](https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/c05197985d9ee92a9e12aaa71ab7508682bc3fbc.gif)

Once created, dashboards can be shared with third parties through a link or configured as templates to analyze data from different devices of the same type. In the following sections, we will explain how to create awesome dashboards in a few and very easy steps. Are you ready to create your own dashboard?

## Create a Dashboard

To manage all your dashboards, it is necessary to access to the `Dashboards` section, by clicking in the following menu item:

![](../.gitbook/assets/dashboardtab.PNG)

Then click on the `Add Dashboard` button that will open a new interface for entering the dashboard details, like in the following screenshot:

![](../.gitbook/assets/createdashboard.png)

It is necessary to configure different parameters:

* **Dashboard Id**: Unique identifier for your dashboard. 
* **Dashboard name**: A representative name of your dashboard, in a more friendly way than its identifier.
* **Dashboard description**: Fill here any description or detailed information you need to keep about the dashboard.

Clicking again on the "Add Dashboard" button, the new dashboard will be added to the account, and the browser will be automatically redirected to the empty board in order to start adding widgets as explained in the sections below.

## Edit Dashboard

By default, the dashboard appears empty and in view mode, so it is not possible to make modifications, so, to start working, the first step will be switching on the button on the upper-right corner of the dashboard that will change to the edition mode.

![](../.gitbook/assets/image%20%28378%29.png)

The dashboard edition mode allows moving, or resizing existing widgets, but also enables different options using the left-side buttons such as:

* **Add Widgets**:  To create new elements from the list. There are two different widget types depending on their objective: **Display widgets** allow to show real-time data from devices or historical data from buckets, and **Control Widgets** allows to connect the dashboard with devices functionalities in order to control them in real-time. 
* **Add Tab**: This button allows to create an additional dashboard tab that will appear associated to the original one in order to provide simple navigation over related boards. 
* **Settings**: There are multiple parameters that can be configured in order to set the dashboard behavior, such as the number of columns, the background image or the sharing options.  

These three options have been explained in more detail in the sections below.

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

![](../.gitbook/assets/image%20%28170%29.png)

{% hint style="warning" %}
Note that Data Aggregation system is only available in **private server** instances with **InfluxDB** 
{% endhint %}

### Tachometer Chart

It is a quite visual widget that allows showing device data in a traditional "dial gauge" representation, that can be customized with different value ranges and color marcs, making it more accurate or simplifying the simpection with just a glance.

![](../.gitbook/assets/iot-tachometer.PNG)

The configurable parameters are the following:

![](../.gitbook/assets/image%20%2877%29.png)

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\). This widget has a particularity behavior in relation to this parameter. Pressing into the green "+" button, It is possible to select different background colors depending on the real time value that is being shown: 

![](../.gitbook/assets/image%20%2886%29.png)

This image is representing an example in which the measured variable is reaching dangerous pressure values. According to this situation, the background color is changing to red, so it will be easier to identify and manage the event if there is not any automatic system in the product.  

![](../.gitbook/assets/image%20%2868%29.png)

* **Chart Input**: Configure how to feed the values to the tachometer chart. It is possible to feed the information from a connected **device** or from a **data bucket**
  * **From Device Resource**: With this option it is necessary to select a device \(that must be connected to provide information\) and specify the resources to plot. The following figure is an example that is selecting the device `deviceA`, and the resource `millis` from the device. Notice that when a time series widget is feed from a device, it will not keep the information if the dashboard is closed or refreshed, as it is just real-time data from your device to your dashboard. You can also select between different refresh modes, like sampling at different intervals \(that can be updated online\), or the chart is updated by the device.
  * **From Device Property:** This option allows retrieving data from device properties, which is really useful to show device configuration data, but also is the better way to show real time \(or last received\) data from HTTP devices. 
  * **From Data Bucket**: With this option, the widget will take the information from a given bucket to plot the historic information on it. So, it is necessary to just select the bucket identifier created in your account. If the bucket is composed by multiple variables, it will allow selecting the variables to plot, like in the following picture. When the information is selected from the data bucket, you will require to establish a data timeframe to be displayed, that can be relative to the current time, or an absolute period between two dates.
  * **Manual Data**: It is always possible to manually introduce values in order to create simulate the behavior of the widget.

The last tab shows all the display options. This is probably the most customizable widget of Thinger.io Platform. It allows selecting a lot of different parameters as shown in the image below: 

![](../.gitbook/assets/image%20%28213%29.png)

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

![](../.gitbook/assets/image%20%28234%29.png)

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

![](../.gitbook/assets/image%20%2810%29.png)

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

### HTML Widget 

This widget allows creating custom data representation interfaces by programming it with standard web source code languages such as HTML, CSS, JS. Being also able to represent data from Thinger.io devices or data buckets or show data from third party sources into the same dashboard. 

To start using the HTML widget, first step is configure the common Thinger.io widget parameters, such as:

* **Title**: Optional title for the widget. 
* **Subtitle**: Optional subtitle for the widget.
* **Background**: Optional color for the widget background \(defaults to white\).

Then, selecting HTML widget type, it is possible to introduce the data source, that can be a manual value, a [**Device Resource**](https://docs.thinger.io/coding#adding-resources) or a [**Data Bucket**](buckets.md). And finally, it is possible to define a display HTML source.

#### From Snipped Code

To create a basic widget with a simple code such as a data table,  a ready-to-paste script from any website, or any other easy integration. The source code can be wrote using the small text editor in the widget form. Note that this code will be executed in the browser's [**Node.JS**](https://nodejs.org/es/docs/) console and that the HTML body context has been defined previously so only should directly enter the elements you want to represent, as shown in the examples below:

{% tabs %}
{% tab title="Basic Code Sniped" %}
![](../.gitbook/assets/image%20%2898%29.png)

Note that the value specified in "data source" can be retrieved in the console by using {{value}} command
{% endtab %}

{% tab title="Widget Script" %}
![](../.gitbook/assets/image%20%28152%29.png)

Next script can be used as example to create an HTML widget with another weather forecast provider:

```text
<div id="c_c1374694634f9f99525990d7fe6508ae" class="ancho"></div><script type="text/javascript" src="https://www.eltiempo.es/widget/widget_loader/c1374694634f9f99525990d7fe6508ae"></script>
```

![](../.gitbook/assets/image%20%28233%29.png)
{% endtab %}
{% endtabs %}

#### From File Storage: 

For more complex developments over the HTML Widget where several source code files are required, it is possible to use  to use our [**File System**](https://docs.thinger.io/console/file-system). This allows the development of more complex interfaces that exploit all the representation capabilities of the browser such as 3D object representation, animated widgets, etc.



{% hint style="info" %}
This is work in progress, we will add more documentation soon. Sorry for the inconveniences 
{% endhint %}

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

This widget has the possibility to be shown in two different appearances, that can be specified in the **Switch Style** parameter: **Switch** is the standard configuration with a little non-configurable switch, and **Button** which is an improved face that can be configured with different colors and icons. When this option is selected, next paremeters will be shown:

![](../.gitbook/assets/image%20%28235%29.png)

* **On Color**: The color that will be displayed when the boolean value of this resource is true.
* **Off Color**: The color that will be displayed when the boolean value of this reource is false.
* **Icon**: This button is able to show a customizable icon from favicon library or any other icon library URL.
* **Icon Color**: Icon color is also configurable with an hexadecimal value. Note that there are different color options for both button status, so you can customize it as you want.

  ![](../.gitbook/assets/image%20%28144%29.png)

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

## Dashboard Tabs

A Dashboard Tab is an additional work page that can be added to a dashboard to organize the visualization of data and simplify navigation between related pannels. The widgets and data sources of each tab can be completely independent of the others but all the tabs will share the same configuration settings \(column number, background image, widgets border-radius, etc\). 

This feature also has the advantage of keeping all the tabs of a dashboard open even if they are not being visualized, so the data of the devices shown in real-time will not be lost when changing from one tab to another.

![](../.gitbook/assets/image%20%28377%29.png)

### Adding a new tab

To add a tab to a dashboard you only need to click on the blue "add tab" button, this button can be clicked as many times as tabs are desired to create, they will be labeled as "new tab" and will show a generic icon. The label can be modified just by typing a new name, but the icon can also be customized by pressing over the existent one, deploying a menu with every available icon.

![](../.gitbook/assets/image%20%28375%29.png)

## Dashboard Settings

El sistema de dashboards permite setear algunos parametros de la configuración de los tableros. Para acceder al menú de settings es necesario activar el switch de edición del dashboard y entonces hacer click en el botón azul "Settings" de la zona superior del dashboard. 

![](../.gitbook/assets/image%20%28373%29.png)

The Settings menu allows to custom the next parameters of a dashboard:

* **Name**: Dashboard name that will be shown on the header and browser tab
* **Description**: Dashboard description for additional information 
* **Columns**: This number allows selecting the logical amount of horizontal places that can be used to insert widgets on the dashboard. Each widget can be scaled to fill more than one position. 
* **Background image**: The background image can be selected in order to custom dashboard appearance 
* **Border radius**: widget corners will be rounded according to this parameter
* **Hide Header**: Shared dashboards have been provided with a header that shows the dashboard name, but this header can be hidden if the developer switches this option on. 
* **Make template**: To enable template feature \(as explained below\)
* **Share Dashboard**: To create a dashboard link and authorization in order to share it with thirds

### Share Dashboard

By default, any dashboard is private to the account owner. However, it is possible to share the dashboard so others can access your information. To share a dashboard, just enter in the dashboard config and enable the `Share` switch. After enabling the dashboard sharing, an URL will be generated, which can be publicly shared.

{% hint style="info" %}
Any modification on a shared dashboard widget that includes new device or data bucket resources must be updated in the authorization by means of the Access Tokens menu or by re-generating the link by turning the shared dashboard option off and on again. 
{% endhint %}

![](../.gitbook/assets/sharedashboard.png)

## 

