# FILE STORAGES

Thinger.io provides a flexible cloud storage system that allows uploading files to the IoT server in order to provide support to other platform features such as the HTML Widget or the OTA System. The information will be stored in a non-volatile memory of the server host

{% hint style="success" %}
Note that this feature is only available for [**private instances**](../server/deployment/), as it requires cloud storage system.&#x20;
{% endhint %}

## Create an Storage Profile

Each storage profile will create an isolated directory with different access permissions. To create a new storage profile, press the "Add Storage" button and complete the form as shown in the image below:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LpXqB3J1BMD5s4OpYSg%2F-LucqRxhetQ1epw8c0rM%2F-LugYjBTWsAuRiVdLaAK%2Fimage.png?alt=media\&token=395f802e-c8f2-467d-8db7-73628d1b34ca)

* **Storage ID:** Enter here a unique ID for the Storage profile
* **Storage name:** Each Storage can have also a mnemonic name
* **Storage Description:** Additional information for easy identification of the storage profile proposals.
* **Public Read:** This switch provides public read-only permissions to third parties using an HTTP path that will be created automatically using the Storage ID.

Once the form is completed, pressing the "Add Storage" button will create a new Storage Profile.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LpXqB3J1BMD5s4OpYSg%2F-LucqRxhetQ1epw8c0rM%2F-LugaRt3C0OAQRYEPjUu%2Fimage.png?alt=media\&token=c01e5a95-6d26-4a58-bdee-79edd01be338)

* **Index File:** Is the file from the storage system that will be opened when accessing the public HTTP path. By default is "index.html" but can be changed with any other file stored in the system.
* **HTTP Path:** Is the URL that allows reading the index file from a third party program if the public access permission is switched on.
* **Storage Explorer Button:** This element opens the experimental File Explorer that allows managing the file system.

## Storage Explorer (Experimental)

The storage explorer is a complete file management tool ready to upload, modify and remove documents from the file system. Its graphical interface is constituted by three sections:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LpXqB3J1BMD5s4OpYSg%2F-LuhR1o724KmtLC5k65J%2F-LumBFh1TC55YoEbfyKQ%2Fimage.png?alt=media\&token=ab290ea5-b7fc-47f9-8b76-074b9cd1606b)

* Top bar contains logotypes with different controls
  * **Refresh**: If any changes have been made to the file system from a source other than this terminal, the refresh button allows you to update the status of the documents.
  * **Delete files**: The "-" button allows removing all selected files.
  * **Upload files**: This button opens the file upload context, which is explained in the next section.
  * **Constraint**: Collapses/deploy the files navigator.
  * **Store**: Save the current status of all the edited documents
* Left sidebar is a files navigator in which the database elements can be shown and selected to be edited or deleted.
* In the bottom section of the interface, the explorer shows a terminal console in which some information can be displayed when working with extensions.

### Uploading files

Thinger.io's file explorer has a upload manager that can be accessed by pressing the "Upload files" icon. this manager supports the massive files uploading from any computer, just dragging the files or directories to the central box surface and pressing the green "upload all" button.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LpXqB3J1BMD5s4OpYSg%2F-LugngoLxkMv8Jes9uZR%2F-Lugnrpvsu9Nqv1oYOhi%2Fimage.png?alt=media\&token=5aede838-6fd8-4fb2-9548-53d5d76c359e)

### Edit Files

The storage explorer contains a navigator in which the data hierarchy is displayed. Each file is represented under its hierarchy context. an enhanced text editor is also included, allowing yo show and modify files in the cloud.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LpXqB3J1BMD5s4OpYSg%2F-LugngoLxkMv8Jes9uZR%2F-LugopLGYysSswo2P4VS%2Fimage.png?alt=media\&token=ace67590-ec66-4f73-b28e-2284e37cf75e)

### Deleting Files

when the files are no longer required in the file system, we can delete them using the file explorer. It is possible to select more than one file by pressing the "cmd" button on the keyboard and clicking on the files you want to delete. Finally, pressing the "-" icon of the top bar.&#x20;

![](<../.gitbook/assets/image (162).png>)

### &#x20;
