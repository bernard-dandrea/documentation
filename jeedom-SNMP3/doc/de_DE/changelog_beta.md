# Änderungsprotokoll des SNMP3-Plugins

# 22/07/2026

- Möglichkeit, einen eigenständigen Cron-Job in der Aufgaben-Engine zu definieren

# 03/07/2026

- Verschlüsselung der Passphrasen in der Datenbank

# 02/07/2026

- Verbesserung der Cron-Verwaltung

# 09/06/2026

- Definition der Cron-Methoden als statisch, um Fehler in PHP 8 zu vermeiden (Änderungen gehen bei einem Update verloren)

# 01/01/2026

- Die Dokumentation wurde in ein separates GitHub-Repository verschoben, damit sie aktualisiert werden kann, ohne dass ein Update des Plugins erforderlich ist
- PHP-Warnungen unterdrücken

# 28/03/2025

- Hinzufügen einer RW-Community (normalerweise „privat“) für Updates im Protokoll v1/v2c

# 17/10/2024

- Definition der Cron-Methoden als statisch, um Fehler in PHP 8 zu vermeiden
- Behebung eines Fehlers bei den Aktualisierungsbefehlen
  
# 11/08/2024

- Die Verwaltung der Wiederholungsversuche erfolgt nun direkt über das Plugin und nicht mehr über die Net-SNMP-Bibliothek

# 20/02/2024

- Erstladung

