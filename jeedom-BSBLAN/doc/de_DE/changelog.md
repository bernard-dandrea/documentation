# Änderungsprotokoll des BSBLAN-Plugins

# 28/07/2026

- Verschlüsselung der Zugangscodes in der Datenbank
- Verbesserung der Cron-Verwaltung
- Möglichkeit, den Cron-Job in der Aufgaben-Engine zu starten
- Code-Bereinigung

# 05/01/2026

- Ersetzen von „event“ durch „checkAndUpdateCmd“, um doppelte Einträge im Verlauf zu vermeiden
- Die Dokumentation wurde in ein separates GitHub-Repository verschoben, damit sie aktualisiert werden kann, ohne dass ein Update des Plugins erforderlich ist

# 27/01/2025

- Verwaltung von Aktualisierungsbefehlen über JSON oder URL/S (siehe Dokumentation)

# 10/11/2024

- Aktualisierung der Dokumentation

# 07/11/2024

- Umstellung der Cron-Methoden auf statische Methoden, um Fehler in PHP 8 zu vermeiden

# 06/08/2024

- Möglichkeit, eine fehlgeschlagene Bestellung mehrmals erneut aufzugeben

Nach dem Umstieg auf Debian 11 habe ich festgestellt, dass es nach dem Absenden von Befehlen an den BSBLAN zu Timeouts kam (unter Debian 10 trat dieses Problem nicht auf, und ich weiß nicht, wo ich nach einer Lösung auf Betriebssystemebene suchen soll). Wenn ich den Befehl erneut sende, wird er in der Regel problemlos ausgeführt. Aus diesem Grund habe ich bei jedem Gerät eine Option „Anzahl der Versuche“ hinzugefügt, die es ermöglicht, den Befehl mehrmals zu senden.

# 28/04/2024

- Kleinere Aktualisierung der Dokumentation

# 25/02/2024

- Erweiterung der zulässigen Länge von Bestellnamen von 40 auf 100 Zeichen
- Aktualisierung der Dokumentation

# 28/12/2023

- Hinzufügen eines Refresh-Befehls (dieser wird beim Speichern des Geräts erstellt)
  
# 21/10/2023

- Debug-Meldung aktualisieren
- Update des Index und des Changelogs für die Beta-Version

# 01/08/2023

- Hinzufügen eines Timeouts für HTTP-Anfragen

# 10/07/2023

- Erstladung

