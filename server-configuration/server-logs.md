# Server Logs

Thigner.io server instances provide a logging console, that allows to visualize the program status and events in real-time. These data can be used to evaluate the behavior of the server or any integration that is being developed.&#x20;

{% hint style="info" %}
This feature is only available for the instance's manager account&#x20;
{% endhint %}

## Start Logging

Follow the next steps to start logging the events of the server instance:

![](<../.gitbook/assets/image (284).png>)

1. Go to "Cluster Hosts" section of Thinger.io main menu. The Host list will display all the server instances that have been connected in the same cluster classified by its web domain.&#x20;
2. Click the desired instance to access the server administration panel&#x20;
3. On the right top corner, choose "Logs" tab to open the logging console
4. Press "Connect" button to start the log sampling

![](<../.gitbook/assets/image (285).png>)

The structure of the log entries consists of six columns in which the following data is specified:

* **Date: **YYYY-MM-DD
* **Hour:** HH:MM:SS.SSS
* **PC**: Program Counter in seconds
* **Working Thread:** Thinger.io program has been created to optimize host architecture with multithread execution. This column identifies the thread that managed each event.
* **Program Module: **This column informs about the feature that is being used by each event.
* **Message:** Is the log content including a message type identification and a description

\


| Date | Hour | PC | Thread |   |   |
| ---- | ---- | -- | ------ | - | - |
