<!--  
Last Modified: July 28, 2026, 3:35:23 PM
-->


# OZW Plugin

Plugin that allows integration with SIEMENS OZW-type communication control units.

OZW communication hubs are used to communicate with the control boards that operate many boilers, heat pumps, and other industrial devices. These hubs feature a built-in web server that allows users to control the devices connected to them.

There are two models that work in roughly the same way:

- OZW672 for communicating with devices directly on the LPB and BSB buses
- OZW772 for communicating with devices via the KNX protocol

Communication between the plugin and the OZW takes place via the WEB APIs provided by SIEMENS, which allow you to simulate the interactions that normally occur on the web server.

This plugin is a major update to the OZW672 plugin (see https://github.com/NextDom/plugin-ozw672), which is no longer maintained and does not work with the current version of Jeedom.

# Installation and Configuration of the OZW Controller

For installation of the WEB communication hub, refer to the corresponding SIEMENS documentation.

![OZW_WEB_ACCESS](../images/OZW_WEB_ACCESS.png)

Enable access to the WEB APIs (Home menu > 0.5 OZWx72.01 > Settings > Communication > Services).

The plugin has been tested with version 12 of the web server. In principle, the plugin should work with earlier versions because the API calls are fairly basic and have likely been available for many versions.

![OZW_home](../images/OZW_accueil.png)

Once the installation is complete, you should see a web page that looks like this.

In this configuration, there are 2 devices:

-   The first one shows an LMS14 board controlling a boiler
-   The second one represents the OWZ672 communication hub and allows for its configuration

![OZW_device](../images/OZW_device.png)

The various data points defined for the map are accessible. You can view them and, if necessary, modify them.

In the APIs provided by SIEMENS, data points must be specified using their web reference, which can be found in the web interface.

![OZW_datapoint_reference](../images/OZW_datapoint_reference.png)

To find it, select the corresponding line and inspect the element (usually by right-clicking and selecting "Inspect"). In the corresponding code, you'll find a number in the 'openDialog('xxx')' or 'id='dpxxx'' statement that indicates the web reference—591 in the example above.

![OZW_ID_menu](../images/OZW_ID_menu.png)

Similarly, a menu ID may be required and is found in the same way—590 in the example above.

# Plugin Configuration

Once the plugin is installed, you need to activate it.

![Setup](../images/OZW_configuration.png)

You can also specify whether to use a standalone cron job. This prevents other cron jobs from being blocked if the plugin's cron job freezes, and ensures that the plugin's cron job isn't blocked by other cron jobs run by other plugins.

You can enable the Debug log level to monitor the plugin's activity and identify any issues.

# Device Setup

You can configure the devices from the plugin menu (Plugins menu, Connected Devices, then OZW).

Click Add to configure the OZW.

![OZW_Equipment_OZW](../images/OZW_Equipement_OZW.png)

Specify the OZW configuration:

-   **Name**: Name of the OZW
-   **Parent object**: Specifies the parent object to which the device belongs
-   **Category**: indicates the Jeedom category of the device
-   **Enable**: turns the device active
-   **Visible**: Makes it visible on the dashboard
-   **IP Address**: Device IP address
-   **Username and password**: login credentials for the web server
-   **Session duration**: the period after which the session ID is renewed
-   **Icon**: Allows you to select an icon type for the device in the control panel

After saving the OZW, the following buttons are active:

-   **Access the OZW**: allows you to log in to the OZW via the web
-   **Import Devices**:  Allows you to import the equipment corresponding to the devices connected to the OZW.

![OZW_Equipment_OZW_devices](../images/OZW_Equipement_OZW_devices.png)

In the example above, after importing the devices, you'll find:

- the OZW672 as the main device
- the OZW672.01 as a device
- the LMS14 board that controls the boiler

![OZW_Equipment_OZW_device](../images/OZW_Equipement_OZW_device.png)

You can assign a specific icon to a device. You can also customize a "perso" type icon by adding the corresponding image (for example, perso1.png for the "perso1" icon) to the plugin's plugin_info directory.

# Commands associated with devices

![OZW_Controls](../images/OZW_Commandes.png)

For the OZW, two info-type commands are created:

- Status: 1 when communication with the web server is established; 0 otherwise
- SessionID: ID used by the Web APIs

![OZW_Commands_device_initial](../images/OZW_Commandes_device_initial.png)

For devices connected to the OZW, two commands are created:

- Last refresh: a info-type command indicating when the device's information was last updated
- Refresh: a type-action command that updates all data points for which the update is enabled

![OZW_Importer_Main Menu](../images/OZW_Importer_Menu_principal.png)

The "Import Main Commands" button in the Equipment tab allows you to import all data points from the menu labeled "Mobile." This menu is available in the Android app provided by SIEMENS and is not available on all devices. Creating the commands may take several minutes. Once completed, the device's main data points will be defined as "info" type commands.

![OZW_import_specific_menu](../images/OZW_import_menu_specifique.png)

Similarly, the "Import Menu" button in the Equipment tab allows you to import all data points from a specific menu. To do this, you must provide the menu's web reference.


![OZW_buttons_import_order](../images/OZW_boutons_import_commande.png)

In the "Commands" tab, the following buttons are available:

- Import a data point: allows you to create an info command for a specific data point
- Add an action: allows you to change the value of the data point (when permitted by the web server)
- Add a "refresh" command: forces the retrieval of the datapoint's value

**Note**: Be sure to provide the datapoint's WEB reference, not the line number displayed on the datapoint line.

# Analysis of Command Fields

![OWZ_Control_Analysis](../images/OWZ_Analyse_commande.png)

For each command related to a data point, in addition to the usual Jeedom fields, you'll find:

- LogicalID:
  - for "info" type commands, equal to the datapoint's WEB reference
  - for action commands, equal to 'A_' followed by the datapoint's WEB reference
  - For refresh commands, set to 'R_' followed by the datapoint's WEB reference
- the "update" checkbox that allows you to choose whether or not to update the data point
- the "scan" field, which indicates the update frequency of the data point

# Widget

![OZW_widget](../images/OZW_widget.png)

Here is an example of a widget. You can change the names of the commands to match the line number specified on the web server.

# Translation

The interface, log messages, and documentation have been translated into the 5 languages supported by Jeedom (thanks to @mips for developing ga-translation and docs-translations). If you find any translation errors, please open a support ticket and, if possible, attach the corrected translation file (located in the plugin’s core/i18n directory).

# Reviews

![OZW_review](../images/OZW_avis.png)

If you like this plugin, please leave a rating and a comment on the Jeedom Market—it’s always appreciated: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4414#>
