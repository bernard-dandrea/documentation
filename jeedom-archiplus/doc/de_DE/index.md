<!--  
Zuletzt geändert: 26.06.2026
-->
- [Verwaltung von Verlaufsdaten in Jeedom](#Verwaltung-von-Verlaufsdaten-in-Jeedom)
  - [Funktionsweise](#Funktionsweise)
  - [Umfang der Verlaufsdaten](#Umfang-der-Verlaufsdaten)
  - [Die Grenzen der Archivierung in Jeedom](#die-grenzen-der-archivierung-in-jeedom)
  - [Die Vorteile des archiplus-Plugins](#die-vorteile-des-archiplus-plugins)
  - [Hinweis](#hinweis)
- [Archiplus-Plugin](#plugin-archiplus)
  - [Das archiplus-Plugin installieren](#das-archiplus-plugin-installieren)
  - [Plugin konfigurieren](#plugin-konfigurieren)
  - [Die Module des Plugins](#die-module-des-plugins)
- [Zugriff auf die Module](#zugriff-auf-die-module)
  - [Die Bedientasten](#die-bedientasten)
  - [Die Spalte zur Zeilenauswahl](#die-spalte-zur-zeilenauswahl)
  - [Spaltenüberschriften](#Spaltenüberschriften)
  - [Die Leitungen](#die-leitungen)
  - [Die Summen am Tabellenende](#die-summen-am-tabellenende)
- [Das Monitor-Modul](#das-monitor-modul)
  - [Statistiken](#statistiken)
  - [Visualisierung](#visualisierung)
  - [Änderungen](#Änderungen)
  - [Änderungen anhand einer Excel-Datei](#Änderungen-anhand-einer-Excel-Datei)
  - [Bearbeitbare Daten](#bearbeitbare-daten)
    - [KLV (Keep Last Value)](#klv-keep-last-value)
    - [Uniq](#uniq)
    - [Frist](#frist)
    - [Rahmung](#cadrage)
    - [Pond](#pond)
    - [Paket](#pack)
    - [Abgerundet](#abgerundet)
  - [Über das Kontextmenü zugängliche Funktionen](#über-das-kontextmenü-zugängliche-funktionen)
- [Historische Daten](#historische-daten)
  - [Zugang](#zugang)
  - [Änderung](#Änderung)
  - [Löschen](#löschen)
  - [Export](#export)
- [Das Import-Modul](#das-import-modul)
- [Das Restore-Modul](#das-restore-modul)
- [FAQ](#faq)
  - [Letzten Wert beibehalten](#keep-last-value)
  - [Uniq](#uniq-1)
  - [Zeitrahmen und Rahmenbedingungen](#zeitrahmen-und-rahmenbedingungen)
  - [Glättung und Gewichtung](#glättung-und-gewichtung)
  - [Paket](#pack-1)
  - [Abgerundet](#abgerundet-1)
  - [Daten aus „historyArch“ in „history“ kopieren](#daten-aus-historyarch-in-history-kopieren)
  - [Archiplus in PHP verwenden](#archiplus-in-php-verwenden)
- [Die Protokolle](#die-protokolle)
- [Übersetzung](#übersetzung)
- [Bewertung](#bewertung)



Die Hauptfunktion des Plugins besteht darin, eine umfassende Palette an Tools bereitzustellen, die Folgendes ermöglichen:

*   **die Archivierungseinstellungen für Befehle vom Typ INFO zu verwalten**
*   **Datenmengen zu visualisieren und Anomalien zu erkennen**
*   **einfaches Einfügen von historischen Daten aus Excel-Dateien**
*   **Abrufen der Verlaufsdaten aus dem Jeedom-Archiv**
*   **Erweiterung der Standard-Archivierungsoptionen von Jeedom**

Durch die optionale Aktivierung der im Plugin integrierten Archivierungsfunktion lassen sich die von Jeedom angebotenen Archivierungsfunktionen erheblich erweitern.

# Verwaltung von Verlaufsdaten in Jeedom

## Funktionsweise

Der Verlauf in Jeedom hat sich seit den ersten Versionen kaum verändert und basiert auf zwei Tabellen:

* Die Tabelle „history“, in die die aktualisierten Werte der Befehle vom Typ INFO, für die der Verlauf aktiviert ist, geschrieben werden
* Die Tabelle „historyArch“, die bei jeder Archivierung (in der Regel täglich um 5:00 Uhr) die konsolidierten oder nicht konsolidierten History-Werte entsprechend den für den Befehl festgelegten Einstellungen erhält.

Der Aufbau der beiden Tabellen ist identisch und sehr einfach: Pro Bestellung wird ein Wert mit der ID und dem Datum sowie der Uhrzeit (auf die Sekunde genau) gespeichert.

Der Verlauf kann in der Jeedom-Benutzeroberfläche als Diagramm angezeigt werden.

Die offizielle Dokumentation zur Verwaltung von Verlaufsdaten in Jeedom finden Sie [hier](https://doc.jeedom.com/fr_FR/core/4.5/history).

## Umfang der historischen Daten

Jeedom-Nutzer werden sich erst dann für den Verlauf interessieren, wenn sie feststellen, dass die Datenbank übermäßig anwächst, die Ladezeiten des Verlaufs sehr lang werden und die Größe der Sicherungsdateien ständig zunimmt.

Über den folgenden Link gelangen Sie zu einer Anleitung, in der erklärt wird, wie Sie ein Szenario erstellen, das die Volumina der umfangreichsten Tabellen sowie die INFO-Befehle mit den umfangreichsten Verlaufsdaten auflistet [Tutorial – Archive analysieren](https://community.jeedom.com/t/tuto-analyser-les-archives-pour-detecter-des-pbs-lenteurs-espaces-disques/104384).

Einfacher ausgedrückt: Sie können die Tabellengrößen einsehen, indem Sie die Datenbank direkt abfragen (Menü „Einstellungen“ / „System“ / „Konfiguration“, dann die Registerkarte „OS / DB“ (die letzte), dann die Schaltfläche „Datenbankverwaltung“ (die unterste rote Schaltfläche) und anschließend links die Abfrage „Größe“).

In einer Standardinstallation sollte man sich Gedanken machen, wenn das Gesamtvolumen des Archivs eine Million Datensätze überschreitet oder ein „info“-Befehl mehr als 10.000 Datensätze liefert. In diesem Fall ist es notwendig, die betroffenen Befehle zu analysieren und die verschiedenen Parameter der Protokollierung und Archivierung anzupassen, um dieses Volumen zu reduzieren. Ist dies nicht möglich, muss möglicherweise auf andere Archivierungsmethoden zurückgegriffen werden, beispielsweise auf InfluxDB, das standardmäßig mit Jeedom verbunden werden kann.

Das Plugin „archiplus“ zeigt sofort die Volumina von „history“ und „historyArch“ an und ermöglicht es, Probleme leicht zu identifizieren und Lösungen dafür zu finden.

## Die Grenzen der Archivierung in Jeedom

Obwohl in vielen Anlagen der Standardbetrieb ausreicht, sind folgende Einschränkungen zu beachten:

* Schwierigkeiten beim Anzeigen und Ändern der Archivierungseinstellungen: Das einzige verfügbare Tool (Menü „Analyse“ / „Verlauf“ und dann „Konfiguration“) ist sehr langsam, unpraktisch und bietet nur wenige konfigurierbare Felder.
* Schwierigkeiten bei der Darstellung der historischen Volumina pro Auftrag und beim Erkennen ungewöhnlicher Volumina: Dazu sind SQL-Abfragen und umständliche Prozesse erforderlich
* Einstellungen für die Datengruppierung in historyArch, global definiert und nicht pro Befehl anpassbar
* Keine Transparenz hinsichtlich des Archivierungsprozesses (kein Protokoll)
* Globale Archivierung: Es ist nicht möglich, die Archivierung für einen bestimmten Auftrag zu starten
* Ungefähre Mittelwertglättung
* Grundlegende Tools zum Exportieren/Importieren von Daten (Plugin „dataexport“). Es gibt keine Möglichkeit, die in den Sicherungen enthaltenen historischen Daten wiederherzustellen.

## Die Vorteile des archiplus-Plugins

Das Plugin „archiplus“ ermöglicht die Darstellung von Befehlen vom Typ INFO in einer Tabelle zusammen mit allen Parametern zur Archivierung. Die Anzahl der Einträge in „history“ und „historyArch“ wird ebenfalls angezeigt, wodurch sich übermäßige Datenmengen sehr leicht erkennen lassen. Das Plugin nutzt die JavaScript-Bibliothek „Tabulator“, die äußerst leistungsstark ist und einen sehr einfachen Zugriff auf die Funktionen des Plugins ermöglicht.

Alle von Jeedom angebotenen Funktionen sind direkt verfügbar, und es wurden weitere Funktionen hinzugefügt:

* Erweiterte Konfiguration der Steuerung
* Anzeige von Diagrammen und Datenextraktion
* Verlauf löschen
* Standard-CSV-Export
* Kopieren der Konfiguration aus dem Verlauf (oder einer einzelnen Einstellung) in mehrere Befehle
* Laden der Parameter der INFO-Befehle zum Verlauf aus einer Excel-Datei
* Archivierung für einen bestimmten Auftrag starten
* Kopieren des Verlaufs einer Bestellung in eine andere Bestellung
* Kopie von „historyArch“ nach „history“, um eine Konsolidierung nach Intervallen zu starten
* Importieren des Bestellverlaufs aus einer Excel-Datei
* Auslesen des Verlaufs in verschiedenen Formaten (xlsx, CSV, JSON, HTML) für eine oder mehrere Bestellungen aus Jeedom oder einem Standard-Jeedom-Backup
* Extraktion der Parameter der INFO-Befehle zum Verlauf aus einem Jeedom-Backup (diese Parameter können anschließend in Jeedom angewendet werden)

Darüber hinaus kann der Archivierungsprozess des Plugins als Alternative zur nativen Archivierungsfunktion von Jeedom aktiviert werden. Dieser ermöglicht:

* die Archivierung für einen bestimmten Auftrag zu starten
* alle durchgeführten Vorgänge und die für jeden Befehl berücksichtigten Parameter im Archiplus-Protokoll zu speichern
* den Berechnungszeitraum (für Min., Max., Durchschnitt), die Frist bis zur Archivierung und die Paketgröße für jeden Befehl individuell anzupassen
* das Datum der Löschung auf einen Tag, eine Stunde oder eine Minute festlegen
* die Archivierung für einen Auftrag aus einem Szenario heraus starten (in PHP-Code)
* Optionen hinzuzufügen, die in Jeedom nicht vorgesehen sind (siehe Erläuterungen weiter unten in der Dokumentation)
  * „Keep Last Value“: Immer mindestens einen Wert im Verlauf beibehalten
  * Uniq: Identische aufeinanderfolgende Werte in historyArch entfernen
  * Gewichtung: Bei der Mittelwertglättung den gewichteten Wert über die Dauer des Intervalls berechnen (und nicht den Mittelwert der Werte)

Das Plugin „archiplus“ wurde unter Debian 12 entwickelt und verwendet kein jQuery (ebenso wenig wie die verwendeten Bibliotheken von Drittanbietern). Es entspricht den Entwicklungsstandards von Jeedom. Der Code der Klasse „archiplus“ ist sehr gut strukturiert und umfassend dokumentiert: Der Autor des Plugins wird alle Vorschläge zur Korrektur oder Verbesserung prüfen.

Da Jeedom keine Pläne für eine Weiterentwicklung der Protokollverwaltung hat, dürfte das Plugin in naher Zukunft keine Überarbeitung erfordern.

## Hinweis

Das Plugin und sein spezifischer Archivierungsprozess wurden sehr gründlich getestet, sind jedoch nicht vor Fehlern gefeit. In diesem Fall ist das Jeedom-Team selbstverständlich nicht verpflichtet, Support zu leisten. Anfragen zur Analyse und Behebung von Fehlern müssen zwingend über das Standard-Support-Formular an den Autor des Plugins gerichtet werden.

Die Aktivierung des Plugins und insbesondere des Archivierungsprozesses setzt daher die uneingeschränkte Zustimmung zu dieser Situation voraus.

# Archiplus-Plugin

## Das Plugin „archiplus“ installieren

Gehen Sie in den Jeedom Market, suchen Sie das Plugin „archiplus“ und installieren Sie die **stabile** Version. Aktivieren Sie anschließend **das Plugin**.

![001](../images/001.png)

Das Plugin ist über das Menü zugänglich.

## Plugin konfigurieren

In den Einstellungen können Sie die üblichen Parameter der Plugins sowie die Standardwerte des Plugins festlegen.

![003](../images/003.png)

Um möglichst viele Informationen über den Archivierungsprozess des Plugins und die durchgeführten Aktionen zu erhalten, empfiehlt es sich, die Protokolle in den Debug-Modus zu versetzen.

Bitte beachten Sie, dass Supportanfragen über die Schaltfläche **Support** gestellt werden müssen.

![002](../images/002.png)

Im Bereich „Konfiguration“ können Sie:

* Spezifische Archivierung aktivieren (standardmäßig deaktiviert)
* Geben Sie an, ob die Einträge in „history“ und „historyArch“ gelöscht werden sollen, falls der betreffende Befehl nicht existiert
* Festlegen, dass History-Einträge nicht in historyArch übertragen werden, wenn keine Glättung erfolgt
* Format für Exporte festlegen
* Standardzeitraum für Lösch- und Archivierungsenddaten festlegen

Durch die Aktivierung der spezifischen Archivierung wird ein neuer Cron-Job in der Aufgaben-Engine erstellt und die Standardarchivierung deaktiviert. Durch die Deaktivierung der spezifischen Archivierung wird der umgekehrte Vorgang ausgeführt.

Wenn Sie den Archivierungsprozess des Plugins testen möchten, können Sie es vorübergehend aktivieren, Archivierungstests für einzelne Befehle durchführen und anschließend die Archivierung des Plugins wieder deaktivieren. Da der Archivierungsprozess von Jeedom normalerweise um 5 Uhr morgens startet, hat dies keine Auswirkungen auf die nicht getesteten Befehle.

## Die Module des Plugins

![004](../images/004.png)

Über das Menü „Plugins / Monitoring / archiplus“ haben Sie Zugriff auf alle Funktionen des Plugins

* Konfiguration des Plugins (siehe oben)
* Zugriff auf die allgemeinen Einstellungen für die Archivierung
* Überwachung: Anzeige und Änderung der Einstellungen sowie Durchführung der wichtigsten Vorgänge im Zusammenhang mit der Archivierung
* Import: Historische Daten aus einer Excel-Datei importieren
* Wiederherstellung: Historische Daten aus einem Jeedom-Standardarchiv extrahieren

Die Anzeige der historischen Daten ist über das Modul „Monitoring und Restore“ zugänglich.

# Zugriff auf die Module

Die Module werden über die Plugin-Konfiguration gestartet.

![005](../images/005.png)

Die Grundlage der Benutzeroberfläche bildet eine Tabulator-Tabelle, die mit den relevanten Daten gefüllt ist.

Beispielsweise wird mit dem Monitor-Modul eine Tabelle mit den Befehlen vom Typ INFO angezeigt, bei denen die Verlaufsfunktion aktiviert ist.

Der Bildschirm besteht aus mehreren Bereichen.

## Die Bedientasten

![006](../images/006.png)

Die Schaltflächen ermöglichen allgemeine Aktionen in Bezug auf die Anzeige, die ausgewählten Zeilen, Aktualisierungen usw.

![013](../images/013.png)

Die oben angezeigten Schaltflächen sind für alle Module gleich und ermöglichen Folgendes:

* die Logdatei von Archiplus anzeigen
* zur ersten oder letzten Zeile der Tabelle springen
* die aktivierten Filter deaktivieren
* Zurück zur ursprünglichen Sortierung
* die in der Tabelle angezeigten Daten exportieren (nur die gefilterten Daten)
* Zurück zu den verschiedenen Modulen von archiplus

![019](../images/019.png)

Über die Standardschaltfläche „Hilfe zur aktuellen Seite“ gelangen Sie zur Dokumentation des Plugins.

## Die Spalte zur Zeilenauswahl

![007](../images/007.png)

In der ersten Spalte können Sie die Zeilen auswählen, auf die Sie eine Aktion anwenden möchten.

Durch Klicken auf die Spaltenüberschriften werden alle angezeigten Zeilen der Tabelle ausgewählt.

Jede Zeile kann einzeln ausgewählt werden, indem man auf das Kontrollkästchen oder eine beliebige Stelle in der Zeile klickt.

Man kann auch eine Reihe von Zeilen auswählen, indem man auf die erste auszuwählende Zeile klickt, dabei die Strg-Taste gedrückt hält und dann auf die letzte Zeile klickt, wobei die Strg-Taste weiterhin gedrückt gehalten wird (Achtung: Klicken Sie unbedingt an eine beliebige Stelle auf der Zeile, aber nicht auf das Auswahlfeld, da sonst die Mehrfachauswahl nicht funktioniert).

## Die Spaltenüberschriften

![008](../images/008.png)

Die Spaltenüberschriften beschreiben den Inhalt der Zellen in der jeweiligen Spalte.

Sie ermöglichen:

* zusätzliche Informationen über einen Tooltip zu erhalten, indem man die Maus eine Sekunde lang über dem Feld verweilt
* die Zeilen nach dem Wert des Feldes sortieren, indem Sie auf die Spaltenüberschrift klicken (beachten Sie, dass Sie mit der Schaltfläche „Sortierung zurücksetzen“ alle vorgenommenen Sortierungen rückgängig machen können)
* Sie können die angezeigten Zeilen filtern, indem Sie ein Auswahlkriterium in das Feld unterhalb des Spaltennamens eingeben (beachten Sie, dass Sie mit der Schaltfläche „Zurücksetzen“ alle Auswahlen aufheben können).

Beim Monitor-Modul ermöglicht eine Gruppierung der Spalten die Auswahl bestimmter Arten von Informationen.

## Die Leitungen

![009](../images/009.png)

In diesen Zeilen werden die angeforderten Informationen angezeigt.

Je nach Kontext erscheint bei einem Rechtsklick ein Kontextmenü mit den verfügbaren Aktionen.

![010](../images/010.png)

Durch Klicken auf ein bearbeitbares Feld kann ein neuer Wert eingegeben werden.

![011](../images/011.png)

Die geänderten Felder werden auf magentafarbenem Hintergrund angezeigt, der nach der Bestätigung der Änderungen wieder verschwindet.

## Die Summen am Ende der Tabelle

![012](../images/012.png)

Am unteren Rand der Tabelle werden die Summen für die angezeigten oder ausgewählten Zeilen angezeigt.

# das Monitor-Modul

Dies ist das Hauptmodul von archiplus.

![005](../images/005.png)

Nach dem Klicken auf „Monitor“ werden die INFO-Befehle mit einem aktiven Verlauf innerhalb weniger Sekunden angezeigt.

![014](../images/014.png)

Wenn Sie auf die Schaltfläche oben klicken, können Sie zur Anzeige aller INFO-Befehle wechseln, auch derjenigen, für die kein Verlauf erforderlich ist, oder derjenigen, bei denen das Gerät inaktiv ist.

## Statistiken

![016](../images/016.png)

Die Anzahl der Einträge in „history“ und „historyArch“ entspricht in der Regel der Anzahl der Einträge bei der letzten Archivierung (das Aktualisierungsdatum wird angezeigt, wenn man den Mauszeiger über einen der Zähler bewegt). Durch Klicken auf die Spaltenüberschrift „#All“ werden sofort die Befehle mit dem umfangreichsten Verlauf angezeigt.

![015](../images/015.png)

Wenn Sie auf die Schaltfläche oben klicken, können Sie eine Berechnung erneut starten, was einige Sekunden dauert.

![017](../images/017.png)

Anhand der Summen am unteren Rand der Tabelle können Sie sofort erkennen, wie groß Ihr Verlauf ist.

## Visualisierung

![018](../images/018.png)

Mit den Anzeigetasten können Sie die angezeigten Daten auswählen

* Konfiguration des Verlaufs
* die Berechnungen
* unzulässige Werte
* Anzeige über Grafiken
* Statistiken

Je nachdem, was Sie interessiert, können Sie den Bereich, den Sie verwalten möchten, aktivieren oder deaktivieren. Um den Startbildschirm von Monitor nicht zu überladen, werden nur die Anmeldedaten, Konfigurationsdaten und Statistiken angezeigt.

## Änderungen

![020](../images/020.png)

Um einen Wert zu ändern, klicken Sie einfach auf das entsprechende Feld und geben Sie einen neuen Wert ein.

![021](../images/021.png)

Die geänderten Daten werden auf magentafarbenem Hintergrund angezeigt.

![022](../images/022.png)

Mit einem Rechtsklick auf eine Zeile kann deren Konfiguration oder einer ihrer Parameter auf die ausgewählten Zeilen kopiert werden.

![023](../images/023.png)

Um die Daten vor der Freigabe zu überprüfen, können nur die geänderten Zeilen angezeigt werden.

![024](../images/024.png)

Nach dem Klicken auf die Schaltfläche „Bestätigen“ werden die Daten aktualisiert und der Hintergrund der geänderten Zellen wird gelöscht.

![025](../images/025.png)

Beachten Sie, dass Sie durch einen Rechtsklick auf eine Zeile direkt die erweiterte Konfiguration der Jeedom-Steuerung aufrufen können.

## Änderungen anhand einer Excel-Datei

![070](../images/070.png)

Es ist auch möglich, Änderungen aus einer Excel- oder CSV-Datei zu laden, indem Sie auf die Schaltfläche „Importieren“ klicken. Über diese Schaltfläche können Sie die Datei auswählen und die geänderten Daten in die Tabelle laden.

![071](../images/071.png)

Die Daten müssen dasselbe Format haben wie die beim Export generierten Daten. Es ist daher möglich, die Daten zu exportieren, sie in Excel zu bearbeiten und die Änderungen anschließend in die Tabelle zu laden.

Es ist auch möglich, die Archivierungseinstellungen aus einem Jeedom-Backup zu extrahieren und die Änderungen zu laden: So lassen sich die seit dem Backup vorgenommenen Änderungen schnell einsehen und gegebenenfalls kann zu einem früheren Zustand zurückgekehrt werden.

![072](../images/072.png)

Nach Abschluss des Imports können Sie durch Klicken auf den Filter „Aktualisierungen“ nur die geänderten Daten anzeigen lassen. Sie können auch auf die Anzeigeschaltflächen (Konfiguration, Berechnungen usw.) klicken, um alle bearbeitbaren Daten anzuzeigen.

Um die Änderungen zu übernehmen, klicken Sie auf die Schaltfläche „Bestätigen“.

## Bearbeitbare Daten

Alle Einstellungsdaten des Standard-Verlaufs von Jeedom sowie die spezifischen Daten des Archiplus-Plugins können direkt über Monitor geändert werden.

Im Folgenden werden die spezifischen Optionen von archiplus detailliert beschrieben:

### KLV (Keep Last Value)

Stellt sicher, dass immer mindestens ein Eintrag im Verlauf erhalten bleibt. Informationen zur Verwendung dieser Option [„Keep Last Value“](#keep-last-value) finden Sie in den folgenden FAQ.

### Uniq

Ermöglicht das Entfernen aufeinanderfolgender identischer Werte in historyArch (und gegebenenfalls in history). Weitere Informationen zur Verwendung dieser Option finden Sie in der folgenden FAQ [Uniq](#uniq-1).

### Frist

Dies ist der Zeitraum, nach dessen Ablauf die Protokolleinträge aus „history“ in „historyArch“ verschoben werden. Standardmäßig ist dieser Parameter in Jeedom für alle Befehle gleich. Mit archiplus kann dieser Zeitraum für jeden Befehl individuell festgelegt werden.

### Rahmung

Ermöglicht die Festlegung des Zeitpunkts, bis zu dem historische Daten gelöscht werden, sowie des Zeitpunkts für die Übertragung der historischen Daten von „history“ nach „historyArch“ anhand einer Begrenzung nach Tag, Stunde oder Minute. Informationen zur Verwendung dieser Option finden Sie in der folgenden FAQ [Zeitlimit und Zeitrahmen](#zeitlimit-und-zeitrahmen).

### Teich

Ermöglicht die Berechnung eines zeitgewichteten Durchschnitts unter Berücksichtigung der Zeit und nicht eines Durchschnitts der über den Zeitraum aufgezeichneten Werte. Weitere Informationen zur Verwendung dieser Option finden Sie in der folgenden FAQ [Glättung und Gewichtung](#glättung-und-gewichtung).

### Paket

Legt fest, in welchen Intervallen die Daten bei der Glättung zusammengefasst werden. In der Standardarchivierung von Jeedom ist dieser Parameter für alle Befehle gleich und entspricht einem Vielfachen von Stunden. Mit archiplus kann das Intervall für jeden Befehl individuell festgelegt und der Wert auch in Minuten angegeben werden (geben Sie die Anzahl der Minuten gefolgt vom Buchstaben m ein).  Lesen Sie die folgende FAQ, um die Verwendung dieser Option [Pack](#pack-1) zu verstehen.

### Abgerundet

Standardmäßig kann man in Jeedom die Rundung für jeden Befehl festlegen. Das Plugin ermöglicht es zusätzlich, beim Glätten der Daten in historyArch eine andere Rundung festzulegen. In der folgenden FAQ erfährst du mehr über die Verwendung dieser Option [Rundung](#arrondi-1).

## Über das Kontextmenü zugängliche Funktionen

![026](../images/026.png)

Durch einen Rechtsklick an einer beliebigen Stelle in einer Zeile der Tabelle wird das Kontextmenü des Befehls angezeigt. Zusätzlich zu den bereits vorgestellten Aktionen bietet dieses folgende Möglichkeiten:

* den Verlauf als Grafik anzeigen  (Aufruf der Standardfunktion von Jeedom)
* die in den Tabellen „history“ und „historyArch“ gespeicherten Daten anzuzeigen
* den Verlauf bis zu einem bestimmten Datum zu löschen
* Historische Daten im CSV-Format exportieren (Aufruf der Standardfunktion von Jeedom)
* die Statistiken für die betreffende Zeile zu aktualisieren
* die Archivierung nur für den betreffenden Auftrag zu starten
* Daten von historyArch nach history kopieren: Lesen Sie die folgende FAQ, um die Verwendung dieser Aktion zu verstehen  [historyArch nach history](#Daten-von-historyArch-nach-history-kopieren)
* den Verlauf des ausgewählten Auftrags in einen anderen Auftrag zu kopieren

# Historische Daten

## Zugang

![027](../images/027.png)

Der Zugriff auf die Daten in den Tabellen „history“ und „historyArch“ erfolgt über:

* das Kontextmenü von „Monitor“ (siehe oben)
* die Auswahl einer oder mehrerer Zeilen, gefolgt vom Drücken der Taste „Data“.

![028](../images/028.png)

Die Daten werden in einem modalen Fenster nach Datums- und Uhrzeitangaben in absteigender Reihenfolge sortiert angezeigt.

## Änderung

![029](../images/029.png)

Manchmal treten Ausreißer auf, in diesem Fall aufgrund von Wartungsarbeiten am Heizkessel.

![030](../images/030.png)

Über das Kontextmenü können Sie den betreffenden Wert ändern oder löschen.

![031](../images/031.png)

Nach der Korrektur ist die Anzeige des Verlaufs nun wesentlich aussagekräftiger.

## Löschen

![032](../images/032.png)

Es ist auch möglich, mehrere historische Daten zu löschen, indem man sie auswählt und auf die Schaltfläche „Löschen“ klickt.

## Export

![033](../images/033.png)

Mit der Schaltfläche „Exportieren“ können Sie die Daten exportieren.

Beachten Sie, dass diese in Excel bearbeitet werden können, um sie über das Import-Modul zu importieren.

# Das Import-Modul

Mit dem Import-Modul können historische Daten in einen oder mehrere Befehle vom Typ INFO importiert werden.

![035](../images/035.png)

Die zu importierende Datei muss im Excel- oder CSV-Format vorliegen und mindestens die folgenden drei Spalten enthalten (alle anderen Spalten werden ignoriert):

* id: Auftrags-ID
* Datums- und Zeitangabe: Datums- und Zeitangabe der historischen Daten im Format JJJJ-MM-TT HH:MM:SS (das interne Datums- und Zeitformat von Excel wird ebenfalls unterstützt)
* value: zu importierender Wert

Stellen Sie sicher, dass die aus den Modulen „Monitor“ oder „Restore“ extrahierten Daten das richtige Format haben.

![034](../images/034.png)

Als Erstes müssen Sie die Datei auswählen, die die Daten enthält.

![036](../images/036.png)

Nach dem Laden werden die historischen Daten aus der Datei geladen.

Die Daten des Befehls „INFO“ werden aus Jeedom abgerufen.

Es wird eine Überprüfung durchgeführt und fehlerhafte Daten werden erkannt.

![037](../images/037.png)

Sie können die geladenen Zeilen einem anderen Befehl zuweisen, indem Sie die betreffende(n) Zeile(n) auswählen und auf die Schaltfläche „Befehl ändern“ klicken.

![038](../images/038.png)

Um die historischen Daten in Jeedom zu importieren, müssen Sie die betreffende(n) Zeile(n) auswählen (hier Filter nach einem Datumsbereich) und auf die Schaltfläche „Importieren“ klicken. Zeilen mit Fehlern werden ignoriert.

![039](../images/039.png)

Beachten Sie, dass der Import über die Standardmethode cmd::addHistoryValue erfolgt. Daher werden die Standardprüfungen und -verarbeitungen von Jeedom durchgeführt. Die neuen Einträge befinden sich in der Tabelle „history“.

# Das Restore-Modul

Mit dem Modul „Restore“ können Sie historische Daten aus einem Standard-Jeedom-Archiv extrahieren und exportieren, um sie anschließend mit dem Modul „Import“ zu importieren.

Alle Verarbeitungsschritte erfolgen lokal im Webbrowser. Alle Befehle und historischen Daten werden in den Speicher des Browsers geladen. Das Programm wurde mit 1,5 Millionen Zeilen in „history“ und „historyArch“ getestet. Die maximale Anzahl der geladenen Daten hängt vom dem Browser zugewiesenen Arbeitsspeicher (RAM) ab und lässt sich nicht im Voraus bestimmen. Es sollte jedoch in der Lage sein, die meisten Sicherungen zu laden, bei denen der Verlauf nicht übermäßig groß geworden ist.

![040](../images/040.png)

Der erste Schritt besteht darin, das Backup lokal auf den Computer zu übertragen. Informationen zur Verwaltung von Jeedom-Backups finden Sie in der folgenden Dokumentation [hier](https://doc.jeedom.com/fr_FR/core/4.5/backup).

![041](../images/041.png)

Starten Sie das Restore-Modul und wählen Sie das Archiv aus, das Sie verwenden möchten.

![042](../images/042.png)

Nach einigen Sekunden werden die Befehle mit ihrem Verlauf angezeigt.

![043](../images/043.png)

Sie können die Befehle auswählen, die Sie interessieren, und den Export starten.

![044](../images/044.png)

Sie können außerdem die entsprechenden historischen Daten anzeigen und diejenigen auswählen, die exportiert werden sollen.

![045](../images/045.png)

In beiden Fällen erhalten Sie eine Exportdatei, die Sie für einen Import mit dem Import-Modul verwenden können.


![073](../images/073.png)

Durch Klicken auf die Schaltflächen zur Anzeige können die Parameter der INFO-Befehle so angezeigt werden, wie sie zum Zeitpunkt der Speicherung waren. Mit dem Filter „Alle“ lassen sich alle INFO-Befehle anzeigen.

Über die Schaltfläche „Exportieren“ kann eine Datei erstellt werden, mit der die Konfigurationsänderungen seit der letzten Sicherung in das Monitor-Modul geladen werden können.

# Häufig gestellte Fragen

## Letzten Wert beibehalten

In bestimmten Fällen ist es erforderlich, den letzten Wert des Befehls INFO zu kennen.

![046](../images/046.png)

Nehmen wir den Fall eines Heizkessels, bei dem regelmäßig der für die Heizung zuständige Gaszähler abgelesen wird.

![047](../images/047.png)

Ein stündlich ausgeführtes Szenario ermöglicht die Berechnung des stündlichen Verbrauchs, indem die Differenz zwischen dem Wert zu Beginn und am Ende der Stunde aus dem Verlauf ermittelt wird. Dazu reicht ein Tagesverlauf aus.

Wenn die Heizperiode jedoch beendet ist, geht der Verlauf des Heizungszählers verloren und steht nicht mehr zur Verfügung, um den ersten Stundenverbrauch beim ersten Heizen der folgenden Saison zu berechnen.

Durch Aktivieren der Option „Keep Last Value“ lässt sich dieses Problem beheben, ohne dass man auf programmiertechnische Tricks zurückgreifen oder einen Verlauf über ein Jahr hinweg speichern muss.

## Uniq

Jeedom verhindert Duplikate in der History-Tabelle mithilfe der Option „Identische Werte wiederholen“, die standardmäßig deaktiviert ist.

Es gibt jedoch mehrere Situationen, in denen aufeinanderfolgende identische Werte nicht ignoriert werden:

  * wenn der Untertyp des Befehls „Binär“ oder „Sonstiges“ ist
  * wenn die Aktualisierung mit der Methode `cmd::event` und nicht mit `eqLogic::checkAndUpdateCmd` durchgeführt wird. Viele Plugins arbeiten noch mit der älteren Methode `cmd::event`, die daher keine Duplikate entfernt.

Wenn bei der Archivierung keine Glättung erfolgt, werden die Daten aus „history“ direkt in „historyArch“ übertragen, sodass Duplikate kopiert werden.

Durch Aktivieren der Option „Uniq“ werden bei der spezifischen Archivierung mit archiplus Duplikate in historyArch entfernt.

Wenn das Plugin zudem so konfiguriert ist, dass die Einträge aus „history“ nicht in „historyArch“ kopiert werden, werden auch die Duplikate in „history“ gelöscht.

## Zeitrahmen und Projektumfang

Standardmäßig wird der Zeitpunkt, ab dem Daten in „history“ und „historyArch“ gelöscht werden, durch den Parameter „Protokoll löschen“ festgelegt, der in Stunden angegeben wird. Ein Standardwert ist in der globalen Konfiguration von Jeedom definiert.

Wenn also die Löschfrist auf 7 Tage festgelegt ist und die Archivierung am 20.01.2025 um 05:11:21 gestartet wird, werden die Einträge in den Verzeichnissen „history“ und „historyArch“ bis zum 13.01.2025 um 05:11:21 gelöscht.

Mit der archiplus-spezifischen Einstellung „Cadrage“ lässt sich der Zeitpunkt der Entlüftung genauer festlegen. Im obigen Beispiel erfolgt die Entlüftung somit:

* am 13.01.2025 um 05:11:21 Uhr, wenn kein Rahmen definiert ist
* am 13.01.2025 um 05:11:00 Uhr mit Fokus auf die letzte Minute
* am 13.01.2025 um 05:00:00 Uhr, mit Fokus auf die letzte Stunde
* am 13.01.2025 um 00:00:00 Uhr, mit Fokus auf den letzten Tag

Mit der „Wartezeit bis zur Archivierung“ (in Stunden) lässt sich festlegen, ab wann die History-Aufzeichnungen in historyArch übertragen werden (mit oder ohne Konsolidierung). Standardmäßig ist dieser Wert global definiert und gilt somit für alle Befehle gleichermaßen.

Die spezifische Archivierung von archiplus ermöglicht es, für jeden INFO-Befehl eine bestimmte Frist festzulegen und das oben gezeigte Schema zu verwenden. Bei einer Frist von 2 Stunden erfolgt die Übertragung von „history“ nach „historyArch“ somit zu folgendem Zeitpunkt:

* am 20.01.2025 um 03:11:21 Uhr, sofern kein Rahmen definiert ist
* am 20.01.2025 um 03:11:00 Uhr mit Fokus auf die letzte Minute
* am 20.01.2025 um 03:00:00 Uhr, mit Fokus auf die letzte Stunde
* am 20.01.2025 um 00:00:00 Uhr mit einer Einschränkung auf den letzten Tag, unabhängig davon, zu welcher Uhrzeit die Archivierung gestartet wird

Beachten Sie, dass der Zeitpunkt der Bereinigung nicht nach dem Zeitpunkt der Übertragung von „history“ nach „historyArch“ liegen darf und daher automatisch angepasst wird.

![048](../images/048.png)

Man kann diese Parameter anpassen, wenn man beispielsweise einen detaillierten Verlauf über einen kurzen Zeitraum (hier maximal 36 Stunden) wünscht, ohne dass eine konsolidierte Archivierung erforderlich ist. So vermeidet man die Übertragung des Verlaufs in „historyArch“, die keinen Mehrwert bietet.

## Glättung und Gewichtung

Die Glättung erfolgt beim Kopieren der History-Daten in die historyArch. Der Archivierungsprozess berücksichtigt alle History-Daten entsprechend dem festgelegten Intervall (standardmäßig eine Stunde) und speichert einen einzigen Wert, der gemäß dem Glättungsmodus berechnet wird. Es stehen drei Modi zur Auswahl:

* Minimum: der kleinste der im Intervall enthaltenen Werte
* Maximum: der größte Wert im Intervall
* Mittelwert: der Durchschnitt der Werte in diesem Intervall

Es ist zu beachten, dass die Standardarchivierung den Wert des Befehls zu Beginn des Intervalls nicht berücksichtigt und einen Mittelwert aus den im Intervall vorhandenen Werten bildet, was das Ergebnis erheblich verfälschen kann.

Der spezifische Archivierungsprozess von archiplus bietet eine Option namens „Pond“, mit der dieses Phänomen korrigiert und ein exaktes Ergebnis für den betrachteten Zeitraum berechnet werden kann.

Dies wird im folgenden Beispiel veranschaulicht.

![050](../images/050.png)

Betrachten wir zwei Befehle mit den folgenden Konfigurationen.

![049](../images/049.png)

Sie haben dieselben Einträge in der Tabelle „history“

![051](../images/051.png)

Nach der Archivierung unterscheiden sich die Einträge in „historyArch“

![052](../images/052.png)

Bei der Standardarchivierung wird der Durchschnittswert des Zeitraums berücksichtigt.

Bei der spezifischen Archivierung von archiplus wird der gewichtete Durchschnitt über den Zeitraum berechnet. Beachten Sie außerdem, dass ein Eintrag in „history“ hinzugefügt wird, um bei der nächsten Archivierung den Startwert des Zeitraums zu ermitteln (ohne diesen Eintrag würde der Durchschnitt des letzten Zeitraums übernommen, was die Berechnung verfälschen würde).

## Paket

Standardmäßig ist in Jeedom das Intervall (in Jeedom als „Paket“ bezeichnet), über das eine Glättung vorgenommen werden kann, in Stunden definiert und für alle INFO-Befehle gleich.

Man könnte sich jedoch ein kleineres Intervall wünschen und dieses für einen bestimmten INFO-Befehl festlegen können.

![055](../images/055.png)

![054](../images/054.png)

Bei einer Batterie kann es ausreichen, über einen langen Zeitraum hinweg täglich einen Wert zu speichern.

![057](../images/057.png)

![056](../images/056.png)

Bei einem Thermometer kann ein Messwert alle Viertelstunde aussagekräftiger sein als ein stündlicher Messwert.

Um Minuten anzugeben, geben Sie im Feld „Pack“ die gewünschte Anzahl an Minuten ein, gefolgt von „m“, zum Beispiel 15m.

## Abgerundet

Standardmäßig ermöglicht Jeedom die Festlegung der Anzahl der Dezimalstellen eines INFO-Befehlswerts.

Bei bestimmten Steuerungsvorgängen kann es sinnvoll sein, über einen kurzen Zeitraum einen genauen Wert zu haben und später einen weniger genauen. Beispielsweise ist die Kenntnis einer genauen Außentemperatur im Moment interessant, nach einigen Tagen jedoch nicht mehr erforderlich.

![064](../images/064.png)

Der obige Befehl ist so konfiguriert, dass er einen Verlauf mit einer Dezimalstelle eine Woche lang speichert und darüber hinaus einen Verlauf ohne Dezimalstelle.

![065](../images/065.png)

Vor der Archivierung gab es 7 Einträge im Verlauf zwischen 7,7 °C und 8,3 °C.

![066](../images/066.png)

Nach der Speicherung werden die 7 Eingabewerte auf 8 °C gerundet, und mit der Option „Uniq“ kann nur einer davon beibehalten werden.

## Daten von „historyArch“ nach „history“ kopieren

Nach der Installation von archiplus möchten Sie möglicherweise vorhandene Verlaufsdaten konsolidieren.

![060](../images/060.png)

![058](../images/058.png)

Für diesen Befehl wäre beispielsweise ein Verlauf in 10-Minuten-Intervallen ausreichend und würde die Anzahl der Einträge in historyArch erheblich reduzieren.

![059](../images/059.png)

Nachdem die Einstellungen geändert wurden, können die Einträge aus „historyArch“ in „history“ übertragen werden.

![061](../images/061.png)

Sobald dieses Update durchgeführt wurde, kann man über den Befehl INFO eine Archivierung starten (oder warten, bis die Archivierung nachts automatisch gestartet wird).

![063](../images/063.png)

![062](../images/062.png)

Nach der Archivierung wird die Anzahl der Datensätze deutlich reduziert, und die Anzeige des Verlaufs erfolgt wesentlich schneller.

## Archiplus in PHP verwenden

Die Funktionen zur Archivierung und Verarbeitung von Verlaufsdaten in archiplus können direkt in einem Szenario oder einer PHP-Funktion aufgerufen werden.

![053](../images/053.png)

Hier werden die archiplus-Funktionen in einem Szenario verwendet, um den Verlauf eines Auftrags zu laden und die Archivierung für diesen Auftrag zu starten.

`require_once dirname(__FILE__) . '../../../plugins/archiplus/core/class/archiplus.class.php';`

Mit dieser Zeile wird der Code der archiplus-Funktionen geladen. Gegebenenfalls muss der Pfad angepasst werden, damit er auf die Klasse des Plugins verweist.

Die verfügbaren Funktionen finden Sie im Code der Klasse „archiplus“. Die wichtigsten sind:

* `archive($_cmd_id = '')`: Startet die Archivierung für einen Auftrag oder für alle Aufträge, wenn kein Parameter angegeben ist
* `History_purge($_cmd_id, $_date='')`: Löscht den Verlauf für einen Befehl bis zu einem bestimmten Datums- und Zeitstempel (oder den gesamten Verlauf, wenn kein zweiter Parameter angegeben ist)
* `addHistoryValue($_cmd_id, $_datetime, $_value)`: Fügt einen Eintrag zum Verlauf hinzu (oder ersetzt den bestehenden Eintrag), indem die Standardfunktion von Jeedom aufgerufen wird
* `historyArch2history($_cmd_id, $_date_start = '', $_date_end = '')`: Überträgt die Datensätze aus historyArch in history
  
Selbstverständlich können die in der Klasse „history.class.php“ verfügbaren Funktionen genutzt werden, nachdem die erforderliche `require_once`-Anweisung eingefügt wurde.

# Die Protokolle

Wenn die Protokollierungsstufe in der Plugin-Konfiguration mindestens auf „Info“ eingestellt ist, werden die verschiedenen Ereignisse im Zusammenhang mit archiplus im archiplus-Protokoll von Jeedom aufgezeichnet. Der Zugriff darauf ist direkt über die Schaltfläche „Protokoll“ in den verschiedenen archiplus-Modulen möglich.

![068](../images/068.png)

Beim Archivieren werden die allgemeinen Archivierungseinstellungen von Jeedom angezeigt.

![067](../images/067.png)

Anschließend werden für jeden Befehl die durchgeführten Vorgänge sowie die Anzahl der Einträge in „history“ und „historyArch“ vor und nach diesem Befehl detailliert aufgeführt.

![069](../images/069.png)

Sie können das Protokoll für einen bestimmten Befehl anzeigen, indem Sie dessen Nummer, vorangestellt von den Zeichen „-“ und einem Leerzeichen, in das Suchfeld eingeben.

# Übersetzung

Die Benutzeroberfläche, die in den Protokollen ausgegebenen Meldungen und die Dokumentation sind in die fünf von Jeedom unterstützten Sprachen übersetzt (vielen Dank an @mips für die Entwicklung von ga-translation und docs-translations). Sollten Sie Übersetzungsfehler feststellen, können Sie eine Supportanfrage stellen und, wenn möglich, die korrigierte Übersetzungsdatei (im Verzeichnis core/i18n des Plugins) beifügen.

# Bewertungen

![archiplus_Bewertung](../images/archiplus_avis.png)

Wenn Ihnen dieses Plugin gefällt, hinterlassen Sie bitte eine Bewertung und einen Kommentar im Jeedom Market – das freut uns immer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=xxxx#>
