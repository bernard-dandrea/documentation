# Änderungsprotokoll des EcoNetatmo-Plugins

# 05/08/2026

- Übersetzung des Plugins und der Dokumentation in en_US, es_ES, de_DE, it_IT, pt_PT
- Umstellung auf Vanilla JS
- Mindestversion von Jeedom -> 4.4.0
- Code-Bereinigung und Optimierungen

# 22/07/2026

- Möglichkeit, einen eigenständigen Cron-Job in der Aufgaben-Engine zu definieren

# 07/11/2025

- Behebung eines Problems in PHP 8 bei der Erneuerung von Tokens
- Korrektur zur Beseitigung der PHP-Warnung bei der Synchronisierung
- Die Dokumentation wurde in ein separates GitHub-Repository verschoben, damit sie aktualisiert werden kann, ohne dass ein Update des Plugins erforderlich ist
- Plugin-Validierung unter Debian 12 Jeedom 4.5
  
# 09/09/2025

- Korrektur der Netamo-Internetadressen (ersetzen Sie .net durch .com): Es ist wahrscheinlich erforderlich, die Tokens neu zu generieren, um die Kommunikation wiederherzustellen

# 20/05/2025

- Änderung der Netatmo-Zugriffsbibliothek aufgrund eines Problems, das den Datenabruf verhinderte

# 07/11/2024

- Umstellung der Cron-Methoden auf statisch, um Fehler in PHP 8 zu vermeiden

# 25/02/2024

- Aktualisierung der Dokumentation

# 21/07/2023

- Entfernung des Parameters „scope“ beim Aktualisieren des Tokens

# 20/07/2023

- Änderung zur Unterstützung der Authentifizierung über authorization_code (siehe Dokumentation)

# 26/05/2023

- PayPal-Link entfernen
- Änderung der Links zur GitHub-Dokumentation
- Links zur Beta-Dokumentation hinzugefügt

# 25/05/2023

- Erstladung
