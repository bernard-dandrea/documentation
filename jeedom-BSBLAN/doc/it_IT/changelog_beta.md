# Registro delle modifiche del plugin BSBLAN

# 21/07/2026

- Crittografia dei codici di accesso nel database
- Miglioramento della gestione dei cron
- Possibilità di avviare il cron nel motore delle attività
- Riorganizzazione del codice

# 05/01/2026

- Sostituzione di "event" con "checkAndUpdateCmd" per evitare la ripetizione dei valori nella cronologia
- Spostamento della documentazione in un repository GitHub separato per poter aggiornare la documentazione senza generare un aggiornamento del plugin

# 27/01/2025

- Gestione dei comandi di aggiornamento tramite JSON o URL /S (vedere la documentazione)

# 10/11/2024

- Aggiornamento della documentazione

# 07/11/2024

- Conversione dei metodi cron in statici per evitare errori in PHP 8

# 06/08/2024

- Possibilità di inviare più volte un ordine che non è andato a buon fine

In seguito al passaggio a Debian 11, ho notato che si verificavano dei timeout dopo aver inviato comandi al BSBLAN (ciò non accadeva con Debian 10 e non so dove cercare per risolvere il problema a livello di sistema operativo). Inviando nuovamente il comando, questo in genere viene eseguito senza problemi. Per questo motivo ho aggiunto, a livello di ogni dispositivo, un’opzione “Numero di tentativi” che consente di inviare il comando più volte.

# 28/04/2024

- Aggiornamento minore della documentazione

# 25/02/2024

- Aumento della lunghezza dei nomi dei comandi da 40 a 100 caratteri
- Aggiornamento della documentazione

# 28/12/2023

- Aggiunta di un comando di aggiornamento (che viene creato quando si salva l'apparecchiatura)
  
# 21/10/2023

- Messaggio di debug dell'aggiornamento
- Aggiornamento dell'indice e del changelog per la versione beta

# 01/08/2023

- Aggiunta di un timeout per le richieste HTTP

# 10/07/2023

- Caricamento iniziale

