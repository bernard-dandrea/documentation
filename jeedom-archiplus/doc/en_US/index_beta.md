<!--  
Last Modified: June 26, 2026
-->
- [Log Management in Jeedom](#log-management-in-jeedom)
  - [How It Works](#how-it-works)
  - [Log Volume](#log-volume)
  - [The Limits of Archiving in Jeedom](#the-limits-of-archiving-in-jeedom)
  - [The Benefits of the Archiplus Plugin](#the-benefits-of-the-archiplus-plugin)
  - [Disclaimer](#disclaimer)
- [Archiplus plugin](#plugin-archiplus)
  - [Install the archiplus plugin](#install-the-archiplus-plugin)
  - [Configure the plugin](#configure-the-plugin)
  - [Plugin Modules](#plugin-modules)
- [Access to Modules](#access-to-modules)
  - [Command Buttons](#command-buttons)
  - [The row selection column](#the-row-selection-column)
  - [Column Headings](#column-headings)
  - [The lines](#the-lines)
  - [Totals at the bottom of the table](#totals-at-the-bottom-of-the-table)
- [the Monitor module](#the-monitor-module)
  - [Statistics](#statistics)
  - [Visualization](#visualization)
  - [Changes](#changes)
  - [Edits from an Excel file](#edits-from-an-excel-file)
  - [Editable data](#editable-data)
    - [KLV (Keep Last Value)](#klv-keep-last-value)
    - [Uniq](#uniq)
    - [Deadline](#deadline)
    - [Framing](#cadrage)
    - [Pond](#pond)
    - [Pack](#pack)
    - [Rounded](#rounded)
  - [Functions accessible via the context menu](#functions-accessible-via-the-context-menu)
- [Historical Data](#historical-data)
  - [Access](#access)
  - [Edit](#edit)
  - [Delete](#delete)
  - [Export](#export)
- [The Import Module](#the-import-module)
- [The Restore Module](#the-restore-module)
- [FAQ](#faq)
  - [Keep Last Value](#keep-last-value)
  - [Uniq](#uniq-1)
  - [Timeline and Scope](#timeline-and-scope)
  - [Smoothing and Weighting](#smoothing-and-weighting)
  - [Pack](#pack-1)
  - [Rounded](#rounded-1)
  - [Copy data from historyArch to history](#copy-data-from-historyArch-to-history)
  - [Using Archiplus in PHP](#using-archiplus-in-php)
- [Logs](#logs)
- [Translation](#translation)
- [Reviews](#reviews)



The plugin's main function is to provide a comprehensive set of tools that allow you to:

*   **to manage the archiving settings for INFO-type commands**
*   **to visualize data volumes and detect anomalies**
*   **easily import historical data from Excel-type files**
*   **retrieving historical data from the Jeedom archives**
*   **to expand Jeedom's standard archiving options**

Enabling the plugin's built-in archiving feature (optional) significantly expands the archiving capabilities offered by Jeedom.

# Log Management in Jeedom

## How It Works

The history feature in Jeedom has changed little since the early versions and is based on two tables:

* the "history" table, which receives updates to the values of INFO-type commands for which the history feature is enabled
* The historyArch table, which receives—during each archiving operation (usually every day at 5:00 a.m.)—the history values, whether consolidated or not, depending on the settings defined for the command.

The structure of the two tables is identical and very simple: a value is recorded for each command, along with an ID and a datetime (specified to the second).

The history can be displayed in the Jeedom interface as a graph.

The official documentation on log management in Jeedom can be found [here](https://doc.jeedom.com/fr_FR/core/4.5/history).

## Historical Data Volume

Jeedom users will start to take an interest in the history log when they notice that the database is growing excessively, that it takes a very long time to display the history, and that the backup size keeps increasing.

The following link takes you to a tutorial that explains how to create a scenario that will list the volumes of the largest tables and the INFO commands with the longest history [Tutorial - Analyzing Archives](https://community.jeedom.com/t/tuto-analyser-les-archives-pour-detecter-des-pbs-lenteurs-espaces-disques/104384).

Simply put, you can view the table sizes by querying the database directly (go to the Settings / System / Configuration menu, then select the OS / DB tab (the last one), then click the "Database Administration" button (the red button at the bottom), and then select "Size" from the query options on the left).

In a standard installation, you should start to investigate when the total volume of the logs exceeds one million records or when a `info` command returns more than 10,000 records. In this case, you’ll need to analyze the relevant commands and adjust the various logging and archiving settings to reduce this volume. If that’s not possible, you may need to consider other archiving methods, such as InfluxDB, which integrates natively with Jeedom.

The archiplus plugin immediately displays the volumes of history and historyArch, making it easy to identify problems and find solutions.

## The Limits of Archiving in Jeedom

Although standard operation will be sufficient in many installations, the following limitations should be noted:

* Difficulty viewing and modifying archiving settings: the only available tool (Analysis / History menu, then Configuration) is very slow, impractical, and offers few fields to configure
* Difficulty viewing historical volume data by command and identifying abnormal volumes: this requires using SQL queries and cumbersome processes
* Settings for data grouping in historyArch are defined globally and cannot be customized on a per-command basis
* No visibility into the archiving process (no log)
* Global Archiving: No option to initiate archiving for a specific command
* Approximate averaging
* Basic tools for exporting/importing data (dataexport plugin). There is no option to restore historical data contained in backups.

## The Benefits of the archiplus Plugin

The archiplus plugin displays INFO-type commands in a table along with all parameters related to archiving. The number of records in history and historyArch is also shown, making it very easy to detect excessive volumes. The plugin uses the Tabulator JavaScript library, which is extremely efficient and provides very easy access to the plugin’s functions.

All the features offered by Jeedom are available directly, and additional features have been added:

* Advanced Command Configuration
* Displaying Graphs and Extracting Data
* Clear History
* Standard CSV Export
* Copy the configuration from the history (or a single setting) to multiple commands
* Loading INFO command settings related to history from an Excel file
* Launch archiving for a specific command
* Copying the history from one command to another
* Copy historyArch to history to initiate interval-based consolidation
* Importing a command history from an Excel file
* Export history data in multiple formats (xlsx, CSV, JSON, HTML) for one or more commands from Jeedom or a standard Jeedom backup
* Extracting INFO command settings related to history from a Jeedom backup (these settings can then be applied to Jeedom)

In addition, the plugin's archiving process can be enabled as an alternative to the native archiving feature provided by Jeedom. This allows you to:

* to start archiving for a given command
* record in the Archiplus log all operations performed and the parameters taken into account for each command
* to customize the calculation period (for min, max, and average), the delay before archiving, and the packet size for each command
* to set the purge date to a specific day, hour, or minute
* to trigger archiving for a command from a scenario (in PHP code)
* to add options not provided by Jeedom (see the explanations later in the documentation)
  * Keep Last Value: Always retain at least one value in the history
  * Uniq: Remove consecutive identical values from historyArch
  * Weighting: In average smoothing, calculate the weighted value over the duration of the interval (rather than the average of the values)

The archiplus plugin was developed on Debian 12 and does not use jQuery (nor do the third-party libraries it uses). It complies with Jeedom's development standards. The archiplus class code is highly structured and extensively documented: the plugin's author will review all suggestions for corrections or improvements.

Since Jeedom has no plans to update its history management system, the plugin should not require a major overhaul in the near future.

## Disclaimer

The plugin and its specific logging process have been thoroughly tested but are not immune to errors. In such cases, the Jeedom team is obviously not obligated to provide support. Requests for analysis and fixes must be directed to the plugin’s author via the standard support ticket system.

Activating the plugin—and in particular, the archiving process—therefore implies full acceptance of this situation.

# Archiplus plugin

## Install the archiplus Plugin

Go to the Jeedom Market, find the archiplus plugin, and install the **stable** version. Then **enable the plugin**.

![001](../images/001.png)

The plugin is accessible via the menu.

## Configure the plugin

In the configuration, you can set the standard plugin settings and the plugin's default values.

![003](../images/003.png)

To get as much information as possible about the plugin's archiving process and the actions performed, it is recommended that you set the logs to Debug mode.

Please note that support requests must be submitted via the **Support** button.

![002](../images/002.png)

In the configuration section, you can:

* Enable specific archiving (disabled by default)
* Specify whether entries in history and historyArch should be deleted if the command in question does not exist
* Choose not to transfer history records to historyArch when there is no smoothing
* Set the format for exports
* Set the default range for purge and archive end dates

Enabling custom archiving creates a new cron job in the task engine and disables standard archiving. Disabling custom archiving performs the reverse operation.

If you want to test the plugin’s archiving process, you can temporarily enable it, run archiving tests on individual commands, and then disable the plugin’s archiving. Since Jeedom’s archiving process usually runs at 5 a.m., there will be no impact on untested commands.

## Plugin Modules

![004](../images/004.png)

From the Plugins / Monitoring / archiplus menu, you have access to all of the plugin's features

* Plugin configuration (see above)
* Access to the global settings for archiving configuration
* Monitoring: view and modify settings and perform key operations related to archiving
* Import: Import historical data from an Excel file of type Excel
* Restore: Extract historical data from a standard Jeedom archive

Historical data can be viewed from the Monitoring and Restore module.

# Access to Modules

The modules are launched from the plugin's configuration.

![005](../images/005.png)

The interface is based on a Tabulator table populated with the relevant data.

For example, with the Monitor module, a table is displayed showing INFO-type commands with the history function enabled.

The screen consists of several sections.

## Command buttons

![006](../images/006.png)

The buttons allow you to perform general actions related to the display, selected lines, updates, etc.

![013](../images/013.png)

The buttons above are common to all modules and allow you to:

* display the Archiplus log file
* to go to the first or last row of the table
* to disable the filters that have been enabled
* to return to the initial sorting
* export the data displayed in the table (filtered data only)
* to return to the various modules offered by Archiplus

![019](../images/019.png)

The standard "Help on this page" button provides access to the plugin's documentation.

## The row selection column

![007](../images/007.png)

The first column allows you to select the rows you want to modify.

Clicking on a column header selects all the rows displayed in the table.

You can select each row individually by clicking the checkbox or anywhere on the row.

You can also select a range of rows by clicking on the first row to select while holding down the Control key, then clicking on the last row while still holding down the Control key (be sure to click anywhere on the row, but not on the selection box; otherwise, the multiple selection will not work).

## Column headers

![008](../images/008.png)

The column headers describe the contents of the cells in that column.

They allow you to:

* to get additional information via a tooltip by hovering the mouse over the field for a second
* Sort the rows by the value of the field by clicking on the column header (note that the "Reset Sort" button resets all sorting).
* Filter the displayed rows by entering a selection criterion in the field located below the column name (note that the "Reset" button clears all selections).

In the case of the Monitor module, grouping columns allows you to select only certain types of information.

## The lines

![009](../images/009.png)

The lines contain the requested information.

Depending on the context, right-clicking brings up a context menu with available actions.

![010](../images/010.png)

By clicking on an editable field, you can enter a new value.

![011](../images/011.png)

Edited fields appear on a magenta background, which disappears after the changes are saved.

## Totals at the bottom of the table

![012](../images/012.png)

At the bottom of the table, the totals corresponding to the displayed or selected rows are shown.

# the Monitor module

This is the main module of archiplus.

![005](../images/005.png)

After clicking Monitor, the INFO commands with an active history are displayed within a few seconds.

![014](../images/014.png)

By clicking the button above, you can switch to a view showing all INFO commands, even those that do not require a history or those for which the device is inactive.

## Statistics

![016](../images/016.png)

The number of entries in history and historyArch generally corresponds to the number from the last archive (you can see the update date by hovering your mouse over one of the counters). By clicking on the #All column header, you can immediately see the commands with the longest history.

![015](../images/015.png)

By clicking the button above, you can rerun a calculation, which will take a few seconds.

![017](../images/017.png)

The totals at the bottom of the table let you see at a glance how large your history is.

## Visualization

![018](../images/018.png)

The display buttons allow you to select the data to be displayed

* Configuring the History
* calculations
* prohibited values
* display via graphs
* statistics

Depending on what interests you, you can choose to enable or disable the sections you want to manage. To keep the Monitor home screen uncluttered, only identification, configuration, and statistics data are displayed.

## Changes

![020](../images/020.png)

To change a value, simply click on the relevant field and enter a new value.

![021](../images/021.png)

Changes to the data are highlighted in magenta.

![022](../images/022.png)

By right-clicking on a line, you can copy its configuration or one of its settings to the selected lines.

![023](../images/023.png)

To review the data before validation, you can choose to display only the modified rows.

![024](../images/024.png)

After clicking the "Validate" button, the data is updated and the background of the modified cells is cleared.

![025](../images/025.png)

Note that right-clicking on a line allows you to directly launch Jeedom's advanced command configuration.

## Edits from an Excel file

![070](../images/070.png)

You can also upload changes from an Excel or CSV file by clicking the Import button. This allows you to select the file and upload the updated data to the table.

![071](../images/071.png)

The data must be in the same format as that generated by the export. Therefore, you can export the data, edit it in Excel, and then upload the changes to the table.

It is also possible to extract the backup settings from a Jeedom backup and load the changes: this allows you to quickly see the changes made since the backup and, if necessary, revert to a previous state.

![072](../images/072.png)

Once the import is complete, you can view only the modified data by clicking the "Updates" filter. You can also click the display buttons (Configuration, Calculations, etc.) to view all editable data.

To apply the changes, click the "Confirm" button.

## Editable data

All configuration settings for Jeedom's standard history and those specific to the archiplus plugin can be modified directly from Monitor.

Below are the options specific to Archiplus:

### KLV (Keep Last Value)

Ensures that at least one entry is always retained in the history. See the following FAQ to understand how to use this option [Keep Last Value](#keep-last-value).

### Uniq

Removes consecutive identical values from historyArch (and possibly history). See the following FAQ to understand how to use this [Uniq](#uniq-1) option.

### Timeframe

This is the time interval after which history records are transferred to historyArch. By default in Jeedom, this setting is the same for all commands. With archiplus, this interval can be specified on a per-command basis.

### Scope

Allows you to set the time up to which historical data is purged, as well as the time at which historical data is transferred to historyArch, based on a limit of days, hours, or minutes. See the following FAQ to understand how to use this option [Timeout and Time Frame](#timeout-and-time-frame).

### Pond

Allows you to calculate a time-weighted average rather than a simple average of the values recorded over the period. See the following FAQ to understand how to use this option [Smoothing and Weighting](#smoothing-and-weighting).

### Pack

Defines the interval at which data will be aggregated during smoothing. In Jeedom’s standard archiving, this setting is the same for all commands and is a multiple of hours. With archiplus, you can specify the interval for each command and also express the value in minutes (enter the number of minutes followed by the letter m). See the following FAQ to understand how to use this option [Pack](#pack-1).

### Rounded

By default in Jeedom, you can specify the rounding method for each command. The plugin also allows you to specify a different rounding method when smoothing data in historyArch. See the following FAQ to understand how to use this [Rounding](#arrondi-1) option.

## Functions accessible via the context menu

![026](../images/026.png)

Right-clicking anywhere on a row in the table brings up the command's context menu. In addition to the actions already discussed, this menu allows you to:

* display the history as a graph  (using Jeedom's standard function)
* to display the data stored in the history and historyArch tables
* to clear the history up to a specific date
* export historical data in CSV format (using Jeedom's standard function)
* to update the statistics for the relevant line
* to start archiving only for the specific command
* Copy data from historyArch to history: See the following FAQ to understand how to use this action  [historyArch to history](#copy-data-from-historyarch-to-history)
* to copy the history of the selected command to another command

# Historical Data

## Access

![027](../images/027.png)

Data in the history and historyArch tables can be accessed via:

* the Monitor context menu (see above)
* Select one or more lines, then press the Data button.

![028](../images/028.png)

The data is displayed in a modal window, sorted by datetime in descending order.

## Modification

![029](../images/029.png)

Sometimes we get outliers; in this case, they were caused by boiler maintenance.

![030](../images/030.png)

Use the context menu to edit or delete the value in question.

![031](../images/031.png)

After the correction, the history display is now much more meaningful.

## Delete

![032](../images/032.png)

You can also delete multiple historical data entries by selecting them and clicking the "Delete" button.

## Export

![033](../images/033.png)

The Export button allows you to export the data.

Note that these files can be edited in Excel so they can be imported using the Import module.

# The Import Module

The Import module allows you to import historical data into one or more INFO-type commands.

![035](../images/035.png)

The file to be imported must be of Excel or CSV type and must contain at least the following 3 columns (any others will be ignored):

* id: Command ID
* datetime: historical data in the format YYYY-MM-DD HH:MM:SS (Excel's internal datetime format is also supported)
* value: value to import

Note that the data extracted from the Monitor or Restore modules is in the correct format.

![034](../images/034.png)

The first action is to select the file containing the data.

![036](../images/036.png)

After loading, the historical data from the file is loaded.

The data from the INFO command is retrieved from Jeedom.

A check is performed, and erroneous data is detected.

![037](../images/037.png)

You can assign loaded lines to a different command by selecting the relevant line(s) and clicking the "Change Command" button.

![038](../images/038.png)

To import historical data into Jeedom, select the relevant row(s) (here, filter by a date range) and click the "Import" button. Rows with errors are ignored.

![039](../images/039.png)

Note that the import is performed using the standard cmd::addHistoryValue method. Therefore, Jeedom's standard checks and processing are carried out. The new entries are stored in the history table.

# The Restore module

The Restore module allows you to retrieve historical data from a standard Jeedom archive and export it so that you can import it using the Import module.

All processing is performed locally in the web browser. All commands and historical data are loaded into the browser's memory. The program has been tested with 1.5 million lines in history and historyArch. The maximum amount of data that can be loaded depends on the amount of RAM allocated to the browser and cannot be determined in advance. However, it should be able to load most backups where the history has not become excessively large.

![040](../images/040.png)

The first step is to restore the backup locally to your computer. See the following documentation for managing Jeedom backups [here](https://doc.jeedom.com/fr_FR/core/4.5/backup).

![041](../images/041.png)

Launch the Restore module and select the archive you want to use.

![042](../images/042.png)

After a few seconds, the commands with a history are displayed.

![043](../images/043.png)

You can select the commands you're interested in and start the export.

![044](../images/044.png)

You can also view the relevant historical data and select which data to export.

![045](../images/045.png)

In both cases, you'll find an export file that you can use to perform an import using the Import module.


![073](../images/073.png)

By clicking the view buttons, you can display the settings for the INFO commands as they were at the time they were saved. The "All" filter allows you to view all INFO commands.

The Export button allows you to generate a file that can be used to load configuration changes since the last backup into the Monitor module.

# FAQ

## Keep Last Value

In some cases, it is necessary to have the latest value of the INFO command.

![046](../images/046.png)

Let's consider the case of a furnace for which the gas meter used for heating is read periodically.

![047](../images/047.png)

A scenario that runs every hour calculates hourly consumption by comparing the historical values at the beginning and end of the hour. To do this, one day’s worth of historical data is sufficient.

However, once the heating season is over, the heating meter history is lost and is no longer available to calculate the initial hourly consumption when heating is first turned on for the following season.

Enabling the "Keep Last Value" option solves this problem without having to resort to programming workarounds or maintain a year's worth of history.

## Uniq

Jeedom prevents duplicate entries in the history table with the "Repeat identical values" option, which is disabled by default.

However, there are several situations in which consecutive identical values are not ignored:

  * if the command type is Binary or Other
  * if the update is performed using the cmd::event method rather than eqLogic::checkAndUpdateCmd. Many plugins still use the older cmd::event method, which does not eliminate duplicates.

During archiving, if no smoothing is performed, the history data is transferred directly to historyArch, and duplicates are therefore copied.

Enabling the Uniq option removes duplicates from historyArch during archiplus-specific archiving.

Additionally, if the plugin is configured not to copy history entries to historyArch, duplicates in history will also be deleted.

## Timeline and Scope

By default, the time at which data is deleted from history and historyArch is defined by the "Clear History" parameter, expressed in hours. A default value is set in Jeedom's global configuration.

Thus, with a purge interval set to 7 days, if the archiving process is started on January 20, 2025, at 5:11:21 a.m., the "history" and "historyArch" records will be deleted up to January 13, 2025, at 5:11:21 a.m.

The "Framing" setting specific to Archiplus allows you to set the timing of the purge more precisely. Thus, in the example above, the purge will occur at:

* on 01/13/2025 at 5:11:21 a.m. if no frame is defined
* on January 13, 2025, at 5:11:00 a.m., with a close-up on the last minute
* on January 13, 2025, at 5:00:00 a.m., focusing on the last hour
* on January 13, 2025, at 12:00:00 a.m., focusing on the last day

The "Time Before Archiving" (in hours) determines when history records are transferred to historyArch (with or without consolidation). By default, this setting is defined globally and is therefore the same for all commands.

Archiplus's specific archiving feature allows you to set a specific time limit for each INFO command and use the framework shown above. Thus, with a 2-hour time limit, the time when history is transferred to historyArch will be:

* on 01/20/2025 at 3:11:21 a.m. if no frame is defined
* on January 20, 2025, at 3:11:00 a.m., with a close-up on the last minute
* on January 20, 2025, at 3:00:00 a.m., focusing on the last hour
* on January 20, 2025, at 12:00:00 a.m., with a focus on the last day—regardless of the time of day when the archiving process is initiated

Note that the purge time cannot be later than the time the history is transferred to historyArch and will therefore be adjusted automatically.

![048](../images/048.png)

You can adjust these settings if, for example, you want a detailed history over a short period (in this case, a maximum of 36 hours) without needing consolidated archiving. This prevents the transfer of history to `historyArch`, which serves no purpose.

## Smoothing and Weighting

Smoothing occurs when history data is copied to historyArch. The archiving process considers all history data within the defined interval (one hour by default) and retains a single value calculated according to the smoothing mode. Three modes are available:

* minimum: the smallest value in the range
* maximum: the largest value in the interval
* average: the average of the values in the interval

It should be noted that standard logging does not take into account the command value at the beginning of the interval and instead calculates an average of the values within the interval, which can significantly skew the result.

Archiplus's specific archiving process offers a "Pond" option that corrects this issue and calculates an exact result for the specified interval.

This is illustrated in the example below.

![050](../images/050.png)

Let's consider two commands with the following configurations.

![049](../images/049.png)

They have the same entries in the history table

![051](../images/051.png)

After archiving, the entries in historyArch are different

![052](../images/052.png)

With standard archiving, the average of the values over the period is used.

With archiplus's specific archiving feature, the weighted average for the period is calculated. Note also that an entry is added to the history so that the starting value for the period is known during the next archiving (without this entry, the average from the previous period would be used, which would skew the calculation).

## Pack

By default in Jeedom, the interval (called a "packet" in Jeedom) over which smoothing can be applied is defined in hours and is the same for all INFO commands.

However, it might be desirable to have a shorter interval and to be able to specify it for a particular INFO command.

![055](../images/055.png)

![054](../images/054.png)

For a battery, storing one value per day over a long period of time may be sufficient.

![057](../images/057.png)

![056](../images/056.png)

For a thermometer, a reading every quarter hour may be more useful than one reading per hour.

To specify minutes, enter the desired number of minutes followed by "m" in the "Pack" field, for example, 15m.

## Rounded

By default, Jeedom allows you to specify the number of decimal places for an INFO command value.

For certain commands, it may be useful to have a precise value for a short period of time and then a less precise value afterward. For example, knowing the exact outdoor temperature is useful at the moment but is no longer necessary after several days.

![064](../images/064.png)

The command above is configured to store a history with one decimal place for one week and a history without decimals beyond that.

![065](../images/065.png)

Before archiving, there are 7 entries in the history ranging from 7.7 °C to 8.3 °C.

![066](../images/066.png)

After archiving, the 7 entries are rounded to 8 °C, and the Uniq option allows you to keep only one of them.

## Copy data from historyArch to history

After installing archiplus, you may want to consolidate existing logs.

![060](../images/060.png)

![058](../images/058.png)

For example, for this command, a history recorded at 10-minute intervals would be sufficient and would significantly reduce the number of entries in historyArch.

![059](../images/059.png)

After changing the settings, you can transfer the entries from historyArch to history.

![061](../images/061.png)

Once this update is complete, you can run an archive using the INFO command (or wait for the archive to run automatically at night).

![063](../images/063.png)

![062](../images/062.png)

After archiving, the number of records is significantly reduced, and the history displays much faster.

## Using Archiplus in PHP

You can call the archiplus functions for archiving and processing history data directly from a scenario or a PHP function.

![053](../images/053.png)

Here, the archiplus functions are used in a scenario to load the history of a command and initiate archiving for that command.

`require_once dirname(__FILE__) . '../../../plugins/archiplus/core/class/archiplus.class.php';`

This line loads the code for the Archiplus functions. You may need to adjust the path to point to the plugin class.

The available functions can be found in the archiplus class code. The main ones are:

* `archive($_cmd_id = '')`: starts the archiving process for a single command or for all commands if no parameter is provided
* `History_purge($_cmd_id, $_date='')`: deletes the history for a command up to a specified datetime (or the entire history if no second parameter is provided)
* `addHistoryValue($_cmd_id, $_datetime, $_value)`: adds an entry (or replaces the existing entry) to the history by calling the standard Jeedom function
* `historyArch2history($_cmd_id, $_date_start = '', $_date_end = '')`: transfers records from historyArch to history
  
It is, of course, possible to use the functions available in the history.class.php class after making the necessary `require_once` declaration.

# Logs

If the log level in the plugin's configuration is set to at least "Info," various events related to Archiplus will be recorded in the Jeedom Archiplus log. You can access it directly using the "Log" button found in the various Archiplus modules.

![068](../images/068.png)

When archiving, Jeedom's general archiving settings are displayed.

![067](../images/067.png)

Next, for each command, the operations performed and the number of entries in history and historyArch before and after that command are listed in detail.

![069](../images/069.png)

You can view the log for a specific command by entering its number preceded by the characters "-" and a space in the search field.

# Translation

The interface, log messages, and documentation have been translated into the 5 languages supported by Jeedom (thanks to @mips for developing ga-translation and docs-translations). If you find any translation errors, please open a support ticket and, if possible, attach the corrected translation file (located in the plugin’s core/i18n directory).

# Reviews

![archiplus_review](../images/archiplus_avis.png)

If you like this plugin, please leave a rating and a comment on the Jeedom Market—it’s always appreciated: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4679#>
