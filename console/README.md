
#Device Management

Any device in Thinger.io must be registered to get access to the cloud. Each one has its own identifier and credentials and is related to the user account. The most easy way to registering the device is by using the thinger.io cloud console. This post describes the required steps to register a new device.

Once you have been logged in your console dashboard, please go to the **Devices** section that appears in the left menu.

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/8b411097d72092216e8611aab2d02efea16ef797.png" width="196" height="38"> 

This section will list your registered devices and will show some information about its connection. Something like the following picture.

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/58e06f4e8771738d9a1fa7f26a53fdb2864b5937.png" width="100%"> 

In this screen click on **Add Device** that will open a form form adding some device information.

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/b0a8aa7501dfb947a8b6d0add5a9d4b698bb657a.png" width="100%"> 

Add here the **device identifier** (unique within your devices), a **device description** that may help you to identify your device, and the **device credentials**. Each device has its own identifier/credential, so a comprised device will not affect other devices. All your passwords in the server are stored securely using `PBKDF2 SHA256` with a 32 bytes salt generated with `PRNG` and a non-despreciable amount of iterations. Keep your **device identifier** and **device credential**, as you will need them for connecting your device (the password is not visible once you set it).

If all goes fine, you should see some success message 

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/68e13f2c4df7ecb9f0261d84ba36e12b3d8498ce.png" width="100%"> 

 Go back to your devices list, and now you should see your new device created.

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/ed5abaa692e2257e08cc06bbed7065d6a4ff3a58.png" width="100%"> 

Now you can use your new device id and the device credentials to connect the new device. 

## Opening the device dashboard ##

You can try clicking in the device name for opening its dashboard, that currently displays some information like its connection status, connected time, up and down bytes, and a real time activity state. This dashboard will be improved in the future with more functionality and configurable elements. Also you can see in this dashboard a small button called **DEVICE_NAME API**. This button will open a screen that allows you interacting directly with the resources programmed in your device.

<img src="https://discoursefiles.s3-eu-west-1.amazonaws.com/original/1X/f61eadbe92939d1c7a6bc066df698e6b88ecb74e.png" width="100%">


## Data Buckets ##