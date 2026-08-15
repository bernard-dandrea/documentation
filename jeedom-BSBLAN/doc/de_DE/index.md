
<!--  
Zuletzt geändert: 28.07.2026, 16:00:31
-->

# BSBLAN-Plugin

Plugin zur Anbindung an den Controller BSB-LPB-LAN.

Der Controller BSB-LPB-LAN ist das Ergebnis eines Projekts, dessen Ziel die Kommunikation mit SIEMENS-Karten ist, die zahlreiche Heizkessel, Wärmepumpen und andere industrielle Anlagen steuern.

Die Dokumentation ist sehr umfassend und findet sich unter dieser Adresse <https://docs.bsb-lan.de>. Die Hardware kann bei Frederik Holst <bsb@code-it.de> erworben werden.

Das BSB-LAN kann die von Siemens gelieferten OZW-Steuerungen vorteilhaft ersetzen. Die Lösung ist deutlich kostengünstiger, ermöglicht den Zugriff auf alle Parameter der Siemens-Karten (im Gegensatz zum OZW) und die Zugriffszeiten auf die Karten sind wesentlich kürzer. Zudem ist es möglich, die Temperatur der beheizten Zonen zu übermitteln, ohne dass ein Raumfühler erforderlich ist.

Die Kommunikation zwischen dem Plugin und dem BSBLAN erfolgt über Web-APIs.

# Installation und Konfiguration des BSBLAN-Controllers

Für den ordnungsgemäßen Betrieb des Plugins muss das BSB-LAN-Modul betriebsbereit sein.

Informationen zur Installation und Konfiguration finden Sie in der hervorragenden Dokumentation auf der Projekt-Website.

Wenn Sie Einstellungen ändern möchten, müssen Sie dies in der Konfiguration des BSBLAN zulassen.

Das Plugin wurde mit den Versionen 3.2 und 4.2 getestet. Grundsätzlich sollte das Plugin auch mit früheren Versionen funktionieren, da die API-Aufrufe recht einfach sind und bereits seit vielen Versionen vorhanden sein dürften.

# Einrichtung des Plugins

Sobald das Plugin installiert ist, muss es aktiviert werden.

![Konfiguration](../images/BSBLAN_configuration.png)

Sie können außerdem festlegen, ob ein eigenständiger Cron-Job verwendet wird. Dadurch werden andere Cron-Jobs nicht blockiert, falls der Cron-Job des Plugins abstürzt, und das Plugin selbst wird nicht durch andere Cron-Jobs blockiert, die für andere Plugins gestartet werden.

Sie können die Debug-Protokollstufe aktivieren, um die Aktivität des Plugins zu verfolgen und eventuelle Probleme zu identifizieren.

# Konfiguration der Geräte

Die Konfiguration der Geräte erfolgt über das Plugin-Menü (Menü „Plugins“, „Verbundene Objekte“ und dann „BSBLAN“).

Klicken Sie auf „Hinzufügen“, um den BSBLAN-Controller zu konfigurieren.

![BSBLAN_Ausstattung](../images/BSBLAN_Equipement.png)

Geben Sie die Konfiguration des BSBLAN an:

-   **Name**: Name des BSBLAN
-   **Übergeordnetes Objekt**: Gibt das übergeordnete Objekt an, zu dem das Gerät gehört
-   **Kategorie**: Gibt die Jeedom-Kategorie des Geräts an
-   **Aktivieren**: Damit wird das Gerät aktiviert.
-   **Sichtbar**: Macht es auf dem Dashboard sichtbar
-   **IP-Adresse**: IP-Adresse des Geräts
-   **Benutzername und Passwort**: Zugangsdaten für den Webserver
-   **Passkey**: Präfix, das in HTML-Anfragen angegeben werden muss (siehe BSBLAN-Dokumentation)
-   **Timeout**: Maximale Wartezeit für eine Antwort auf die HTTP-Anfrage (15 Sekunden, wenn das Feld leer ist)
-   **Updates**: Methode zur Durchführung von Updates, entweder über JSON oder einen direkten Befehl in der URL. In einigen Fällen wurde festgestellt, dass Updates über JSON nicht durchgeführt wurden. Der Grund dafür konnte nicht ermittelt werden. In diesem Fall kann man die Option mit dem Befehl /S verwenden, die in jedem Fall funktioniert. In Version 3 von BSBLAN werden jedoch bestimmte Befehle, bei denen das INFO-Flag angegeben werden muss (z. B. das Senden der Raumtemperatur), nicht berücksichtigt.
-   **Anzahl der Versuche**: Anzahl der Versuche, bei denen der Befehl im Falle eines Fehlers erneut gesendet wird (3, wenn das Feld leer ist)
-   **Symbol**: Ermöglicht die Auswahl eines Symboltyps für das Gerät in der Systemsteuerung

Es ist möglich, dem BSBLAN ein bestimmtes Symbol zuzuweisen. Man kann auch ein benutzerdefiniertes Symbol anpassen, indem man das entsprechende Bild (z. B. perso1.png für das Symbol „perso1“) im Verzeichnis „plugin_info“ des Plugins ablegt.

Die folgenden Tasten ermöglichen folgende Funktionen:

-   **Zugriff auf BSBLAN**: Ermöglicht die Anmeldung über das Web bei BSBLAN
-   **Verbindung zum BSBLAN testen**: Hiermit können Sie überprüfen, ob die Verbindungseinstellungen korrekt sind (denken Sie daran, die Konfiguration zu speichern, bevor Sie auf die Schaltfläche klicken). Die Versionsnummer des BSBLAN wird angezeigt.

# Gerätebezogene Steuerungen

![BSBLAN_Befehle](../images/BSBLAN_Commandes.png)

Standardmäßig werden zwei Befehle erstellt:

- Letzte Aktualisierung: Befehl, der angibt, wann die letzten Informationen des BSBLAN aktualisiert wurden
- Refresh: Befehl, mit dem alle Parameter aktualisiert werden, für die die Aktualisierungsfunktion aktiviert ist

Die folgenden Schaltflächen stehen zur Verfügung:

- Parameter importieren: Ermöglicht die Erstellung eines Befehls für einen bestimmten Parameter
- Befehl „refresh“ hinzufügen: Ermöglicht das erzwungene Abrufen des Parameterwerts
- Aktionsbefehl hinzufügen: Ermöglicht die Änderung des Parameterwerts (sofern dies im Webserver zulässig ist)

# Analyse der Bestellfelder

Für jeden Befehl, der sich auf einen Parameter bezieht, gibt es zusätzlich zu den üblichen Jeedom-Feldern:

- die LogicalID:
  - bei Befehlen vom Typ „info“, entspricht dem Parametercode
  - für Aktionsbefehle, beginnend mit „A_“, gefolgt vom Parametercode
  - Bei Refresh-Befehlen entspricht dies „R_“, gefolgt vom Parametercode
- Das Kontrollkästchen „Update“, mit dem festgelegt werden kann, ob eine Aktualisierung des Parameters angefordert werden soll oder nicht
- Bei Info-Befehlen gibt das Feld „scan“ die Aktualisierungsfrequenz des Parameters an
- Bei Aktionsbefehlen dient das Feld „MAJ“ dazu, einen bestimmten Aktualisierungsmodus festzulegen

# Widget

![BSBLAN_Widget](../images/BSBLAN_Widget.png)

Hier ist ein Beispiel für ein Widget. Man kann die Namen der Befehle ändern, damit sie aussagekräftiger sind.

# Bewertungen

![BSBLAN_Bewertung](../images/BSBLAN_avis.png)

Wenn Ihnen dieses Plugin gefällt, hinterlassen Sie bitte eine Bewertung und einen Kommentar im Jeedom Market – das freut uns immer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4424#>
