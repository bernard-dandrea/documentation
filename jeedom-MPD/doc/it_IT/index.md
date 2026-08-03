# Plugin MPD

Plugin che consente di controllare un lettore MPD.

Music Player Daemon, o MPD, è un lettore audio open source che consente l'accesso remoto da un altro computer. È presente in background su numerosi server multimediali come moodeaudio, volumio, ...

MPD consente di riprodurre i file audio (= Song) presenti nella propria coda (= Queue). Quest'ultima viene alimentata dalle playlist (le playlist non sono gestite dal plugin).

Il plugin consente di eseguire le funzioni di base (caricamento di playlist, riproduzione, volume, ecc.) da Jeedom. Il plugin utilizza l'utilità mpc per eseguire i comandi sul server MPD, sia esso locale o remoto. Il pacchetto mpc viene installato al momento dell'attivazione del plugin (link GitHub <https://github.com/MusicPlayerDaemon/mpc>).

# Installazione e configurazione del server MPD

Il corretto funzionamento del plugin presuppone che il server MPD sia operativo.

Questo viene solitamente installato in modo trasparente dal server multimediale corrispondente (Volumio, Moodeaudio, ...).

Per impostazione predefinita, il server MPD ascolta i comandi sulla porta 6600. L'accesso può essere protetto da password.

# Configurazione del plugin

Una volta installato il plugin, è necessario attivarlo.

È possibile attivare il livello di log "Debug" per monitorare l'attività del plugin e individuare eventuali problemi.

# Configurazione delle apparecchiature

La configurazione delle apparecchiature è accessibile dal menu del plugin (menu Plugins, Multimedia, quindi MPD).

Fare clic su Aggiungi per configurare un nuovo controller MPD.

![MPD_Attrezzature](../images/MPD_Equipement.png)

Specificare la configurazione dell'MPD:

-   **Nome**: nome dell'MPD
-   **Oggetto padre**: indica l'oggetto padre a cui appartiene l'apparecchio
-   **Categoria**: indica la categoria Jeedom dell'apparecchio; per impostazione predefinita è "Multimedia"
-   **Attiva**: consente di attivare l'apparecchio
-   **Visibile**: lo rende visibile sulla dashboard
-   **Indirizzo IP**: IP del server MPD; lasciare vuoto se installato sul server Jeedom
-   **Porta**: porta di ascolto del server MPD; lasciare vuota se si desidera utilizzare il valore predefinito
-   **Password**: password per l'accesso al server MPD

I seguenti pulsanti consentono di eseguire le seguenti funzioni:

-   **Verifica della connessione all'MPD**:  consente di verificare se le impostazioni di connessione sono corrette (ricordarsi di salvare la configurazione prima di fare clic sul pulsante).
-   **Genera comandi**: consente di generare i comandi necessari per controllare il lettore (utile solo se è stato eliminato uno dei comandi).

# Comandi associati alle apparecchiature

![MPD_Comandi](../images/MPD_Commandes.png)

I comandi di base vengono generati al momento della creazione dell'apparecchio.

Per ogni comando di tipo azione, il campo Comando (memorizzato nel LogicalID del comando Jeedom) indica il comando trasmesso all'utilità mpc. Per ulteriori informazioni, consultare la documentazione di mpc ( <https://www.musicpd.org/doc/mpc/html/> ).

![MPD_Comandi_Aggiunta](../images/MPD_Commandes_Ajout.png)

Il comando "Crea un comando" consente di aggiungere un'azione, ad esempio per creare un collegamento rapido per riprodurre una stazione radio. A tal fine, è possibile utilizzare il comando "playsong", che verrà trasformato in "play" seguito dal numero del brano nella coda.

# Widget

![MPD_Widget](../images/MPD_Widget.png)

La presentazione creata di default consente di eseguire le funzioni di base. Da notare il pulsante "refresh" (in alto a destra del widget) che permette di aggiornare lo stato del lettore MPD (playlist, brano in riproduzione, ...). Selezionando una playlist, si inizializza la coda di MPD con i brani corrispondenti. Selezionando un brano, è possibile riprodurlo.

![MPD_Attrezzature_Disposizione](../images/MPD_Equipement_Disposition.png)

La presentazione viene realizzata utilizzando la funzione "Disposizione delle apparecchiature" (in "Configurazione avanzata").

![MPD_Widget_Preferiti](../images/MPD_Widget_Favoris.png)

Modificando la visualizzazione, è possibile aggiungere scorciatoie.

# Controllo del lettore audio tramite Mi Cube

![MPD_micube](../images/MPD_micube.png)

Utilizzando gli scenari, è possibile controllare il proprio lettore audio senza utilizzare l'interfaccia di Jeedom, tramite un dispositivo di comando come, ad esempio, il Mi Cube di Xiaomi.

![MPD_micube_song](../images/MPD_micube_song.png)

Lo scenario sopra riportato, attivato al cambiamento di stato di #[Nessuno][Cube][side]#, consente di cambiare la stazione radio, ruotando il Mi Cube.

![MPD_micube_toggle](../images/MPD_micube_toggle.png)

Lo scenario sopra riportato, attivato al cambiamento di stato di #[Nessuno][Cube][Pulsante]#, consente di interrompere e riavviare il brano scuotendo il Mi Cube.

# Plugin MPD

Plugin che consente di controllare un lettore MPD.

Music Player Daemon, o MPD, è un lettore audio open source che consente l'accesso remoto da un altro computer. È presente in background su numerosi server multimediali come moodeaudio, volumio, ...

MPD consente di riprodurre i file audio (= Song) presenti nella propria coda (= Queue). Quest'ultima viene alimentata dalle playlist (le playlist non sono gestite dal plugin).

Il plugin consente di eseguire le funzioni di base (caricamento di playlist, riproduzione, volume, ecc.) da Jeedom. Il plugin utilizza l'utilità mpc per eseguire i comandi sul server MPD, sia esso locale o remoto. Il pacchetto mpc viene installato al momento dell'attivazione del plugin (link GitHub <https://github.com/MusicPlayerDaemon/mpc>).

# Installazione e configurazione del server MPD

Il corretto funzionamento del plugin presuppone che il server MPD sia operativo.

Questo viene solitamente installato in modo trasparente dal server multimediale corrispondente (Volumio, Moodeaudio, ...).

Per impostazione predefinita, il server MPD ascolta i comandi sulla porta 6600. L'accesso può essere protetto da password.

# Configurazione del plugin

Una volta installato il plugin, è necessario attivarlo.

È possibile attivare il livello di log "Debug" per monitorare l'attività del plugin e individuare eventuali problemi.

# Configurazione delle apparecchiature

La configurazione delle apparecchiature è accessibile dal menu del plugin (menu Plugins, Multimedia, quindi MPD).

Fare clic su Aggiungi per configurare un nuovo controller MPD.

![MPD_Attrezzature](../images/MPD_Equipement.png)

Specificare la configurazione dell'MPD:

-   **Nome**: nome dell'MPD
-   **Oggetto padre**: indica l'oggetto padre a cui appartiene l'apparecchio
-   **Categoria**: indica la categoria Jeedom dell'apparecchio; per impostazione predefinita è "Multimedia"
-   **Attiva**: consente di attivare l'apparecchio
-   **Visibile**: lo rende visibile sulla dashboard
-   **Indirizzo IP**: IP del server MPD; lasciare vuoto se installato sul server Jeedom
-   **Porta**: porta di ascolto del server MPD; lasciare vuota se si desidera utilizzare il valore predefinito
-   **Password**: password per l'accesso al server MPD

I seguenti pulsanti consentono di eseguire le seguenti funzioni:

-   **Verifica della connessione all'MPD**:  consente di verificare se le impostazioni di connessione sono corrette (ricordarsi di salvare la configurazione prima di fare clic sul pulsante).
-   **Genera comandi**: consente di generare i comandi necessari per controllare il lettore (utile solo se è stato eliminato uno dei comandi).

# Comandi associati alle apparecchiature

![MPD_Comandi](../images/MPD_Commandes.png)

I comandi di base vengono generati al momento della creazione dell'apparecchio.

Per ogni comando di tipo azione, il campo Comando (memorizzato nel LogicalID del comando Jeedom) indica il comando trasmesso all'utilità mpc. Per ulteriori informazioni, consultare la documentazione di mpc ( <https://www.musicpd.org/doc/mpc/html/> ).

![MPD_Comandi_Aggiunta](../images/MPD_Commandes_Ajout.png)

Il comando "Crea un comando" consente di aggiungere un'azione, ad esempio per creare un collegamento rapido per riprodurre una stazione radio. A tal fine, è possibile utilizzare il comando "playsong", che verrà trasformato in "play" seguito dal numero del brano nella coda.

# Widget

![MPD_Widget](../images/MPD_Widget.png)

La presentazione creata di default consente di eseguire le funzioni di base. Da notare il pulsante "refresh" (in alto a destra del widget) che permette di aggiornare lo stato del lettore MPD (playlist, brano in riproduzione, ...). Selezionando una playlist, si inizializza la coda di MPD con i brani corrispondenti. Selezionando un brano, è possibile riprodurlo.

![MPD_Attrezzature_Disposizione](../images/MPD_Equipement_Disposition.png)

La presentazione viene realizzata utilizzando la funzione "Disposizione delle apparecchiature" (in "Configurazione avanzata").

![MPD_Widget_Preferiti](../images/MPD_Widget_Favoris.png)

Modificando la visualizzazione, è possibile aggiungere scorciatoie.

# Controllo del lettore audio tramite Mi Cube

![MPD_micube](../images/MPD_micube.png)

Utilizzando gli scenari, è possibile controllare il proprio lettore audio senza utilizzare l'interfaccia di Jeedom, tramite un dispositivo di comando come, ad esempio, il Mi Cube di Xiaomi.

![MPD_micube_song](../images/MPD_micube_song.png)

Lo scenario sopra riportato, attivato al cambiamento di stato di #[Nessuno][Cube][side]#, consente di cambiare la stazione radio, ruotando il Mi Cube.

![MPD_micube_toggle](../images/MPD_micube_toggle.png)

Lo scenario sopra riportato, attivato al cambiamento di stato di #[Nessuno][Cube][Pulsante]#, consente di interrompere e riavviare il brano scuotendo il Mi Cube.

# Traduzione

L'interfaccia, i messaggi inviati nei log e la documentazione sono tradotti nelle 5 lingue supportate da Jeedom (grazie a @mips per lo sviluppo di ga-translation e docs-translations). Se si riscontrano errori di traduzione, è possibile aprire una richiesta di assistenza e, se possibile, allegare il file di traduzione corretto (che si trova nella directory core/i18n del plugin).

# Recensioni

![MPD_recensione](../images/MPD_avis.png)

Se ti piace questo plugin, ti invitiamo a lasciare una valutazione e un commento sul Jeedom Market, ci fa sempre piacere: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4464#>

