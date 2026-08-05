# Registro delle modifiche del plugin EcoNetatmo

# 05/08/2026

- traduzione del plugin e della documentazione in en_US, es_ES, de_DE, it_IT, pt_PT
- passaggio a Vanilla JS
- versione minima di Jeedom -> 4.4.0
- ripulitura del codice e ottimizzazioni

# 22/07/2026

- Possibilità di definire un cron autonomo nel motore delle attività

# 07/11/2025

- Risoluzione del problema in PHP8 durante il rinnovo dei token
- Correzione per eliminare l'avviso PHP durante la sincronizzazione
- Spostamento della documentazione in un repository GitHub separato per poter aggiornare la documentazione senza generare un aggiornamento del plugin
- Verifica del plugin su Debian 12 Jeedom 4.5
  
# 09/09/2025

- Correzione degli indirizzi Internet di Netamo (sostituire .net con .com): sarà sicuramente necessario rigenerare i token per ripristinare la comunicazione

# 20/05/2025

- Modifica della libreria di accesso a Netatmo a seguito di un problema che impediva il recupero dei dati

# 07/11/2024

- Conversione dei metodi cron in statici per evitare errori in PHP 8

# 25/02/2024

- Aggiornamento della documentazione

# 21/07/2023

- Rimozione del parametro scope durante l'aggiornamento del token

# 20/07/2023

- Modifica per gestire l'autenticazione tramite authorization_code (vedere la documentazione)

# 26/05/2023

- Rimozione del link PayPal
- Modifica dei link alla documentazione su GitHub
- Aggiunta di link alla documentazione beta

# 25/05/2023

- Caricamento iniziale
