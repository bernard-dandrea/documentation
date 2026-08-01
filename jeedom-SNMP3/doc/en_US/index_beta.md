
<!--  
Last Modified: July 25, 2026, 6:39:50 PM
-->

# SNMP3 Plugin

Plugin for reading and writing the OIDs of devices that support the SNMP protocol.

SNMP is one of the widely accepted protocols for managing and monitoring network devices. Most enterprise-grade network devices come with a built-in SNMP agent.

The plugin uses the php-snmp package (see <https://www.php.net/manual/fr/book.snmp.php>), which is a wrapper for the Net-SNMP library (see <http://www.net-snmp.org>). The plugin allows you to query (get command) and update (set command) OIDs that support it.

# DISCLAIMER

This plugin is intended for people who are familiar with the protocol.

This isn't particularly complicated, but it still requires a solid understanding of the underlying concepts (authentication, OID, MIB, etc.).

Before contacting the developer about any issues, first verify that the settings for communicating with the SNMP device are correct.

To do this, you can use the snmpget command in an SSH session, for example:

snmpget -v 3 -n "" -u admin_snmp_2024 -a MD5 -A "xxxxxx" -x DES -X "yyyyy" -l authPriv 192.168.1.5 .1.3.6.1.2.1.1.6.0

![SNMP3_snmp_get](../images/SNMP3_snmp_get.png)

# Installation and Configuration of SNMP Devices

For the plugin to work properly, the SNMP protocol must be correctly installed and configured on the target system. Refer to the manufacturer's documentation for configuration instructions.

The v3 protocol is recommended to secure the connection.

![SNMP3_Synology](../images/SNMP3_Synology.png)

See above for an example of a configuration on a Synology NAS.

Test the connection settings using the snmpget command (see previous section) or other utilities.

# Plugin Configuration

Once the plugin is installed, you need to activate it. The php-snmp package is installed when the dependencies are installed.

You can enable the Debug log level to monitor the plugin's activity and identify any issues.


![SNMP3_Equipment](../images/SNMP3_cron.png)

You can also specify whether to use a standalone cron job. This prevents other cron jobs from being blocked if the plugin's cron job hangs, and ensures that the plugin's cron job isn't blocked by other cron jobs running for other plugins.

# MIB Management

OIDs can be identified by their numeric code, for example, .1.3.6.1.4.1.6574.1.1.0, or by using the corresponding MIB, for example, SYNOLOGY-SYSTEM-MIB::systemStatus.0.

When you install the php-snmp package, a number of MIBs are installed (usually in the /usr/share/snmp/mibs directory) and can be used directly.

The plugin allows you to install specific MIBs by placing the corresponding files—for example, SYNOLOGY-SYSTEM-MIB.txt—in the plugins/SNMP3/data/mibs directory.

You can also copy the files to the shared directory (usually /usr/share/snmp/mibs). Note that you will need to repeat this process if you restore Jeedom.

If you encounter difficulties implementing the MIBs, you can test them using the snmptranslate command (see <HTTPS://net-snmp.sourceforge.io/tutorial/tutorial-5/commands/snmptranslate.html>). Please note that in this case, the MIBs in the plugins/SNMP3/data/mibs directory are not taken into account.

# Device Setup

You can configure the devices from the plugin menu (Plugins menu, Connected Devices, then SNMP3).

Click Add to configure the SNMP device.

![SNMP3_Equipment](../images/SNMP3_Equipement.png)

Specify the SNMP device configuration:

-   **Name**: Name of the SNMP device
-   **Parent object**: Specifies the parent object to which the device belongs
-   **Category**: indicates the Jeedom category of the device
-   **Enable**: turns the device active
-   **Version**: SNMP version
-   **localhost**: Device IP address
-   **Security settings**: see <https://www.php.net/manual/fr/snmp.setsecurity.php>
-   **timeout**: maximum time to wait for a response to an SNMP request
-   **retries**: number of times the command is resubmitted in case of failure (3 if the field is empty)
-   **Icon**: Allows you to select an icon type for the device in the control panel

You can customize a specific icon by adding the corresponding image (for example, perso1.png for the "perso1" icon) to the plugin's plugin_info directory.

The **Test SNMP3 Connection** button lets you test whether the connection settings are correct (be sure to power on the device and save the configuration before clicking the button).

# Commands associated with devices

![SNMP3_Commands](../images/SNMP3_Commandes.png)

By default, two commands are created:

- Last Refresh: command that indicates when the SNMP device's information was last updated
- Refresh: an action command that updates all OIDs for which the update is enabled

The following buttons are available:

- Import an OID: creates an info command for an OID
- Add a refresh command: allows you to create an action command to force the retrieval of the OID value
- Add an action: allows you to create an action command to change the OID value (when permitted by the SNMP device)

# Analysis of Command Fields

For each command related to an OID, in addition to the usual Jeedom fields, you'll find:

- LogicalID:
  - for info-type commands, equal to the OID
  - For refresh commands, it is equal to 'R_' followed by the OID
  - For action commands, equal to 'A_' followed by the OID
- the "update" checkbox that allows you to choose whether or not to request an OID update
- the "scan" field, which indicates the OID's update frequency

For commands that update the OID, the subtype of the action command determines the format of the value transmitted to the SNMP device. When the subtype is 'Message', the header specifies the format and the message body specifies the value (only the first line is transmitted). See <https://www.php.net/manual/fr/function.snmpset.php> for a list of supported formats.

# Widget

![SNMP3_Widget](../images/SNMP3_Widget.png)

Here is an example of a widget. You can change the names of the commands to make them more descriptive.

# Reviews

![SNMP3_review](../images/SNMP3_avis.png)

If you like this plugin, please leave a rating and a comment on the Jeedom Market—it’s always appreciated: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4484#>
