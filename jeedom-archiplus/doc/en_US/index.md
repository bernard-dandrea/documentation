<!--  
  Last Modified : 2026/02/24 18:32:45
-->
- [History management in Jeedom](#history-management-in-jeedom)
  - [Functioning](#functioning)
  - [History volume](#history-volume)
  - [The limits of archiving in Jeedom](#the-limits-of-archiving-in-jeedom)
  - [The ADVANTAGES of the archiplus plugin](#the-advantages-of-the-archiplus-plugin)
  - [Warning](#warning)
- [Plugin archiplus](#plugin-archiplus)
  - [Install the archiplus Plugin](#install-the-archiplus-plugin)
  - [Configure the plugin](#configure-the-plugin)
  - [Plugin modules](#plugin-modules)
- [Access to modules](#access-to-modules)
  - [The command buttons](#the-command-buttons)
  - [The row selection column](#the-row-selection-column)
  - [Column headers](#column-headers)
  - [The lines](#the-lines)
  - [Bottom of table totals](#bottom-of-table-totals)
- [the Monitor module](#the-monitor-module)
  - [Statistics](#statistics)
  - [Visualisation](#visualisation)
  - [Modifications](#modifications)
  - [Editable data](#editable-data)
    - [KLV (Keep Last Value)](#klv-keep-last-value)
    - [unique](#unique)
    - [Deadline](#deadline)
    - [Framing](#framing)
    - [Pond](#pond)
    - [Pack](#pack)
    - [Rounded](#rounded)
  - [Functions accessible via the context menu](#functions-accessible-via-the-context-menu)
- [Historical data](#historical-data)
  - [Access](#access)
  - [Modification](#modification)
  - [Suppression](#suppression)
  - [Export](#export)
- [The Import module](#the-import-module)
- [The Restore module](#the-restore-module)
- [FAQ](#faq)
  - [Keep Last Value](#keep-last-value)
  - [unique](#unique-1)
  - [Deadline and Framing](#deadline-and-framing)
  - [Smoothing and weighting](#smoothing-and-weighting)
  - [Pack](#pack-1)
  - [Rounded](#rounded-1)
  - [Copy data from historyArch to history](#copy-data-from-historyarch-to-history)
  - [Using archiplus in PHP](#using-archiplus-in-php)
- [Les logs](#les-logs)
- [Translation](#translation)
- [Avis](#avis)



The main function of the plugin is to provide a complete set of tools allowing:

*   **to manage the archiving parameters of INFO type orders**
*   **to visualize data volumes and detect anomalies**
*   **Easily insert historical data from Excel files**
*   **recover histories from the Jeedom archives**
*   **to extend Jeedom's standard archiving options**

The optional activation of archiving integrated into the plugin allows you to very significantly extend the archiving functions offered by Jeedom.

# History management in Jeedom

## Functioning

The history in Jeedom has changed little since the first versions and is based on 2 tables:

* the history table which receives updates to the values ​​of INFO type commands for which history is enabled
* the historyArch table which receives during each archiving (usually every day at 5:00 a.m.) the history values, consolidated or not, depending on the settings defined for the command.

The structure of the two tables is identical and very simple: a value is recorded by Id and datetime command (managed to the second). 

The history can be displayed in the Jeedom interface in the form of a graph.

The official documentation regarding history management in Jeedom can be found [ici](https://doc.jeedom.com/fr_FR/core/4.5/history).

## History volume

The Jeedom user will begin to be interested in history when he notices a database that grows exaggeratedly, history display times that become very long, a backup size that continues to grow.

The following link provides access to a tutorial that explains how to create a scenario that will list the volumes of the largest tables and the INFO commands with the largest histories [Tutorial - Analyze archives](https://community.jeedom.com/t/tuto-analyser-les-archives-pour-detecter-des-pbs-lenteurs-espaces-disques/104384).

More simply, you can see the table volumes by directly querying the database (Settings / System / Configuration menu then OS / DB tab (the last) then "Database Administration" button (lowest red button) then on the left "size" query).

In a standard installation, you should start asking questions when the overall volume of the archives exceeds one million records or an info command has more than 10,000 records. In this case, it is necessary to analyze the commands concerned and adjust the various parameters of historization and archiving in order to reduce this volume. If this is not possible, it may be necessary to turn to other archiving methods, for example influxDB which can interface as standard with Jeedom.

The archiplus plugin immediately gives the volumes of history and historyArch and makes it easy to target problems and provide solutions.

## The limits of archiving in Jeedom

Although in many installations standard operation will be sufficient, the following limitations can be noted:

* Difficulty viewing and modifying archiving parameters: the only tool available (Analysis / History menu then Configuration) is very slow, impractical and has few fields to configure
* Difficulty viewing historical volumes by order and spotting abnormal volumes: you have to go through SQL queries and impractical processes 
* Parameters for data grouping in historyArch defined globally and not customizable by command
* No visibility regarding the archiving process (no log)
* Global archiving: no possibility of launching archiving for a specific order
* Approximate averaging smoothing
* Basic tools for exporting/importing data (dataexport plugin). Nothing is offered to restore historical data contained in backups.

## The ADVANTAGES of the archiplus plugin

The archiplus plugin allows you to view INFO type commands in a table with all the parameters relating to archiving. The number of records in history and historyArch is also presented making it very easy to detect excessive volumes. The plugin uses the JavaScript Tabulator library which is extremely powerful and allows very easy access to the plugin's functions.

All functions offered by Jeedom are available directly and other functions have been added:

* Advanced order configuration
* Viewing graphs and extracting data
* Purge history
* Export standard CSV
* Copying history configuration (or a single parameter) to multiple commands
* Launching archiving for a given order
* Copying order history to another order
* Copying historyArch to history to initiate interval consolidation
* Importing order history from an Excel file
* Extraction of history in several formats (xlsx, CSV, JSON, HTML) of one or more orders from Jeedom or a standard Jeedom backup

Additionally, the plugin's archiving process can be activated as a replacement for the native archive function offered by Jeedom. This allows:

* to start archiving for a given order
* to record in the archiplus log all the operations carried out and the parameters taken into account for each command
* customize the calculation period (for min, max, average), the time before archiving and the packet size for each order
* to set the purge date to a day, an hour or a minute
* to launch archiving for a command from a scenario (in PHP code)
* to add options not provided in Jeedom (see the explanations later in the documentation)
  * Keep Last Value: always keep at least one value in history
  * Uniq: eliminate identical consecutive values ​​in historyArch
  * Pond: in average smoothing, calculate the weighted value over the duration of the interval (and not the average of the values)

the archiplus plugin was developed under Debian 12 and does not use jQuery (nor do the 3rd party libraries used). It respects Jeedom development standards. The code of the archiplus class is very structured and widely documented: the author of the plugin will study all suggestions for correction or improvement.

Since Jeedom has no history management evolution plan, the plugin should not require an overhaul in the near future. 

## Warning

The plugin and its specific archiving process have been tested very rigorously but are not immune to anomalies. In this case, the Jeedom team is obviously not required to provide support. Requests for analysis and correction must be sent to the author of the plugin via the standard support request. 

Activation of the plugin and in particular of the archiving process therefore imply full acceptance of this situation.

# Plugin archiplus

## Install the archiplus Plugin

Go to the Jeedom Market, find the archiplus plugin and install the **stable** version. Then **Activate the plugin**.

![001](../images/001.png)

The plugin is accessible via the menu.

## Configure the plugin

In the configuration, you can configure the usual plugin settings and plugin default values.

![003](../images/003.png)

To have maximum information on the plugin archiving process and the actions performed, it is recommended to put the logs in Debug mode.

Note that support requests must be made via the **Assistance** button.

![002](../images/002.png)

In the configuration section you can:

* Enable specific archiving (disabled by default)
* Indicate whether records in history and historyArch should be deleted in case the relevant command does not exist
* Define the format for exports
* Set default framing for purge and archive end dates

Enabling specific archiving creates a new cron in the task engine and disables standard archiving. Disabling specific archiving performs the opposite operation.

If you want to test the plugin's archiving process, you can temporarily enable it, do archiving tests on individual orders and then disable plugin archiving. Since Jeedom's archiving process usually launches at 5 a.m., there will be no impact on untested orders.

## Plugin modules

![004](../images/004.png)

From the Plugins / Monitoring / archiplus menu, you have access to all of the plugin's functions

* Plugin configuration (see above)
* Access to global archiving settings
* Monitoring: view and modify the settings and carry out the main operations relating to archiving
* Import: import historical data from an Excel file
* Restore: extract historical data from a standard Jeedom archive

Viewing historical data is accessible from the Monitoring and Restore module.

# Access to modules

Modules are launched from the plugin configuration.

![005](../images/005.png)

The basis of the interface is a Tabulator table populated with the relevant data.

For example, with the Monitor module, a table is displayed with INFO type commands having the history function activated.

The screen has several parts.

## The command buttons 

![006](../images/006.png)

The buttons allow global actions concerning the display, selected lines, updates, ...

![013](../images/013.png)

The buttons above are common to all modules and allow:

* to display the archiplus log file
* to go to the first or last row of the table
* to cancel filters that have been activated
* to return to the initial sort
* export the data displayed in the table (only filtered data)
* to return to the different modules offered by archiplus

![019](../images/019.png)

The standard “Help on current page” button provides access to the plugin documentation.

## The row selection column

![007](../images/007.png)

The first column allows you to select the lines on which you wish to act.

By clicking on the column headers, we select all the rows displayed in the table.

Each line can be selected individually by clicking on the check box or anywhere on the line.

You can also select a series of lines by clicking on the first to select then clicking on the last while holding down the SHIFT key.

## Column headers

![008](../images/008.png)

Column headers describe the contents of the cells located in the column.

They allow:

* to obtain additional information via a tooltip by leaving the mouse on the field for a second
* to sort the rows according to the value of the field by clicking on the column label (note that the "Initial sort" button allows you to cancel all the sorts carried out)
* to filter the rows displayed by entering a selection criterion in the field located under the column name (note that the "Reset" button allows you to cancel all selections).

In the case of the Monitor module, grouping the columns allows you to select only certain types of information.

## The lines

![009](../images/009.png)

The lines present the requested information.

Depending on the context, a right click brings up a context menu with possible actions.

![010](../images/010.png)

By clicking on an editable field, you can enter a new value.

![011](../images/011.png)

The modified fields appear on a magenta background which disappears after validation of the modifications.

## Bottom of table totals

![012](../images/012.png)

At the bottom of the table, the totals corresponding to the displayed or selected lines are displayed.

# the Monitor module

This is the main module of archiplus.

![005](../images/005.png)

After clicking Monitor, INFO commands with active history are displayed within seconds.

![014](../images/014.png)

By clicking on the button above, you can switch to the display of all INFO commands, even those which do not require history or those for which the equipment is inactive.

## Statistics

![016](../images/016.png)

The number of records in history and historyArch generally corresponds to that of the last archiving (you can see the update date by leaving the mouse on one of the counters). By clicking on the #All column header, you can immediately see the orders with the largest history.

![015](../images/015.png)

By clicking on the button above, you can restart a calculation which will take a few seconds.

![017](../images/017.png)

The totals at the bottom of the table allow you to immediately know the size of your history.

## Visualisation

![018](../images/018.png)

The visualization buttons allow you to select the data displayed

* history configuration
* the calculations
* forbidden values
* display via graphics
* the statistics

Depending on what interests you, you can activate or not the part you want to manage. In order not to overload the Monitor start screen, only identification data, configuration data and statistics are presented.

## Modifications

![020](../images/020.png)

To modify data, simply click on the area concerned and enter a new value. 

![021](../images/021.png)

The modified data appears on a magenta background. 

![022](../images/022.png)

With a right click on a line, it is possible to copy its configuration or one of its parameters to the selected lines. 

![023](../images/023.png)

In order to check the data before validation, it is possible to display only the modified lines. 

![024](../images/024.png)

After clicking on the Validate button, the data is updated and the background of the modified cells is cleared.

![025](../images/025.png)

Note that right-clicking on a line directly launches Jeedom's advanced command configuration.

## Editable data

All configuration data from the standard Jeedom history and those specific to the archiplus plugin can be modified directly from Monitor.

Below are detailed the options specific to archiplus:

### KLV (Keep Last Value) 

Allows you to always keep at least one record in history. See the following FAQ to understand the use of this option [Keep Last Value](#keep-last-value).

### unique 

Allows you to remove identical consecutive values ​​in historyArch. See the following FAQ to understand the use of this option [unique](#uniq-1).

### Deadline

This is the time limit from which records are transferred from history to historyArch. As standard in Jeedom, this parameter is the same for all commands. With archiplus, this deadline can be specified per order.

### Framing 

Allows you to set the time until which historical data is purged and also that of transferring data from history to historyArch on a limit of day, hour or minute. See the following FAQ to understand the use of this option [Deadline and Framing](#délai-et-cadrage).

### Pond

Allows you to make a weighted average taking into account time and not an average of the values ​​recorded over the period. See the following FAQ to understand the use of this option [Smoothing and weighting](#lissage-et-pondération).

### Pack

Defines at what interval the data will be grouped during smoothing. In standard Jeedom archiving, this parameter is the same for all orders and is a multiple of hours. With archiplus, you can specify the interval for each command and also express the value in minutes (enter the number of minutes followed by the letter m).  See the following FAQ to understand the use of this option [Pack](#pack-1).

### Rounded

As standard in Jeedom, you can specify rounding for each order. The plugin also allows you to specify a different rounding when smoothing data in historyArch. See the following FAQ to understand the use of this option [Rounded](#arrondi-1).

## Functions accessible via the context menu

![026](../images/026.png)

By right-clicking anywhere on a row in the table, the context menu of the command appears. In addition to the actions already seen, this allows:

* to display the history in graph form (call of the standard Jeedom function)
* display data stored in the history and historyArch tables
* to purge the history up to a given date
* to export historical data in CSV format (call of the standard Jeedom function)
* to update the statistics for the line concerned
* to launch archiving only for the order concerned
* to copy data from historyArch to history: See the following FAQ to understand the use of this action  [historyArch vers history](#copier-les-données-de-historyarch-vers-history)
* to copy the history of the selected command to another command

# Historical data

## Access

![027](../images/027.png)

Access to data in the history and historyArch tables is via:

* the Monitor context menu (see above)
* the selection of one or more lines followed by pressing the Data button.

![028](../images/028.png)

The data is presented in a modal window sorted by descending datetime.

## Modification 

![029](../images/029.png)

It sometimes happens that we have aberrant values, here following boiler maintenance.

![030](../images/030.png)

The context menu allows you to modify or delete the value concerned. 

![031](../images/031.png)

After correction, the display of the history is then much more meaningful.

## Suppression

![032](../images/032.png)

It is also possible to delete several historical data by selecting them and clicking on the delete button.

## Export

![033](../images/033.png)

The Export button allows you to export the data.

Note that these can be reworked in Excel in order to be imported via the Import module.

# The Import module

The Import module allows you to import historical data into one or more INFO type commands.

![035](../images/035.png)

The file to import must be of Excel or CSV type and must contain at least the following 3 columns (the others will be ignored):

* id: Order ID
* datetime: datetime of historical data in the format YYYY-MM-DD HH:MM:SS (the internal Excel datetime format is also supported)
* value: value to import

Note that the data extracted from the Monitor or Restore modules are in the correct format.

![034](../images/034.png)

The first action to perform is to select the file containing the data.

![036](../images/036.png)

After loading, the historical data of the file is loaded. 

The INFO command data is extracted from Jeedom.

A check is carried out and the error data is detected.

![037](../images/037.png)

It is possible to assign the loaded lines to another order by selecting the line(s) concerned and clicking on the “Change Order” button.

![038](../images/038.png)

To import historical data into Jeedom, you must select the row(s) concerned (here filter on a date range) and click on the "Import" button. Error lines are ignored.

![039](../images/039.png)

Note that the import is carried out using the standard cmd::addHistoryValue method. Also, standard Jeedom checks and treatments are carried out. New entries are found in the history table.

# The Restore module

The Restore module allows you to extract historical data from a standard Jeedom archive and export them in order to be able to import them with the Import module.

All processing is carried out locally on the WEB browser. All commands and historical data are loaded into the browser's memory. The program has been tested with 1.5 million rows in history plus historyArch. The maximum number of data loaded depends on the RAM allocated to the browser and cannot be known a priori. It should, however, be able to load most saves where the history hasn't exploded.

![040](../images/040.png)

The first step is to repatriate the backup locally to the computer. See the following documentation for managing Jeedom backups [ici](https://doc.jeedom.com/fr_FR/core/4.5/backup).

![041](../images/041.png)

Launch the Restore module and select the archive you want to use.

![042](../images/042.png)

After a few seconds, commands with a history are displayed.

![043](../images/043.png)

You can select the commands that interest you and start the export.

![044](../images/044.png)

You can also view the relevant historical data and select which ones to export.

![045](../images/045.png)

In both cases, you will find an export that you can use to perform an import with the Import module.

# FAQ

## Keep Last Value

In some cases it is necessary to have the latest value of the INFO command.

![046](../images/046.png)

Let's take the case of a boiler where the gas meter allocated to heating is periodically read. 

![047](../images/047.png)

A scenario executed every hour allows the hourly consumption to be calculated by making the difference between the value in the history at the start and end of the hour. To do this, a one-day history is sufficient.

However, when the heating season is over, the history of the heating meter has disappeared and it is no longer available to calculate the first hourly consumption during the first heating of the following season.

Activating the Keep Last Value option makes it possible to overcome this problem without having to resort to programming tricks or keep a history over a year.

## unique

Jeedom allows you to avoid duplicates in the history table with the "Repeat identical values" option which is disabled by default.

There are, however, several situations in which identical consecutive values ​​are not ignored:

  * if the subtype of the command is Binary or Other
  * if the update is performed with the cmd::event method and not eqLogic::checkAndUpdateCmd. Many plugins still work with the older cmd::event method and therefore do not eliminate duplicates.

When archiving, if there is no smoothing, the history data is transferred directly to historyArch and therefore duplicates are copied.

Enabling the Uniq option allows you to remove duplicates in historyArch when specifically archiving archiplus.

## Deadline and Framing

As standard, the time from which data is deleted in history and historyArch is defined by the "Purge history" parameter expressed in hours. A default value is defined in the Jeedom global configuration.

Thus, with a purge set to 7 days, if archiving is launched on 01/20/2025 at 05:11:21, the history and historyArch records will be deleted until 01/13/2025 at 05:11:21. 

The Framing parameter specific to archiplus allows you to set the time of the purge more precisely. So, in the example above, the purge time will be:

* on 01/13/2025 at 05:11:21 if no framing is defined
* on 01/13/2025 at 05:11:00 with last-minute framing
* on 01/13/2025 at 05:00:00 with a focus on the last hour
* on 01/13/2025 at 00:00:00 with a focus on the last day

The "Delay before archiving" (in hours) allows you to determine from when history records are transferred to historyArch (with or without consolidation). As standard, it is defined globally and is therefore identical for all commands. 

The specific archiving of archiplus allows you to define a specific delay for each INFO command and to use the framing seen above. So with a delay of 2 hours, the transfer time from history to historyArch will be:

* on 01/20/2025 at 03:11:21 if no framing is defined
* on 01/20/2025 at 03:11:00 with last-minute framing
* on 01/20/2025 at 03:00:00 with a focus on the last hour
* on 01/20/2025 at 00:00:00 with a framing on the last day, here whatever the time of day when archiving is launched

Note that the time of the purge cannot be later than the time of transfer from history to historyArch and will therefore be adjusted automatically.

![048](../images/048.png)

We can play on these parameters if we want, for example, a detailed history over a short period (here 36 hours maximum) without the need for consolidated archiving. This avoids the transfer of history to historyArch which adds nothing.

## Smoothing and weighting

Smoothing occurs when copying data from history to historyArch. The archiving process considers all history data according to the defined interval (by default one hour) and keeps a single value calculated according to the smoothing mode. Three modes are possible:

* minimum: the smallest of the values ​​contained in the interval
* maximum: the largest of the values ​​contained in the interval
* average: the average of the values ​​contained in the interval

It should be noted that standard archiving does not take into account the value of the order at the start of the interval and takes an average of the values ​​present in the interval, which can significantly distort the result. 

The specific archiplus archiving process offers a Pond option which allows you to correct this phenomenon and calculate an exact result over the interval considered.

This is illustrated in the example below.

![050](../images/050.png)

Consider two commands with the following configurations.

![049](../images/049.png)

They have the same entries in the history table

![051](../images/051.png)

After archiving, the entries in historyArch are different

![052](../images/052.png)

With standard archiving, the average of the values ​​over the period is taken into account.

With the specific archiplus archiving, the weighted average over the period is calculated. Also note that an entry is added in history in order to know the starting value of the period during the next archiving (without this entry, we would recover the average of the last period which would distort the calculation).

## Pack

As standard in Jeedom, the interval (called packet in Jeedom) over which smoothing can be done is defined in hours and is the same for all INFO commands.

However, one may want a smaller interval and be able to specify it for a particular INFO command.

![055](../images/055.png)

![054](../images/054.png)

For a battery, maintaining a value per day over a long period of time may be sufficient.

![057](../images/057.png)

![056](../images/056.png)

For a thermometer, a reading every quarter of an hour may be more useful than a reading per hour.

To indicate minutes, enter in the Pack area the desired number of minutes followed by m, for example 15m.

## Rounded

As standard, Jeedom allows you to specify the number of decimal places of an INFO command value. 

For certain orders, it may be interesting to have a precise value over a short period and then less precise later. For example, knowing a precise outside temperature is interesting at the time but is no longer necessary after several days.

![064](../images/064.png)

The above command is configured to keep a history with 1 decimal place for one week and a history without decimal places beyond that.

![065](../images/065.png)

Before archiving, we have 7 entries in the history between 7.7 °C and 8.3 °C.

![066](../images/066.png)

After archiving, the 7 entries are rounded to 8°C and the Uniq option allows only one to be kept.

## Copy data from historyArch to history

After installing archiplus, you may want to consolidate existing histories.

![060](../images/060.png)

![058](../images/058.png)

For example, for this command, a history in 10 minute intervals would be sufficient and would greatly reduce the number of records in historyArch.

![059](../images/059.png)

After changing the settings, you can transfer the entries from historyArch to history.

![061](../images/061.png)

Once this update has been carried out, you can launch an archiving using this INFO command (or wait for the archiving to be launched automatically at night).

![063](../images/063.png)

![062](../images/062.png)

After archiving, the number of recordings is greatly reduced and the display of history is much faster.

## Using archiplus in PHP

It is possible to call the archiplus archiving and history processing functions directly in a scenario or a PHP function.

![053](../images/053.png)

Here, the archiplus functions are used in a scenario to load the history of an order and start archiving on it.

`require_once dirname(__FILE__) . '../../../plugins/archiplus/core/class/archiplus.class.php';`

This line is used to load the code for archiplus functions. It may be necessary to adapt the path to point to the plugin class.

The usable functions can be found in the code of the archiplus class. The main ones are:

* `archive($_cmd_id = '')` : launches archiving for an order or all orders if there is no parameter
* `History_purge($_cmd_id, $_date='')` : deletes the history for a command up to a specific datetime (or all history if no second parameter)
* `addHistoryValue($_cmd_id, $_datetime, $_value)` : adds an entry (or replaces the existing entry) in the history by calling the standard Jeedom function
* `historyArch2history($_cmd_id, $_date_start = '', $_date_end = '')` : transfer records from historyArch to history
  
It is obviously possible to use the functions available in the history.class.php class after making the declaration `require_once` necessary.

# Les logs

If the log level in the plugin configuration is set to at least Info, the various events linked to archiplus will be recorded in the Jeedom archiplus log. It can be accessed directly with the log button present in the different archiplus modules.

![068](../images/068.png)

When archiving, the general Jeedom archiving settings are displayed.

![067](../images/067.png)

Then, for each command, the operations carried out and the number of records in history and historyArch before and after it are detailed.

![069](../images/069.png)

It is possible to display the log for a particular command by indicating its number preceded by the characters - and space in the search box.

# Translation

The interface and the messages sent in the logs are translated into the 5 languages ​​supported by Jeedom (thanks to @mips for the ga-translation development). If translation errors are found, you can open a support request and if possible attach the corrected translation file (located in the core/i18n directory of the plugin).

The plugin documentation is translated into English only (other languages ​​refer to the English translation). The translation is done via an automatic translator. However, the screenshots are not translated. 

# Avis

![archiplus_opinion](../images/archiplus_avis.png)


If you like this plugin, please leave a rating and a comment on the Jeedom market, it's always nice: <https://jeedom.com/market/index.php?v=d&p=market_display&id=xxxx#>