# Changelog plugin SNMP3

# 03/07/2026

- Implement encryption and decryption for passphrases

# 02/07/2026

- Amélioration de la gestion des crons

# 09/06/2026

- Définition des méthodes cron en static pour éviter erreur en PHP 8 (modifications perdues lors d'une mise à jour)

# 01/01/2026

- Déplacement de la documentation dans un repository github séparé afin de pouvoir mettre à jour la documentation sans générer un update du plugin
- Suppression des warnings PHP

# 28/03/2025

- Ajout de community RW (habituellement 'private') pour les mises à jour en protocol v1/v2c

# 17/10/2024

- Définition des méthodes cron en static pour éviter erreur en PHP 8
- Correction bug sur commandes refresh
  
# 11/08/2024

- la gestion du retry est maintenant gérée par le plugin directement et non par la bibliothèque Net-SNMP

# 20/02/2024

- Initial load

