# MPD Plugin

Plugin allowing control of an MPD player.

Music Player Daemon, or MPD, is a free audio player that allows remote access from another computer. It runs in the background on many multimedia servers such as moodeaudio, volumio, ...

MPD makes it possible to stream audio files (= Songs) that are located in its queue (= Queue). This queue is populated by playlists (playlists are not managed by the plugin).

The plugin allows execution of basic functions (loading playlists, play, volume, ...) from Jeedom. The plugin uses the mpc utility to execute commands on the MPD server, whether local or remote. The mpc package is installed when the plugin is activated (github link <https://github.com/MusicPlayerDaemon/mpc>).

# Installation and configuration of the MPD server

The proper functioning of the plugin assumes that the MPD server is operational.

It is most often installed transparently by the corresponding multimedia server (volumio, moodeaudio, ...).

By default, the MPD server listens for commands on port 6600. Access may be controlled by a password.

# Plugin configuration

Once the plugin is installed, it must be activated.

You can enable Debug log level to monitor plugin activity and identify possible issues.

# Equipment configuration

Equipment configuration is accessible from the plugin menu (Plugins, Multimedia then MPD).

Click on Add to define a new MPD controller.

![MPD_Equipement](../images/MPD_Equipement.png)

Specify the MPD configuration:

- **Name**: name of the MPD
- **Parent object**: indicates the parent object to which the equipment belongs  
- **Category**: indicates the Jeedom category of the equipment, Multimedia by default  
- **Enable**: makes the equipment active  
- **Visible**: makes it visible on the dashboard  
- **IP Address**: IP of the MPD server, leave blank if installed on the Jeedom server  
- **Port**: listening port of the MPD server, leave blank if default value  
- **Password**: password for accessing the MPD server  

The following buttons provide these functions:

- **Test connection to MPD**: allows you to test whether connection parameters are correct (remember to save the configuration before clicking the button).
- **Generate commands**: allows generation of commands to control the player (useful only if one of the commands has been deleted).

# Commands associated with equipment

![MPD_Commandes](../images/MPD_Commandes.png)

Basic commands are generated when the equipment is created.

For each action-type command, the Command field (stored in the Jeedom command LogicalID) indicates the command sent to the mpc utility. Refer to the mpc documentation for more information (<https://www.musicpd.org/doc/mpc/html/>).

![MPD_Commandes_Ajout](../images/MPD_Commandes_Ajout.png)

The 'Create a command' option allows adding an action, for example to create a shortcut to play a radio station. For this, you can use the 'playsong' command which will be transformed into 'play' followed by the song number in the Queue.

# Widget

![MPD_Widget](../images/MPD_Widget.png)

The default layout allows execution of basic functions. Note the 'refresh' button (top right of the widget) which updates the state of the MPD player (playlists, current song, ...). Selecting a playlist initializes the MPD Queue with the corresponding Songs. Selecting a Song plays it.

![MPD_Equipement_Disposition](../images/MPD_Equipement_Disposition.png)

The layout is created using the Equipment Layout function (in 'Advanced configuration').

![MPD_Widget_Favoris](../images/MPD_Widget_Favoris.png)

By modifying the layout, you can add shortcuts.

# Controlling the audio player from a Mi Cube

![MPD_micube](../images/MPD_micube.png)

Using scenarios, it is possible to control your audio player without using the Jeedom interface, for example from a control device such as the Xiaomi Mi Cube.

![MPD_micube_song](../images/MPD_micube_song.png)

The scenario above, triggered by a change of state of #[None][Cube][side]#, allows changing the radio station by changing the side of the Mi Cube.

![MPD_micube_toggle](../images/MPD_micube_toggle.png)

The scenario above, triggered by a change of state of #[None][Cube][Button]#, allows stopping and restarting the song by shaking the Mi Cube.

# Review

![MPD_avis](../images/MPD_avis.png)

If you appreciate this plugin, please leave a review and a comment on the Jeedom market, it’s always nice: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4464#>
