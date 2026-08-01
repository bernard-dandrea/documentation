# SNMP3 Plugin Changelog

# 25/07/2026

- Ability to set up a standalone cron job in the task engine
- Database encryption of passphrases
- Improvements to cron management

# 16/06/2026

- Defining cron methods as static to avoid errors in PHP 8 (changes lost during an update)

# 01/01/2026

- Moved the documentation to a separate GitHub repository so that the documentation can be updated without triggering a plugin update
- Removing PHP warnings

# 28/03/2025

- Addition of an RW community (usually 'private') for updates using protocol v1/v2c

# 17/10/2024

- Defining cron methods as static to avoid errors in PHP 8
- Bug fix for refresh commands
  
# 11/08/2024

- Retry handling is now managed directly by the plugin rather than by the Net-SNMP library

# 20/02/2024

- Initial load

