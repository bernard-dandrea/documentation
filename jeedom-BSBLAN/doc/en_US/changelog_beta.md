# BSBLAN Plugin Changelog

# 21/07/2026

- Database encryption of access codes
- Improvements to cron management
- Ability to run cron jobs in the task engine
- Code cleanup

# 05/01/2026

- Replace "event" with "checkAndUpdateCmd" to prevent duplicate values in the history
- Moved the documentation to a separate GitHub repository so that the documentation can be updated without triggering a plugin update

# 27/01/2025

- Management of update commands via JSON or URL/S (see documentation)

# 10/11/2024

- Documentation Update

# 07/11/2024

- Converting cron methods to static to avoid errors in PHP 8

# 06/08/2024

- Ability to resubmit a command that failed multiple times

After upgrading to Debian 11, I noticed that I was getting timeouts after sending commands to the BSBLAN (this didn’t happen in Debian 10, and I don’t know where to look to fix the problem at the OS level). When I resubmit the command, it usually goes through without any issues. That’s why I added a “Number of Attempts” option for each device, which allows the command to be submitted multiple times.

# 28/04/2024

- Minor update to the documentation

# 25/02/2024

- Increase in command name length from 40 to 100 characters
- Documentation Update

# 28/12/2023

- Add a "refresh" command (this is created when the device is saved)
  
# 21/10/2023

- Update debug message
- Update index and changelog for the beta version

# 01/08/2023

- Adding a timeout for HTTP requests

# 10/07/2023

- Initial load

