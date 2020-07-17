# Grafana Plugin

_Grafana_ is an Open Source visualization and analytics software that allows to query, visualize, alert on, and explore metrics no matter where they are stored. This integration plugin allows working together with Grafana & Thinger.io, providing a lot of new tools to analyze IoT devices data with amazing graphs and visualization widgets. 

![](../.gitbook/assets/image%20%28270%29.png)

{% hint style="info" %}
[Note: Plugins are only available for premium Thinger.io serves. Check **this link** to create your own instance within minutes](https://pricing.thinger.io).
{% endhint %}

Graphics technology allows for the simple development of advanced dynamic dashboards for data visualization and analysis, create logs, metrics, or even automatic alerts. The configuration of Grafana widgets is done in a similar way to Thinger.io's dashboard widgets, with drag&drop and flexible sizes that allow us to design a custom layout according to the requirements of each project, so the adaptation to this new tool will be very fast.

Another interesting fact of Grafana is its flexibility, thanks to a large widget import repository we can acquire online new representation elements created by Grafana's own developers or by contributions from a large community of users. We can even develop our own widgets and publish them. 

A lot of additional information can be found on the creator's website and also in their extensive documentation, available at [**this link**](https://grafana.com/docs/grafana/latest/features/datasources/add-a-data-source/?utm_source=grafana_gettingstarted)**.**

## Starting with Grafana Plugin 

Once the standard plugin deployment process has been done \(as explained here\), Grafana plugin requires few additional steps to be ready to work. In the following sections, we explain how to log in and how to complete the configuration of the plugin successfully

### First Login

When the plugin is newly installed, the first login should be done as login credentials your Thinger.io account username and "admin" into the password field. Once the first login occurs the system will ask to change the password to put a custom one. After this, it is possible to see the Grafana workspace with its different options. 

![](../.gitbook/assets/image%20%28263%29.png)

### Adding a Datasource

The link between the Thinger.io data and the graphical plug-in is done by adding the buckets database as a new Data Source for Grafana, so the first step after logging in is pressing the data sources block, this will open a new context in which we can browse different technologies to obtain the data that will be displayed later in the dashboards.

![](../.gitbook/assets/image%20%28274%29.png)

In this menu, select the database technology that is being used to store Thinger.io data buckets. It is by default  "InfluxDB", but the instance administrator may have selected another technology, so if you do not know the correct system you should contact the server administrator.

Then configure the address and the credentials to provide Grafana the required authorization to access and retrieve the data. When using InfluxDB the next configuration form should be filled according to the next instructions: 

* **Name**: Just for identification purposes
* **HTTP**: 
  * **URL**: It is the IP address and socket of the database server that will listen Grafana requests. 
  * **Access**: Server \(default\)
* **InfluxDB Details:**
  * **Database**: Thinger.io account username 
  * **User**: Thinger.io account username
  * **Password**: Thinger.io account password

![](../.gitbook/assets/image%20%28289%29.png)

## Working with Grafana

### Creating a new dashboard

Once the data source has been configured, the most common way to start working with Grafana is to create a new dashboard for data representation. To do this, click on the + button in the Grafana main menu and select "new dashboard". This will open a new empty dashboard ready to be configured with a custom layout and representation pannels by clicking on the button "+Add new pannel", that will open the panel configuration context, which is organized in two sections:

* **Panel Configuration**: On this step, we can choose the graphic to be used in the widget, for example time series charts, tatalalalala Es interesante que las explores todas las opciones que ofrece este menú, tanto en tipos de visualización como en otras configuraciones del panel que permiten elegir la leyenda, los ejes, etc. 

![](../.gitbook/assets/image%20%28281%29.png)

* **Data source Configuration:** Select the data source you want to use by clicking on the drop-down menu to change "default" to the name of the InfluxDB database that you added in the first step \(adding a datasource\). If it was done correctly, in the section `FORM`  the complete list of Thigner.io data buckets will be available to select any of them. Then, the section `SELECT` allows to specify any of the stored device data to be represented on the panel. 

![](../.gitbook/assets/image%20%28277%29.png)

* **Transform data:** Allows to filter or aggregate devices data using different tools before it will be shown in the representation pannel. 
* **Automatic Alerts**: The graph process can be configured to work in the background evaluating the variables received to generate alerts in real-time. This panel allows the configuration of many parameters for the creation of alerts in a personalized way.

### Adding new widget types

Grafana is a very flexible tool, users can add new panels as they go, retrieving it from different repositories. These panels are developed by the Grafana team but also by community contributors, allowing this technology to grow very quickly thanks to the group effort. The process to add new elements in the Grafana Plugin of Thigner.io is through the command console of the docker container in which the program is executed locally. In it, a command is introduced that allows you to quickly download and install components as explained below:

1. The first step is to find the new element to add, the following link leads to the official Grafana plugin repository. 
2. Access Grafana's shell, to do so click on the Plugins tab in the Thinger.io main menu, then access Grafana's profile in the Plugins Marketplace:

![](../.gitbook/assets/image%20%28268%29.png)

3. Once inside click on the _&gt;\_Shell_ option and click on "connect". Then the download and installation command can be pasted into the console and executed. 

![](../.gitbook/assets/image%20%28283%29.png)

4- After the installation of a new element in Grafana it is necessary to restart the plugin container. This process is done through the plugin's function bar by means of the Restart button.

## Provide public access to share Grafana projects

Grafana allows creating working groups to collaborate on project development. 

It also allows sharing dashboards with third parties or in a public way



