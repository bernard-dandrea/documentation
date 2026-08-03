
<!--  
Zuletzt geändert: 25.07.2026, 18:39:50
-->

# SNMP3-Plugin

Plugin zum Lesen und Schreiben der OIDs von Geräten, die das SNMP-Protokoll unterstützen.

SNMP ist eines der weit verbreiteten Protokolle zur Verwaltung und Analyse von Netzwerkkomponenten. Die meisten Netzwerkkomponenten der professionellen Klasse verfügen über einen integrierten SNMP-Agenten.

Das Plugin nutzt das Paket php-snmp (siehe <https://www.php.net/manual/fr/book.snmp.php>), bei dem es sich um einen Wrapper der Net-SNMP-Bibliothek handelt (siehe <http://www.net-snmp.org>). Das Plugin ermöglicht es, die OIDs, die dies unterstützen, abzufragen (Befehl „get“) und zu aktualisieren (Befehl „set“).

# HINWEIS

Dieses Plugin richtet sich an Personen, die mit dem Protokoll vertraut sind.

Das Thema ist nicht besonders kompliziert, erfordert aber dennoch ein Verständnis der zugrunde liegenden Konzepte (Authentifizierung, OID, MIB, ...).

Bevor Sie sich bei eventuellen Problemen an den Entwickler wenden, überprüfen Sie bitte zunächst, ob die Einstellungen für die Kommunikation mit dem SNMP-Gerät korrekt sind.

Dazu kann man beispielsweise in einer SSH-Sitzung den Befehl „snmpget“ verwenden:

snmpget -v 3 -n "" -u admin_snmp_2024 -a MD5 -A "xxxxxx" -x DES -X "yyyyy" -l authPriv 192.168.1.5 .1.3.6.1.2.1.1.6.0

![SNMP3_snmp_get](../images/SNMP3_snmp_get.png)

# Installation und Konfiguration von SNMP-Geräten

Damit das Plugin ordnungsgemäß funktioniert, muss das SNMP-Protokoll auf dem Zielsystem korrekt installiert und konfiguriert sein. Informationen zur Konfiguration finden Sie in der Dokumentation des Herstellers.

Zur Sicherung der Verbindung wird das Protokoll v3 empfohlen.

![SNMP3_Synology](../images/SNMP3_Synology.png)

Oben finden Sie ein Beispiel für eine Konfiguration auf einem Synology-NAS.

Testen Sie die Verbindungseinstellungen mit dem Befehl „snmpget“ (siehe vorheriger Absatz) oder anderen Dienstprogrammen.

# Einrichtung des Plugins

Sobald das Plugin installiert ist, muss es aktiviert werden. Das Paket „php-snmp“ wird bei der Installation der Abhängigkeiten mitinstalliert.

Sie können die Debug-Protokollstufe aktivieren, um die Aktivität des Plugins zu verfolgen und eventuelle Probleme zu identifizieren.


![SNMP3_Geräte](../images/SNMP3_cron.png)

Sie können außerdem festlegen, ob ein eigenständiger Cron-Job verwendet wird. Dadurch werden andere Cron-Jobs nicht blockiert, falls der Cron-Job des Plugins abstürzt, und das Plugin selbst wird nicht durch andere Cron-Jobs blockiert, die für andere Plugins gestartet werden.

# Verwaltung von MIBs

OIDs können entweder über ihren numerischen Code, z. B. .1.3.6.1.4.1.6574.1.1.0, oder über die entsprechende MIB, z. B. SYNOLOGY-SYSTEM-MIB::systemStatus.0, bezeichnet werden.

Bei der Installation des Pakets „php-snmp“ werden eine Reihe von MIBs installiert (normalerweise im Verzeichnis /usr/share/snmp/mibs), die direkt verwendet werden können.

Mit dem Plugin lassen sich spezifische MIBs installieren, indem die entsprechenden Dateien, zum Beispiel SYNOLOGY-SYSTEM-MIB.txt, im Verzeichnis plugins/SNMP3/data/mibs abgelegt werden.

Sie können die Dateien auch in das gemeinsame Verzeichnis (in der Regel /usr/share/snmp/mibs) kopieren. Beachten Sie, dass dieser Vorgang bei einer Wiederherstellung von Jeedom erneut durchgeführt werden muss.

Sollten Sie Schwierigkeiten bei der Implementierung der MIBs haben, können Sie diese mit dem Befehl snmptranslate testen (siehe <https://net-snmp.sourceforge.io/tutorial/tutorial-5/commands/snmptranslate.html>). Achtung: In diesem Fall werden die MIBs im Verzeichnis plugins/SNMP3/data/mibs nicht berücksichtigt.

# Konfiguration der Geräte

Die Konfiguration der Geräte erfolgt über das Plugin-Menü (Menü „Plugins“, „Verbundene Objekte“ und dann „SNMP3“).

Klicken Sie auf „Hinzufügen“, um das SNMP-Gerät zu konfigurieren.

![SNMP3_Geräte](../images/SNMP3_Equipement.png)

Geben Sie die Konfiguration des SNMP-Geräts an:

-   **Name**: Name des SNMP-Geräts
-   **Übergeordnetes Objekt**: Gibt das übergeordnete Objekt an, zu dem das Gerät gehört
-   **Kategorie**: Gibt die Jeedom-Kategorie des Geräts an
-   **Aktivieren**: Damit wird das Gerät aktiviert.
-   **Version**: SNMP-Version
-   **localhost**: IP-Adresse des Geräts
-   **Sicherheitseinstellungen**: siehe <https://www.php.net/manual/fr/snmp.setsecurity.php>
-   **Timeout**: Maximale Dauer, während der auf eine Antwort auf die SNMP-Anfrage gewartet wird
-   **Wiederholungsversuche**: Anzahl der Versuche, bei denen der Befehl im Falle eines Fehlers erneut gesendet wird (3, wenn das Feld leer ist)
-   **Symbol**: Ermöglicht die Auswahl eines Symboltyps für das Gerät in der Systemsteuerung

Es ist möglich, ein bestimmtes Symbol individuell anzupassen, indem man das entsprechende Bild (z. B. perso1.png für das Symbol „perso1“) im Verzeichnis „plugin_info“ des Plugins ablegt.

Mit der Schaltfläche **SNMP3-Verbindung testen** können Sie überprüfen, ob die Verbindungseinstellungen korrekt sind (denken Sie daran, das Gerät einzuschalten und die Konfiguration zu speichern, bevor Sie auf die Schaltfläche klicken).

# Gerätebezogene Steuerungen

![SNMP3_Befehle](../images/SNMP3_Commandes.png)

Standardmäßig werden zwei Befehle erstellt:

- Letzte Aktualisierung: Befehl, der angibt, wann die letzten Informationen des SNMP-Geräts aktualisiert wurden
- Refresh: Befehl, mit dem alle OIDs aktualisiert werden, für die die Aktualisierungsfunktion aktiviert ist

Die folgenden Schaltflächen stehen zur Verfügung:

- OID importieren: Ermöglicht die Erstellung eines Info-Befehls für eine OID
- Befehl „refresh“ hinzufügen: Ermöglicht die Erstellung eines Aktionsbefehls, um die Abfrage des OID-Werts zu erzwingen
- Aktion hinzufügen: Ermöglicht das Erstellen eines Befehls, um den Wert der OID zu ändern (sofern dies vom SNMP-Gerät unterstützt wird)

# Analyse der Bestellfelder

Für jeden Befehl, der sich auf eine OID bezieht, gibt es zusätzlich zu den üblichen Jeedom-Feldern:

- die LogicalID:
  - für Befehle vom Typ „info“, die der OID entsprechen
  - Bei Refresh-Befehlen entspricht dies „R_“, gefolgt von der OID
  - Bei Aktionsbefehlen muss der Befehl mit „A_“ beginnen, gefolgt von der OID
- Das Kontrollkästchen „Update“, mit dem festgelegt werden kann, ob ein Update der OID angefordert werden soll oder nicht
- Das Feld „scan“, das die Aktualisierungsfrequenz der OID angibt

Bei Befehlen, die eine Aktualisierung der OID ermöglichen, bestimmt der Subtyp des Befehls „action“ das Format des an das SNMP-Gerät übermittelten Werts. Wenn der Subtyp „Message“ ist, gibt der Titel das Format an und der Inhalt der Nachricht liefert den Wert (es wird nur die erste Zeile übertragen). Unter <https://www.php.net/manual/fr/function.snmpset.php> finden Sie die unterstützten Formate.

# Widget

![SNMP3_Widget](../images/SNMP3_Widget.png)

Hier ist ein Beispiel für ein Widget. Man kann die Namen der Befehle ändern, damit sie aussagekräftiger sind.

# Übersetzung

Die Benutzeroberfläche, die in den Protokollen ausgegebenen Meldungen und die Dokumentation sind in die fünf von Jeedom unterstützten Sprachen übersetzt (vielen Dank an @mips für die Entwicklung von ga-translation und docs-translations). Sollten Sie Übersetzungsfehler feststellen, können Sie eine Supportanfrage stellen und, wenn möglich, die korrigierte Übersetzungsdatei (zu finden im Verzeichnis core/i18n des Plugins) beifügen.

# Bewertungen

![SNMP3_Hinweis](../images/SNMP3_avis.png)

Wenn Ihnen dieses Plugin gefällt, hinterlassen Sie bitte eine Bewertung und einen Kommentar im Jeedom Market – das freut uns immer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4484#>
