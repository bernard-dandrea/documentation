
<!--  
Last Modified: July 28, 2026, 4:00:31 PM
-->

# BSBLAN Plugin

Plugin for interfacing with the BSB-LPB-LAN controller.

The BSB-LPB-LAN controller is the result of a project aimed at enabling communication with SIEMENS control cards that manage numerous boilers, heat pumps, and other industrial devices.

The documentation is very comprehensive and can be found at this address: <https://docs.bsb-lan.de>. The hardware can be purchased from Frederik Holst <bsb@code-it.de>.

The BSB-LAN can serve as a cost-effective replacement for the OZW controllers supplied by Siemens. This solution is much less expensive, provides access to all parameters of the Siemens cards (unlike the OZW), and offers much faster access times to the cards. Additionally, it is possible to transmit the temperature of heated zones without needing to use a room sensor.

Communication between the plugin and BSBLAN takes place via web APIs.

# Installation and Configuration of the BSBLAN Controller

For the plugin to work properly, the BSB-LAN module must be operational.

For installation and configuration, refer to the excellent documentation available on the project's website.

If you want to change any settings, you'll need to enable them in the BSBLAN configuration.

The plugin has been tested with versions 3.2 and 4.2. In principle, the plugin should work with earlier versions as well, since the API calls are fairly basic and have likely been available for many versions.

# Plugin Configuration

Once the plugin is installed, you need to activate it.

![Setup](../images/BSBLAN_configuration.png)

You can also specify whether to use a standalone cron job. This prevents other cron jobs from being blocked if the plugin's cron job hangs, and ensures that the plugin's cron job isn't blocked by other cron jobs running for other plugins.

You can enable the Debug log level to monitor the plugin's activity and identify any issues.

# Device Setup

You can configure the devices from the plugin menu (Plugins menu, Connected Devices, then BSBLAN).

Click Add to configure the BSBLAN controller.

![BSBLAN_Equipment](../images/BSBLAN_Equipement.png)

Specify the BSBLAN configuration:

-   **Name**: BSBLAN name
-   **Parent object**: Specifies the parent object to which the device belongs
-   **Category**: indicates the Jeedom category of the device
-   **Enable**: turns the device active
-   **Visible**: Makes it visible on the dashboard
-   **IP Address**: Device IP address
-   **Username and password**: login credentials for the web server
-   **Passkey**: prefix to be included in HTML requests (see BSBLAN documentation)
-   **Timeout**: maximum time to wait for a response to an HTTP request (15 seconds if the field is empty)
-   **Updates**: The method used to perform updates is either JSON or a direct command in the URL. In some cases, it was observed that updates via JSON were not being performed. It was not possible to determine why. In such cases, you can use the /S command option, which works every time. However, in version 3 of BSBLAN, certain commands that require specifying the INFO flag (for example, sending the room temperature) are not supported.
-   **Number of attempts**: the number of times the command is submitted in case of failure (3 if the field is empty)
-   **Icon**: Allows you to select an icon type for the device in the control panel

You can assign a specific icon to BSBLAN. You can also customize a "perso" type icon by adding the corresponding image (for example, perso1.png for the "perso1" type icon) to the plugin's plugin_info directory.

The following buttons perform the following functions:

-   **Access BSBLAN**: allows you to log in to the BSBLAN web interface
-   **Test BSBLAN Connection**:  Allows you to test whether the connection settings are correct (be sure to save the configuration before clicking the button). The BSBLAN version number is displayed.

# Commands associated with devices

![BSBLAN_Controls](../images/BSBLAN_Commandes.png)

By default, two commands are created:

- Last Refresh: command that indicates when the BSBLAN information was last updated
- Refresh: an action command that updates all settings for which the update is enabled

The following buttons are available:

- Import a parameter: allows you to create an info command for a specific parameter
- Add a refresh command: forces the parameter value to be retrieved
- Add an action command: allows you to change the parameter value (when permitted by the web server)

# Analysis of Command Fields

For each command related to a parameter, in addition to the usual Jeedom fields, you'll find:

- LogicalID:
  - for info-type commands, equal to the parameter code
  - For action commands, use 'A_' followed by the parameter code
  - For refresh commands, it is equal to 'R_' followed by the parameter code
- the "update" checkbox that allows you to choose whether or not to update the setting
- For info commands, the "scan" field indicates the update frequency of the parameter
- For action commands, the MAJ field, which allows you to specify a specific update mode

# Widget

![BSBLAN_Widget](../images/BSBLAN_Widget.png)

Here is an example of a widget. You can change the names of the commands to make them more descriptive.

# Reviews

![BSBLAN_review](../images/BSBLAN_avis.png)

If you like this plugin, please leave a rating and a comment on the Jeedom Market—it’s always appreciated: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4424#>
