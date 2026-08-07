
<!--  
Last Modified: July 27, 2026, 3:27:46 PM
-->


# EcoLegrand Plugin

Plugin for retrieving data from older-generation Legrand energy meters (part number 412000).

Unlike the new smart meters, whose data is accessible only through the cloud, the older smart meters can be accessed via a local web interface. In particular, you can view real-time consumption directly, which is not possible with the new smart meters (you have to view the data directly on the smart meter).

The 412000 smart meters have not been sold since 2020, but they remain a viable option compared to the current version.

Communication between the plugin and the energy meter occurs by retrieving data from user-defined JSON files. The user defines the data they wish to retrieve in the JSON file.

The plugin’s basic function is to retrieve data from smart meters. This data must be processed using other methods (virtual devices, scenarios, etc.) and requires a certain level of familiarity with Jeedom in order to manipulate the data.

# Installation and Setup of the EcoLegrand Eco-Meter

For the plugin to work properly, the eco-meter must be operational and accessible via the web interface.

The plugin has been tested with version 3.0.17, which is the latest release and will no longer be updated since this energy meter is no longer supported.

# Defining the data to be retrieved in a JSON file

The data to be retrieved is defined in a JSON file that must be copied to the smart meter.

{   
"meter_C1":~LG536 2 12,724$,
"C2_meter":~LG536 4 12,724$,
"meter_C3":~LG536 6 12,724$,
"C4_meter":~LG536 8 12,724$,
"meter_C5":~LG536 10 12,724$,
"EF_Meter":~LG538 0 12,907$,
"EC_Meter":~LG538 1 12,907$
}

The JSON file has the format shown above. There is one line for each piece of data to be retrieved (be careful not to include a comma on the last line and to use double quotes).

Each line includes the data name and the internal reference defined in the eco-meter. The linked file <https://github.com/bernard-dandrea/documentation/blob/main/jeedom-EcoLegrand/doc/fr_FR/JSON_codes.txt> provides a non-exhaustive list of usable references.

Please visit the following forum <https://easydomoticz.com/forum/viewtopic.php?t=1942&start=20> for more information.

# Copy the JSON file to the smart meter

Copying is done via the FTP protocol. You can use the FileZilla program.

![FileZilla_Connect](../images/FileZilla_Connect.png)

Log in using the IP address and login credentials (default: admin / password).

![FileZilla_SYS](../images/FileZilla_SYS.png)

Navigate to the SYS directory.

![FileZilla_COPY](../images/FileZilla_COPY.png)

Copy the JSON file. Note that its name must be short enough; otherwise, the copy will fail.

The SYS directory contains the HTML files used by the eco-meter. By analyzing them, you can find references to the variables you're interested in.

# Problem with energy meters

The forum thread above explains very well the issue encountered with energy meters (pulse meters are not affected).

It appears that the eco-meter software manages these meters internally using float variables of type 32 bits. These have a precision of approximately 7 decimal places.

These meters are updated every second and display readings in kWh to six decimal places.

As a result, when consumption exceeds 10 kWh, accuracy begins to decline, especially if there is little usage on the line in question. This becomes a significant issue when consumption exceeds 100 kWh.

To address this issue, the plugin can reset the meters to zero based on a user-defined threshold (typically between 1 and 10 kWh). The plugin handles the offset and provides a cumulative meter reading. Note that resetting the internal meter in this way may affect the statistics provided by the eco-meter.

# Installing the plugin

Once the plugin is installed, you need to activate it.


![Setup](../images/configuration.png)

You can also specify whether to use a standalone cron job. This prevents other cron jobs from being blocked if the plugin's cron job freezes, and ensures that the plugin's cron job isn't blocked by other cron jobs running for other plugins.

You can enable the Debug log level to monitor the plugin's activity and identify any issues.

# Device Setup

You can configure the devices from the plugin menu (Plugins menu, Energy, then Legrand Ecocompteur).

Click Add to set up an energy meter.

![Equipment](../images/Equipement.png)

Specify the energy meter configuration:

-   **Name**: name of the smart meter
-   **Parent object**: Specifies the parent object to which the device belongs
-   **Category**: indicates the Jeedom category of the device
-   **Enable**: turns the device active
-   **Visible**: Makes it visible on the dashboard
-   **IP Address**: Device IP address
-   **JSON file**: a JSON file containing the definition of the data to be retrieved

The following buttons perform the following functions:

-   **Access the Eco-Meter**: allows you to log in to the Eco-Meter via the web
-   **Test JSON**: allows you to test the JSON file and verify that the returned parameters are correct
-   **Create meters**: Generates the info commands corresponding to the meters

# Commands associated with devices

![Commands](../images/Commandes.png)

By default, two commands are created:

- Last Refresh: indicates when the eco-meter's data was last updated
- Refresh: Forces the retrieval of meter readings. A cron job triggers the update every minute.

An info command is created for each meter. For each one, in addition to the usual Jeedom fields, you'll find:

- the "update" checkbox that lets you choose whether or not to update the counter
- the threshold, which is the value at which the meter is reset to zero
- the command that resets the counter
- the offset, which is the cumulative value of the counter at the time of reset
- the current meter reading (offset + meter reading in the eco-meter)

What is the command used to reset the counters and the type, such as http://192.168.1.xxx/wp.cgi?wp=536+X+12724+-1+-1+4+0.0, i.e., wp.cgi? followed by the meter references and fixed values; for example, wp=536+2+12724+-1+-1+4+0.0 for meter_C1. See the forum <https://easydomoticz.com/forum/viewtopic.php?t=1942&start=120> for more information.

For non-numeric fields, change the field type from Numeric to Other (the threshold and offset are irrelevant in this case).

# Widget

![Widget](../images/Widget.png)

Here is an example of a widget. Note that you must specify the units yourself in the command.

# Data Analysis

Using scenarios—whether virtual or PHP procedures—you can generate your own metrics based on meter readings.

![power](../images/puissance.png)

For example, you can generate a power reading based on the average power calculated between two measurements.

![daily consumption](../images/conso_jour.png)

Or generate daily electricity usage reports.

# FAQ

In some cases, the JSON file returned by the eco-meter may not be decoded.

![json_error](../images/json_error.png)

In this case, a message is displayed in the log.

![json_lint](../images/json_lint.png)

To find the source of the error, retrieve the JSON file returned by the eco-meter from the log and test it on the website <https://jsonlint.com/>.

In this case, the error is caused by the fact that the conversion routine does not recognize the leading 0 in the "Linky_Conso":024795944 input.

You can fix this by enclosing the value 024795944 in quotes.

To do this, edit the definition file for the data to be retrieved and add quotes to the corresponding entry:

"Linky_Conso":~LG526 1 12005$, --> "Linky_Conso":"~LG526 1 12005$",

The string "024795944" will then be treated as a string, and there will no longer be any issues during conversion.

# Translation

The interface, log messages, and documentation have been translated into the 5 languages supported by Jeedom (thanks to @mips for developing ga-translation and docs-translations). If you find any translation errors, please open a support ticket and, if possible, attach the corrected translation file (located in the plugin’s core/i18n directory).

# Reviews

![EcoLegrand_review](../images/EcoLegrand_avis.png)

If you like this plugin, please leave a rating and a comment on the Jeedom Market—it’s always appreciated: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4430#>
