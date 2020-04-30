# SystemLink Web Interface Template

## Installing a Plugin Built in LabVIEW NXG

This repository contains a project created in LabVIEW NXG 3.0 preconfigured to build a NIPKG that can be used to install a built WebVI application onto a SystemLink Server. The following instructions provide steps to install the NIPKG onto the a SystemLink server in the same manner that SystemLink server installs NIPKGs onto remote Windows and Linux Real-Time targets.

### Configuring the SystemLink server for self deployment

* Log into Windows on the SystemLink server machine
* If not done already install the **NI SystemLink Client** onto the server using [NI Package Manager](https://www.ni.com/en-us/support/downloads/software-products/download.package-manager.html)
* Open the **NI SystemLink Client** configuration utility
* Under **Connection Status** select the **Connect to a SystemLink server** radio button
* In the **Server hostname and IP address failover list** field enter `localhost`.
* In a browser, navigate to the **Systems Manager** application in SystemLink
* Go to **Pending Systems** and add the newly listed system (it will be using the server's hostname) as a managed system.

### Building the LabVIEW NXG WebVI Application

* Open `webvi-systemlink-plugin.lvproject` in LabVIEW NXG 3.0 or later.
* Edit the WebVI in the project as you see fit.
* See **Installing and Customizing the Plugin** in this readme for details for modifying the support files needed for SystemLink plugins.
* Go to the WebApp.lvdist and click **Build Distribution**

### Uploading and Installing Using Package Repository and Systems Manager

* In a browser, navigate to the **Package Repository** application, and create a feed or reuse an existing feed.
* In the details for the feed click the **Add** button under the **Packages** section.
* Click **Upload Packages** and **Browse**
* Navigate to and select the NIPKG built in LabVIEW NXG
* Click **Close** and navigate to your SystemLink server as its listed under **Managed Systems** and the **Systems Manager** application
* Go to **Software** and navigate to the **Feeds** tab.
* If not already complete add the feed containing the NIPKG to the System
* Go to the **Available** software tab and find the WebVI build application. This example project uses the display name *SystemLink WebVI Plugin*.
* Select install from the dropdown button in the same row as the application.
* Upon successful install navigate back to the SystemLink landing page which lists all installed applications.
* Under **Additional Applications** you should see an application named **Web VI**. Clicking on this icon will load the WebVI application created in LabVIEW NXG.

## Creating a Generic SystemLink Web Application Plugin

The following instructions can be used to add any generic web application created with any framework as a SystemLink plugin. A working plugin template is included in the `Web Server` folder called `webapp_plugin`. You can modify this template or duplicate the plugin by modifying the following configuration files. E.g. If you needed to create a second plugin (`webapp_plugin2`), you could duplicate the `webapp_plugin` directory, and make the following changes.

### Name

To change the name of the plugin in the SystemLink homepage, change the `pluginTitle` value within the en.json file as well as a few values within the config.json file:

`C:\Program Files\National Instruments\Shared\Web Server\htdocs\plugins\webapp_plugin2\resources\json\locales\en.json`

```json
{
   "webapp_plugin2": {
      "pluginTitle": "Web Application 2"
   }
}
```

`C:\Program Files\National Instruments\Shared\Web Server\htdocs\plugins\webapp_plugin2\config.json`

```json
{
   "authorizationMarker": "/plugins/webapp_plugin2/resources/marker.txt",
   "buttonIconCls": "fa fa-area-chart",
   "buttonLabelToken": "webapp_plugin2.pluginTitle",
   "buttonTooltipToken": "webapp_plugin2.pluginTitle",
   "iframeResources": {
      "en": {
         "json": [
            "resources/json/locales/en.json"
         ]
      }
   },
   "iframeSrc": "plugins/webapp_plugin2/index.html",
   "orderWeight": 2,
   "permission": "",
   "resources": {
      "en": {
         "css": [
            "plugins/webapp_plugin2/resources/css/webapp_plugin.css"
         ],
         "json": [
            "plugins/webapp_plugin2/resources/json/locales/en.json"
         ]
      }
   },
   "routeToken": "webapp_plugin2",
   "titleToken": "webapp_plugin2.pluginTitle",
   "waitUntilLaunched": false
}
```

### Access Control

To configure security/access control for all WebVIs in one place, modify the `52_webapp_plugin.conf` file, and duplicate the `<Directory>` tag to specify any other plugins.

 ```xml
<Directory htdocs/plugins/webapp_plugin/resources>
    # Tell the privilege module to use webapp_plugin.htpriv
   Session On
    AuthNIPrivilegeApplication webapp_plugin
    Require privilege ModifyResource
</Directory>
<Directory htdocs/plugins/webapp_plugin2/resources>
    # Tell the privilege module to use webapp_plugin.htpriv
   Session On
    AuthNIPrivilegeApplication webapp_plugin
    Require privilege ModifyResource
</Directory> ​
```

To change the name of the application shown in the NI Web Server Configuration utility, change the `<description>` tag within the `conf\htpriv.d\webapp_plugin.xml` file. E.g.

```xml
<description xml:lang="en">Web Application</description>
```

### Icon

We use font awesome <http://fontawesome.io/icons/> by default for SWIF icons

* Add the icon image file to `htdocs\plugins\webapp_plugin\resources\images`.
* Modify `htdocs\plugins\webapp_plugin\resources\css\webapp_plugin.css` to use the new icon.
* Modify `htdocs\plugins\webapp_plugin\config.json` to use the new icon class.

### Copying the Plugin files to the Server

Copy the `Web Server\htdocs` and `Web Server\conf` folders and files to your SystemLink web server (e.g. the NI Web Server)  `C:\Program Files\National Instruments\Shared\Web Server`. **Note** this step is not necessary if you are using NI Packages and SystemLink feeds to install the application on the SystemLink Server. See **Installing a Plugin Build in LabVIEW NXG**.
