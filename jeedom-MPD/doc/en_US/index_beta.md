# MPD Plugin

Plugin for controlling an MPD player.

Music Player Daemon, or MPD, is a free audio player that allows remote access from another computer. It runs in the background on many multimedia servers such as MoodeAudio, Volumio, and others.

MPD allows you to play audio files (= Songs) from its queue (= Queue). The queue is populated by playlists (playlists are not managed by the plugin).

The plugin allows you to perform basic functions (loading playlists, playback, volume, etc.) from Jeedom. The plugin uses the mpc utility to execute commands on the MPD server, whether it is local or remote. The mpc package is installed when the plugin is activated (GitHub link <HTTPS://github.com/MusicPlayerDaemon/mpc>).

# Installing and Configuring the MPD Server

For the plugin to work properly, the MPD server must be running.

This is usually installed transparently by the corresponding multimedia server (Volumio, MoodeAudio, etc.).

By default, the MPD server listens for commands on port 6600. Access to it can be password-protected.

# Plugin Configuration

Once the plugin is installed, you need to activate it.

You can enable the Debug log level to monitor the plugin's activity and identify any potential issues.

# Device Setup

You can configure the devices from the plugin menu (Plugins menu, Multimedia, then MPD).

Click Add to configure a new MPD controller.

![MPD_Equipment](../images/MPD_Equipement.png)

Specify the MPD configuration:

-   **Name**: MPD name
-   **Parent object**: Specifies the parent object to which the device belongs
-   **Category**: Specifies the Jeedom category of the device; the default is Multimedia
-   **Enable**: turns the device active
-   **Visible**: Makes it visible on the dashboard
-   **IP Address**: IP address of the MPD server; leave blank if installed on the Jeedom server
-   **Port**: MPD server listening port; leave blank to use the default value
-   **Password**: password for accessing the MPD server

The following buttons perform the following functions:

-   **Test Connection to the MPD**:  Allows you to test whether the connection settings are correct (be sure to save the configuration before clicking the button).
-   **Generate Commands**:  allows you to generate the commands needed to control the player (useful only if you have deleted one of the commands).

# Commands associated with devices

![MPD_Controls](../images/MPD_Commandes.png)

Basic commands are generated when the device is created.

For each action-type command, the Command field (stored in the Jeedom command's LogicalID) indicates the command sent to the mpc utility. See the mpc documentation for more information ( <HTTPS://www.musicpd.org/doc/mpc/html/> ).

![MPD_Commands_Add](../images/MPD_Commandes_Ajout.png)

The "Create Command" command lets you add an action, such as a shortcut to play a radio station. To do this, you can use the "playsong" command, which will be converted to "play" followed by the song number in the Queue.

# Widget

![MPD_Widget](../images/MPD_Widget.png)

The default layout allows you to perform basic functions. Note the "refresh" button (in the upper-right corner of the widget), which updates the MPD player's status (playlists, current song, etc.). Selecting a playlist populates the MPD queue with the corresponding songs. Selecting a song plays it.

![MPD_Equipment_Layout](../images/MPD_Equipement_Disposition.png)

The layout is created using the Equipment Layout feature (in "Advanced Configuration").

![MPD_Widget_Favorites](../images/MPD_Widget_Favoris.png)

By modifying the layout, you can add shortcuts.

# Controlling the audio player from a Mi Cube

![MPD_micube](../images/MPD_micube.png)

By using scenarios, you can control your audio player without using the Jeedom interface, using a command device such as Xiaomi's Mi Cube, for example.

![MPD_micube_song](../images/MPD_micube_song.png)

The scenario above, triggered by a state change in #[None][Cube][side]#, allows you to change the radio station by switching the side of the Mi Cube.

![MPD_micube_toggle](../images/MPD_micube_toggle.png)

The scenario above, triggered by a state change in #[None][Cube][Button]#, allows you to pause and resume the song by shaking the Mi Cube.

# Translation

The interface, log messages, and documentation have been translated into the 5 languages supported by Jeedom (thanks to @mips for developing ga-translation and docs-translations). If you find any translation errors, please open a support ticket and, if possible, attach the corrected translation file (located in the plugin’s core/i18n directory).

# Reviews

![MPD_review](../images/MPD_avis.png)

If you like this plugin, please leave a rating and a comment on the Jeedom Market—it’s always appreciated: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4464#>

