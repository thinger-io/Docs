# BUCKETS

A data bucket is some kind of virtual storage where you can keep time series information, like temperature or humidity over time. But it is possible to use them to store any other event, like motion detections, garage door opens, temperature warnings, and so on.

This information can be used to plot information in dashboards, or can be exported in different formats for offline processing.

## Create Bucket

To create a data bucket, you need to access the `Data Buckets` feature, by clicking on this section:

![](../.gitbook/assets/bucketstab.PNG)

To create the bucket, just press in the **Add Bucket** button, which will show the following screen:

![](../.gitbook/assets/addbucket.PNG)

Here it is necessary to configure different parameters:

* **Bucket Id**: Unique identifier for your bucket. 
* **Bucket name**: Use a representative name to remember the bucket scope, like `WeatherData`.
* **Bucket description**: Fill here any description with more details, like Temperature and humidity in house.
* **Enabled**: Data bucket recording can be enabled or disabled. Just switch it on to enable it.
* **Data source**:
  * **From Device Resource**: It means that it will take information from a specific device resource \(like temperature, motion, and so on\). In this option, the device should keep a permanent connection with the server. This add some benefits as we can change the sampling rate on demand, without having to change our device code, by using the `Sampling Interval` option.



    **Remember** that defining a resource in the device is described in more detail [here](http://docs.thinger.io/arduino/#coding-adding-resources), but a single resource reporting temperature and humidity from a DHT sensor could be coded like this:

    ```text
    // define the resource just once in the setup
    thing["TempHum"] >> [](pson &out){ 
      out["temperature"] = dht.readTemperature();
      out["humidity"] = dht.readHumidity();
    };
    ```

    It is also possible to let he device stream the information when required, i.e., by raising an event when detected. In this case, we can use the `Update by Device` option while configuring the bucket, and use the streaming resources as described [here](http://docs.thinger.io/arduino/#coding-streaming-resources).

    Using the previous `TempHum` sample resource, it could be done like in the following code snippet.

    ```cpp
    void loop() {
      thing.handle();
      // use your own logic here to determine when to stream/record the resource.
      if(requires_recording){
          thing.stream("TempHum");
      }
    }
    ```

    This way, the data bucket is subscribed to a device resource, and its information is registered in every stream call.

  * **From Write Call**: This option will allow setting the bucket in a state that it will not register any information by default, but it will just wait for writing calls, both from the Arduino library using the `write_bucket` method, as shown [here](http://docs.thinger.io/hardware/climaStick/#quickstart-examples-data-recording-using-sleep), or calling the REST API directly like done with [Sigfox](http://docs.thinger.io/sigfox/#steps-in-thingerio-create-a-data-bucket). This feature opens the option to register information in the same bucket from different devices, or store information from devices that are not connected permanently with the server, that are in sleep mode, or use a different technology like Sigfox.

    Here is an example of an ESP8266 device writing information to a bucket using the `write_bucket` function:

    ```cpp
    void setup() {
      // define the resource with temperature and humidity
      thing["TempHum"] >> [](pson &out){ 
        out["temperature"] = dht.readTemperature();
        out["humidity"] = dht.readHumidity();
      };
    }

    void loop() { 
      // handle connection
      thing.handle();
      // write to bucket BucketId the TempHum resource
      thing.write_bucket("BucketId", "TempHum");
      // sleep the device SLEEP_MS milliseconds
      ESP.deepSleep(SLEEP_MS*1000, WAKE_RF_DEFAULT); 
    }
    ```

![](../.gitbook/assets/buckettimesample.PNG)

## Review Bucket Data

Once the data bucket has been configured, and it started to record data from a device or from write calls, it will display the information inside a table. Every record contains the server timestamp in UTC \(but shown in local time zone in the console\), and the record value. The value stored in the data bucket can be a single value, or any other JSON document. If the JSON document is composed by key-value pairs, like in the previous examples, they will be displayed in tabular format, just like in the following screenshot.

![](../.gitbook/assets/iotbucketdata.png)

## Clear Bucket Data

Sometimes it can be useful to clear the bucket information without deleting the whole bucket, creating and configuring it again. Therefore, you can clear the bucket, or a part of them easily from the bucket page. In the clear process, the bucket can still record information from your devices.

![](../.gitbook/assets/data-bucket-clear.png)

Data bucket profiles can also be deleted from the data bucket list, by selecting the profiles to be deleted and pressing the red "Remove Bucket" button as shown in the image below:

![](../.gitbook/assets/image%20%2870%29.png)

## Export Bucket Data

It is possible to export all your stored information in different file formats, so you can process the data offline, like applying Artificial Intelligence, Business Analytics, Big Data, etc. In this way, you can access your bucket and configure the export process:

![](../.gitbook/assets/dowloadbucket.PNG)

 The configurable parameters are:

* **Data format:** To obtain CSV, ARFF or JSON format file
* **Time format:**  Timestamp or ISO date format
* **Export range:** This section allows downloading the complete data bucket or selecting a custom range. 

Onces the export data range and format has been select, the system will create a download link that will be stored in the "Export List" section below. This links can be used to provide customers of custom data reports form the IoT data.

![](../.gitbook/assets/image%20%2853%29.png)

{% hint style="info" %}
if traditional Thinger.io Server is used, you will receive an email after few minutes with a download to your file \(valid for 3 months in the default cloud console\).
{% endhint %}

## 

