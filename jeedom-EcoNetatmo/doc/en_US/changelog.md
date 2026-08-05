# EcoNetatmo plugin changelog

# 26/07/2026

- Ability to set up a standalone cron job in the task engine

# 07/11/2025

- Fixed an issue in PHP 8 related to token renewal
- Fix to remove PHP warning during synchronization
- Moved the documentation to a separate GitHub repository so that the documentation can be updated without triggering a plugin update
- Plugin validation on Debian 12 Jeedom 4.5
  
# 09/09/2025

- Correction to Netamo's web addresses (replace .net with .com): You will likely need to regenerate the tokens to restore communication

# 20/05/2025

- Modification to the Netatmo access library following an issue that prevented data retrieval

# 07/11/2024

- Converting cron methods to static to avoid errors in PHP 8

# 25/02/2024

- Documentation Update

# 21/07/2023

- Removal of the "scope" parameter when refreshing the token

# 20/07/2023

- Change to handle authentication via authorization_code (see documentation)

# 26/05/2023

- Remove PayPal link
- Changes to links to the GitHub documentation
- Added links to the beta documentation

# 25/05/2023

- Initial loading
