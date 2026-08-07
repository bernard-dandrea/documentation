# EcoLegrand plugin changelog

# 06/08/2026

- Translation of the plugin and documentation into en_US, es_ES, de_DE, it_IT, pt_PT
- switch to vanilla JS
- Minimum Jeedom version -> 4.4.0
- code cleanup and optimizations
- 
# 22/07/2026

- Ability to set up a standalone cron job in the task engine

# 01/01/2026

- Changes to convert the values retrieved from the energy meter to float and prevent errors when adding them in PHP 8 (thanks to Michel_F)
- Moved the documentation to a separate GitHub repository so that the documentation can be updated without triggering a plugin update

# 28/03/2025

- Retrieving non-numeric fields

# 07/11/2024

- Converting cron methods to static to avoid errors in PHP 8
- Documentation Improvements

# 25/02/2024

- Documentation Update

# 21/12/2023

- Error message in the log if the JSON returned by the eco-meter cannot be decoded (see FAQ)
  
# 24/08/2023

- Fixed a bug that occurred when creating commands

# 14/08/2023

- Add log entry upon counter creation

# 05/08/2023

- Initial load
