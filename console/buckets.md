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
* **Data source:** This parameter allows setting the behavior of the data bucket by selecting the data source and also the sampling method. As there are many different options, this feature is detailed in the section below: 

The sections below explains the different data bucket data acquisition modes and timing configurations: 

### **From Device Resource**

 This option subscrives Thinger.io Server to an specific device resource \(such as temperature, motion, and so on\). It can be configured to retrieve data from the device in an specific sampling interval, or wait asynchronous communications from devices by means of the "Refresh mode" parameter.  
  
__Note that this option is only compatible with devices that has been provided with Thinger.io Software client libraries \(Arduino, Linux or Raspberry\), and it will only work properly if the device keeps permanent connection with the server.

* **Sampling interval:**  Configure the bucket profile to retrieve data from device resources in an specific timing, that can be changed on demand, without modifying device sketch. Other benefit is that no additional codification is needed to implement this feature and start storing data. The next basic code example will store two variables in the data bucket when using the "sampling interval" configuration.

```
// define the resource just once in the setup() section

thing["TempHum"] >> [](pson &out){ 
  out["temperature"] = dht.readTemperature();
  out["humidity"] = dht.readHumidity();
};
```

* **Update by Device:**  This options allows the device to stream the information when required, i.e., by raising an event when detected. In this case, refresh mode must be set at the `Update by Device` option while configuring the bucket, and the device source code will contain an streaming instruction for the resources as shown below \(also described in more detail [**here**](http://docs.thinger.io/arduino/#coding-streaming-resources)\). This way, the data bucket will be listening  to a device resource, and its information is registered in every stream call.

```cpp
/*"TempHum" resource was declared in the setup() function
but the stream instuction is added in the loop*/

void loop() {
  thing.handle();
  // use your own logic here to determine when to stream/record the resource.
  if(requires_recording){
      thing.stream("TempHum");
  }
}
```

{% hint style="warning" %}
This instruction should NEVER be called each loop execution or in lower than 60s streaming rates as the bucket system will only store data each 60s.  
{% endhint %}

### **From device Write Call**

 This option sets the bucket in passive mode, waiting to be called by any Thinger.io "Generic Device" \(with Thinger.h libraries on it\) ****by means of the the `write_bucket()` method, as shown in the example code below. The special feature of this mode is that it allows to store data from different devices in the same data bucket.

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

### **From API Request \(for 3rd parties\):**

This configuration allows to store data from any other device or data source that can't be equipped with Thinger.io libraries on its codification. the data bucket will be set on passive mode waiting to receive data from any [**HTTP Device Callback**](../devices/http-devices.md) that has been properly configured to send data to this data bucket. 

{% hint style="info" %}
This feature can be also used to store data directly from any third party platform just calling to the data bucket REST API and sending information in JSON format. But it is preferable using the[ HTTP device way.](../devices/http-devices.md)
{% endhint %}

### **From MQTT Topic**

Private instances of Thinger.io platform has been provided with an MQTT broker, that can be used to subscribe or publish topics, but also, the data buckets can be configured to subscribe a topic and store all the data that is being published on it.

## Review Bucket Data

Once the data bucket has been configured, and it started to record data from a device or from write calls, it will display the information inside a table. Every record contains the server timestamp in UTC \(but shown in local time zone in the console\), and the record value. The value stored in the data bucket can be a single value, or any other JSON document. If the JSON document is composed by key-value pairs, like in the previous examples, they will be displayed in tabular format, just like in the following screenshot.

![](../.gitbook/assets/iotbucketdata.png)

## Bucket Data Import

In order to make bulk data upload or buckets backup processes, the data bucket system has been provided with an import feature that is able to retrieve a .csv file from a Thinger.io [**File System**](file-system.md) and store it's data with the XXX timestream. 

Note that using this feature has two restrictions. The user account must be able to use File Systems \(this is not available in free accounts\) and the file must be made with one variable per column and contain a variable with the Linux Timestream in microseconds.

![](../.gitbook/assets/image%20%28244%29.png)

{% hint style="info" %}
Files resulting from a data bucket export are completely suitable with the import feature, so they are perfect examples to observe a valid data frame
{% endhint %}

The import will allow to fill the data bucket with the same data contained in the CSV, ordered based on the TimeStamp in milliseconds included in the file.

To execute an import, the following steps must be carried out:

1. Create a new File System \([following **these** instructions](file-system.md)\) ****profile with public access configuration or open an existent one and upload the .csv file to be imported into the File System. 
2. Create the new data bucket
3. Select the source File System and place the file identifier in the "File name" section.
4. Click on "Import Data" button.

## Export Bucket Data

It is possible to export all your stored information in different file formats, so you can process the data offline, like applying Artificial Intelligence, Business Analytics, Big Data, etc. In this way, you can access your bucket and configure the export process:

![](../.gitbook/assets/image%20%28245%29.png)

 The data bucket download configurable parameters are:

* **Data format:** To obtain CSV, ARFF or JSON format file
* **Time format:**  Timestamp or ISO date format
* **Export range:** This section allows downloading the complete data bucket or selecting a custom range. 
* **Callback:** To set how the ending of the data bucket export process will be notified. Currently there are two ways: 
  * Sending an email to the account associated address 
  * Calling to an endpoint. This option allows sending the download link to third parties using an [**Endpoint profile**](endpoints-1.md)**.**

Onces the export data range and format has been select, the system will create a download link that will be stored in the "Export List" section below. This links can be used to provide customers of custom data reports form the IoT data.

![](../.gitbook/assets/image%20%2853%29.png)

The download links will be available for 3 months if the instance administrator has not specified a different interval. 

## Clear Bucket Data

Sometimes it can be useful to clear the bucket information without deleting the whole bucket, creating and configuring it again. Therefore, you can clear the bucket, or a part of them easily from the bucket page. In the clear process, the bucket can still record information from your devices.

![](../.gitbook/assets/data-bucket-clear.png)

Data bucket profiles can also be deleted from the data bucket list, by selecting the profiles to be deleted and pressing the red "Remove Bucket" button as shown in the image below:

![](../.gitbook/assets/image%20%2870%29.png)

## 

