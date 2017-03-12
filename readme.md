#Skyline Web Interface Template
__Creating a Skyline Web Application__

* Copy the `Web Server/htdocs` and `Web Server/conf` folders and files to your Skyline web server `C:\Program Files\National Instruments\Shared\Web Server`
* Replace `index.html` with your application or modify `htdocs\plugins\webvi_plugin\config.json` if you need to launch a page other than index.html
* Modify `conf\htpriv.d\webvi_plugin.xml` to change the name of your application and permissions in the NI Web Server Configuration utility.
* Modify `htdocs\plugins\webvi_plugin\resources\json\locales\en.json` to change the display name of your web application as shown in the web interface
* We use font awesome <http://fontawesome.io/icons/> by default for SWIF icons and you can modify `htdocs\plugins\webvi_plugin\config.json` to use a different icon.


__Deploying a WebVI__

* Build your application with LabVIEW
* Copy the exported files to the `htdocs\plugins\webvi_plugin` folders
* Modify `htdocs\plugins\webvi_plugin\config.json` if your application is something other than index.html


#TODO 

* Create a script to uniquify the plugin to make it easier to create multiple plugins
* Figure out how to use custom icons
