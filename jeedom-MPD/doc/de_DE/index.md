# MPD-Plugin

Plugin zur Steuerung eines MPD-Players.

Music Player Daemon, kurz MPD, ist ein freier Audio-Player, der den Fernzugriff von einem anderen Computer aus ermöglicht. Er läuft im Hintergrund vieler Multimedia-Server wie moodeaudio, volumio, ...

Mit MPD können Sie die Audiodateien (= Songs) abspielen, die sich in der Warteschlange (= Queue) befinden. Diese wird durch die Wiedergabelisten gespeist (die Wiedergabelisten werden nicht vom Plugin verwaltet).

Das Plugin ermöglicht die Ausführung der Grundfunktionen (Laden von Wiedergabelisten, Wiedergabe, Lautstärke usw.) über Jeedom. Das Plugin nutzt das Dienstprogramm „mpc“, um Befehle auf dem MPD-Server auszuführen, unabhängig davon, ob dieser lokal oder remote betrieben wird. Das mpc-Paket wird bei der Aktivierung des Plugins installiert (GitHub-Link <https://github.com/MusicPlayerDaemon/mpc>).

# Installation und Konfiguration des MPD-Servers

Damit das Plugin ordnungsgemäß funktioniert, muss der MPD-Server betriebsbereit sein.

Dieser wird in der Regel nahtlos vom entsprechenden Multimedia-Server (Volumio, Moodeaudio, …) installiert.

Standardmäßig wartet der MPD-Server auf Befehle am Port 6600. Der Zugriff darauf kann durch ein Passwort geschützt werden.

# Einrichtung des Plugins

Sobald das Plugin installiert ist, muss es aktiviert werden.

Man kann die Debug-Protokollstufe aktivieren, um die Aktivität des Plugins zu verfolgen und eventuelle Probleme zu identifizieren.

# Konfiguration der Geräte

Die Konfiguration der Geräte erfolgt über das Plugin-Menü (Menü „Plugins“, „Multimedia“ und dann „MPD“).

Klicken Sie auf „Hinzufügen“, um einen neuen MPD-Controller zu definieren.

![MPD_Ausstattung](../images/MPD_Equipement.png)

Geben Sie die Konfiguration des MPD an:

-   **Name**: Name des MPD
-   **Übergeordnetes Objekt**: Gibt das übergeordnete Objekt an, zu dem das Gerät gehört
-   **Kategorie**: Gibt die Jeedom-Kategorie des Geräts an, standardmäßig „Multimedia“
-   **Aktivieren**: Damit wird das Gerät aktiviert.
-   **Sichtbar**: Macht es auf dem Dashboard sichtbar
-   **IP-Adresse**: IP-Adresse des MPD-Servers; bitte leer lassen, wenn die Installation auf dem Jeedom-Server erfolgt ist
-   **Port**: Port, auf dem der MPD-Server auf Verbindungen wartet; bei Verwendung des Standardwerts leer lassen
-   **Passwort**: Passwort für den Zugriff auf den MPD-Server

Die folgenden Tasten ermöglichen folgende Funktionen:

-   **Verbindung zum MPD testen**: Hiermit können Sie überprüfen, ob die Verbindungseinstellungen korrekt sind (denken Sie daran, die Konfiguration zu speichern, bevor Sie auf die Schaltfläche klicken).
-   **Befehle generieren**: Ermöglicht die Generierung von Befehlen zur Steuerung des Players (nur nützlich, wenn einer der Befehle gelöscht wurde).

# Gerätebezogene Steuerungen

![MPD_Befehle](../images/MPD_Commandes.png)

Die grundlegenden Befehle werden bei der Erstellung des Geräts generiert.

Bei jedem Befehl vom Typ „Aktion“ gibt das Feld „Befehl“ (gespeichert in der LogicalID des Jeedom-Befehls) den an das Dienstprogramm mpc übermittelten Befehl an. Weitere Informationen finden Sie in der Dokumentation zu mpc ( <https://www.musicpd.org/doc/mpc/html/> ).

![MPD_Befehle_Hinzufügen](../images/MPD_Commandes_Ajout.png)

Mit dem Befehl „Befehl erstellen“ kann eine Aktion hinzugefügt werden, beispielsweise um eine Verknüpfung zum Abspielen eines Radiosenders zu erstellen. Dazu kann der Befehl „playsong“ verwendet werden, der in „play“ gefolgt von der Nummer des Titels in der Warteschlange umgewandelt wird.

# Widget

![MPD_Widget](../images/MPD_Widget.png)

Mit der standardmäßig erstellten Ansicht lassen sich die Grundfunktionen ausführen. Beachten Sie die Schaltfläche „Refresh“ (oben rechts im Widget), mit der Sie den Status des MPD-Players (Playlists, aktuell abgespielter Titel usw.) aktualisieren können. Durch Auswahl einer Playlist wird die MPD-Warteschlange mit den entsprechenden Titeln gefüllt. Durch Auswahl eines Titels wird dieser abgespielt.

![MPD_Ausstattung_Anordnung](../images/MPD_Equipement_Disposition.png)

Die Darstellung erfolgt mithilfe der Funktion „Anordnung der Geräte“ (unter „Erweiterte Konfiguration“).

![MPD_Widget_Favoriten](../images/MPD_Widget_Favoris.png)

Durch Ändern der Darstellung können Verknüpfungen hinzugefügt werden.

# Steuerung des Audio-Players über einen Mi Cube

![MPD_micube](../images/MPD_micube.png)

Mithilfe von Szenarien lässt sich der Audio-Player ohne Nutzung der Jeedom-Benutzeroberfläche über ein Steuergerät wie beispielsweise den Mi Cube von Xiaomi bedienen.

![MPD_micube_song](../images/MPD_micube_song.png)

Das oben gezeigte Szenario, das bei einer Statusänderung von #[Keine][Cube][Seite]# ausgelöst wird, ermöglicht es, den Radiosender zu wechseln, indem die Seite des Mi Cube gewechselt wird.

![MPD_micube_toggle](../images/MPD_micube_toggle.png)

Das oben gezeigte Szenario, das bei einer Zustandsänderung von #[Keine][Cube][Button]# ausgelöst wird, ermöglicht es, den Song durch Schütteln des Mi Cube anzuhalten und neu zu starten.

# MPD-Plugin

Plugin zur Steuerung eines MPD-Players.

Music Player Daemon, kurz MPD, ist ein freier Audio-Player, der den Fernzugriff von einem anderen Computer aus ermöglicht. Er läuft im Hintergrund vieler Multimedia-Server wie moodeaudio, volumio, ...

Mit MPD können Sie die Audiodateien (= Songs) abspielen, die sich in der Warteschlange (= Queue) befinden. Diese wird durch die Wiedergabelisten gespeist (die Wiedergabelisten werden nicht vom Plugin verwaltet).

Das Plugin ermöglicht die Ausführung der Grundfunktionen (Laden von Wiedergabelisten, Wiedergabe, Lautstärke usw.) über Jeedom. Das Plugin nutzt das Dienstprogramm „mpc“, um Befehle auf dem MPD-Server auszuführen, unabhängig davon, ob dieser lokal oder remote betrieben wird. Das mpc-Paket wird bei der Aktivierung des Plugins installiert (GitHub-Link <https://github.com/MusicPlayerDaemon/mpc>).

# Installation und Konfiguration des MPD-Servers

Damit das Plugin ordnungsgemäß funktioniert, muss der MPD-Server betriebsbereit sein.

Dieser wird in der Regel nahtlos vom entsprechenden Multimedia-Server (Volumio, Moodeaudio, …) installiert.

Standardmäßig wartet der MPD-Server auf Befehle am Port 6600. Der Zugriff darauf kann durch ein Passwort geschützt werden.

# Einrichtung des Plugins

Sobald das Plugin installiert ist, muss es aktiviert werden.

Man kann die Debug-Protokollstufe aktivieren, um die Aktivität des Plugins zu verfolgen und eventuelle Probleme zu identifizieren.

# Konfiguration der Geräte

Die Konfiguration der Geräte erfolgt über das Plugin-Menü (Menü „Plugins“, „Multimedia“ und dann „MPD“).

Klicken Sie auf „Hinzufügen“, um einen neuen MPD-Controller zu definieren.

![MPD_Ausstattung](../images/MPD_Equipement.png)

Geben Sie die Konfiguration des MPD an:

-   **Name**: Name des MPD
-   **Übergeordnetes Objekt**: Gibt das übergeordnete Objekt an, zu dem das Gerät gehört
-   **Kategorie**: Gibt die Jeedom-Kategorie des Geräts an, standardmäßig „Multimedia“
-   **Aktivieren**: Damit wird das Gerät aktiviert.
-   **Sichtbar**: Macht es auf dem Dashboard sichtbar
-   **IP-Adresse**: IP-Adresse des MPD-Servers; bitte leer lassen, wenn die Installation auf dem Jeedom-Server erfolgt ist
-   **Port**: Port, auf dem der MPD-Server auf Verbindungen wartet; bei Verwendung des Standardwerts leer lassen
-   **Passwort**: Passwort für den Zugriff auf den MPD-Server

Die folgenden Tasten ermöglichen folgende Funktionen:

-   **Verbindung zum MPD testen**: Hiermit können Sie überprüfen, ob die Verbindungseinstellungen korrekt sind (denken Sie daran, die Konfiguration zu speichern, bevor Sie auf die Schaltfläche klicken).
-   **Befehle generieren**: Ermöglicht die Generierung von Befehlen zur Steuerung des Players (nur nützlich, wenn einer der Befehle gelöscht wurde).

# Gerätebezogene Steuerungen

![MPD_Befehle](../images/MPD_Commandes.png)

Die grundlegenden Befehle werden bei der Erstellung des Geräts generiert.

Bei jedem Befehl vom Typ „Aktion“ gibt das Feld „Befehl“ (gespeichert in der LogicalID des Jeedom-Befehls) den an das Dienstprogramm mpc übermittelten Befehl an. Weitere Informationen finden Sie in der Dokumentation zu mpc ( <https://www.musicpd.org/doc/mpc/html/> ).

![MPD_Befehle_Hinzufügen](../images/MPD_Commandes_Ajout.png)

Mit dem Befehl „Befehl erstellen“ kann eine Aktion hinzugefügt werden, beispielsweise um eine Verknüpfung zum Abspielen eines Radiosenders zu erstellen. Dazu kann der Befehl „playsong“ verwendet werden, der in „play“ gefolgt von der Nummer des Titels in der Warteschlange umgewandelt wird.

# Widget

![MPD_Widget](../images/MPD_Widget.png)

Mit der standardmäßig erstellten Ansicht lassen sich die Grundfunktionen ausführen. Beachten Sie die Schaltfläche „Refresh“ (oben rechts im Widget), mit der Sie den Status des MPD-Players (Playlists, aktuell abgespielter Titel usw.) aktualisieren können. Durch Auswahl einer Playlist wird die MPD-Warteschlange mit den entsprechenden Titeln gefüllt. Durch Auswahl eines Titels wird dieser abgespielt.

![MPD_Ausstattung_Anordnung](../images/MPD_Equipement_Disposition.png)

Die Darstellung erfolgt mithilfe der Funktion „Anordnung der Geräte“ (unter „Erweiterte Konfiguration“).

![MPD_Widget_Favoriten](../images/MPD_Widget_Favoris.png)

Durch Ändern der Darstellung können Verknüpfungen hinzugefügt werden.

# Steuerung des Audio-Players über einen Mi Cube

![MPD_micube](../images/MPD_micube.png)

Mithilfe von Szenarien lässt sich der Audio-Player ohne Nutzung der Jeedom-Benutzeroberfläche über ein Steuergerät wie beispielsweise den Mi Cube von Xiaomi bedienen.

![MPD_micube_song](../images/MPD_micube_song.png)

Das oben gezeigte Szenario, das bei einer Statusänderung von #[Keine][Cube][Seite]# ausgelöst wird, ermöglicht es, den Radiosender zu wechseln, indem die Seite des Mi Cube gewechselt wird.

![MPD_micube_toggle](../images/MPD_micube_toggle.png)

Das oben gezeigte Szenario, das bei einer Zustandsänderung von #[Keine][Cube][Button]# ausgelöst wird, ermöglicht es, den Song durch Schütteln des Mi Cube anzuhalten und neu zu starten.

# Übersetzung

Die Benutzeroberfläche, die in den Protokollen ausgegebenen Meldungen und die Dokumentation sind in die fünf von Jeedom unterstützten Sprachen übersetzt (vielen Dank an @mips für die Entwicklung von ga-translation und docs-translations). Sollten Ihnen Übersetzungsfehler auffallen, können Sie eine Supportanfrage stellen und, wenn möglich, die korrigierte Übersetzungsdatei (zu finden im Verzeichnis core/i18n des Plugins) beifügen.

# Bewertungen

![MPD_Bewertung](../images/MPD_avis.png)

Wenn Ihnen dieses Plugin gefällt, hinterlassen Sie bitte eine Bewertung und einen Kommentar im Jeedom Market – das freut uns immer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4464#>


# Bewertungen

![MPD_Bewertung](../images/MPD_avis.png)

Wenn Ihnen dieses Plugin gefällt, hinterlassen Sie bitte eine Bewertung und einen Kommentar im Jeedom Market – das freut uns immer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4464#>
