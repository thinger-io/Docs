# Server Logs

Thigner.io server instances provide a logging console that allows visualizing the program status and events in real-time. These data can be used to evaluate the behavior of the server or any integration that is being developed.&#x20;

{% hint style="info" %}
This feature is only available for the instance's manager account&#x20;
{% endhint %}

## Start Logging

<figure><img src="../../.gitbook/assets/image (763).png" alt=""><figcaption></figcaption></figure>

Follow the next steps to start logging the events of the server instance:

<figure><img src="../../.gitbook/assets/image (762).png" alt=""><figcaption></figcaption></figure>

1. Go to the "Cluster Hosts" section of Thinger.io's main menu. The Host list displays all server instances connected within the same cluster, classified by their web domain.
2. Click the desired instance to access the server administration panel&#x20;
3. On the top right corner, choose the "Logs" tab to open the logging console
4. Press the "Connect" button to start the log sampling

<figure><img src="../../.gitbook/assets/image (764).png" alt=""><figcaption></figcaption></figure>

The structure of the log entries consists of six columns in which the following data is specified:

* **Date:** YYYY-MM-DD
* **Hour:** HH:MM:SS.SSS
* **PC**: Program Counter in seconds
* **Working Thread:** Thinger.io program has been created to optimize host architecture with multithread execution. This column identifies the thread that managed each event.
* **Program Module:** This column informs about the feature that is being used by each event.
* **Message:** Is the log content including a message type identification and a description

<br>

| Date | Hour | PC | Thread |   |   |
| ---- | ---- | -- | ------ | - | - |
