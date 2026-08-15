
<!--  
Ultima modifica: 28/07/2026 16:00:31
-->

# Plugin BSBLAN

Plugin che consente l'interfacciamento con il controller BSB-LPB-LAN.

Il controller BSB-LPB-LAN nasce da un progetto finalizzato alla comunicazione con le schede SIEMENS che controllano numerose caldaie, pompe di calore e altri dispositivi industriali.

La documentazione è molto completa ed è disponibile all'indirizzo <https://docs.bsb-lan.de>. L'hardware può essere acquistato presso Frederik Holst <bsb@code-it.de>.

Il BSB-LAN può sostituire vantaggiosamente i controller OZW forniti da Siemens. La soluzione è molto più economica, consente l'accesso a tutti i parametri delle schede Siemens (a differenza dell'OZW) e i tempi di accesso alle schede sono molto più rapidi. Inoltre, è possibile inviare la temperatura delle zone riscaldate senza dover ricorrere a un sensore ambiente.

La comunicazione tra il plugin e BSBLAN avviene tramite API web.

# Installazione e configurazione del controller BSBLAN

Il corretto funzionamento del plugin presuppone che il modulo BSB-LAN sia operativo.

Per l'installazione e la configurazione, consultare l'ottima documentazione disponibile sul sito del progetto.

Se si desidera modificare alcune impostazioni, sarà necessario abilitarle nella configurazione di BSBLAN.

Il plugin è stato testato con le versioni 3.2 e 4.2. A prima vista, il plugin dovrebbe funzionare anche con le versioni precedenti, poiché le chiamate alle API sono piuttosto semplici e dovrebbero essere presenti già da diverse versioni.

# Configurazione del plugin

Una volta installato il plugin, è necessario attivarlo.

![Configurazione](../images/BSBLAN_configuration.png)

È inoltre possibile specificare se utilizzare un cron autonomo. Ciò consente di evitare che gli altri cron vengano bloccati nel caso in cui il cron del plugin si blocchi e di non essere a propria volta bloccati da altri cron avviati per altri plugin.

È possibile attivare il livello di log "Debug" per monitorare l'attività del plugin e individuare eventuali problemi.

# Configurazione delle apparecchiature

La configurazione dei dispositivi è accessibile dal menu del plugin (menu Plugins, Oggetti connessi, quindi BSBLAN).

Fare clic su Aggiungi per configurare il controller BSBLAN.

![BSBLAN_Attrezzature](../images/BSBLAN_Equipement.png)

Specificare la configurazione del BSBLAN:

-   **Nome**: nome del BSBLAN
-   **Oggetto padre**: indica l'oggetto padre a cui appartiene l'apparecchio
-   **Categoria**: indica la categoria Jeedom dell'apparecchio
-   **Attiva**: consente di attivare l'apparecchio
-   **Visibile**: lo rende visibile sulla dashboard
-   **Indirizzo IP**: IP del dispositivo
-   **Account e password**: codici di accesso al server web
-   **Passkey**: prefisso da fornire alle richieste HTML (vedere la documentazione BSBLAN)
-   **Timeout**: tempo massimo di attesa per una risposta alla richiesta HTTP (15 secondi se il campo è vuoto)
-   **Aggiornamenti**: metodo utilizzato per eseguire gli aggiornamenti tramite JSON o un comando diretto nell'URL. In alcuni casi, si è riscontrato che gli aggiornamenti tramite JSON non venivano eseguiti. Non è stato possibile comprenderne il motivo. In questo caso, è possibile utilizzare l'opzione tramite comando /S, che funziona in ogni circostanza. Tuttavia, nella versione 3 di BSBLAN, alcuni comandi che richiedono la specificazione del flag INFO (ad esempio l'invio della temperatura ambiente) non vengono presi in considerazione.
-   **Numero di tentativi**: numero di volte in cui il comando viene inviato in caso di errore (3 se il campo è vuoto)
-   **Icona**: consente di selezionare un tipo di icona per l'apparecchio nel pannello di configurazione

È possibile associare un'icona specifica al BSBLAN. È inoltre possibile personalizzare un'icona di tipo "perso" aggiungendo l'immagine corrispondente (ad esempio perso1.png per l'icona perso1) nella directory plugin_info del plugin.

I seguenti pulsanti consentono di eseguire le seguenti funzioni:

-   **Accedere a BSBLAN**: consente di effettuare l'accesso via web a BSBLAN
-   **Verifica della connessione a BSBLAN**:  consente di verificare se le impostazioni di connessione sono corrette (ricordarsi di salvare la configurazione prima di fare clic sul pulsante). Viene visualizzato il numero di versione di BSBLAN.

# Comandi associati alle apparecchiature

![BSBLAN_Comandi](../images/BSBLAN_Commandes.png)

Per impostazione predefinita, vengono creati due comandi:

- Ultimo aggiornamento: comando che indica quando sono state aggiornate le ultime informazioni del BSBLAN
- Refresh: comando che consente di aggiornare tutti i parametri per i quali è attivata la funzione di aggiornamento

Sono disponibili i seguenti pulsanti:

- Importa un parametro: consente di creare un comando informativo per un parametro specifico
- Aggiungi un comando di aggiornamento: consente di forzare il recupero del valore del parametro
- Aggiungi un comando di azione: consente di modificare il valore del parametro (se consentito dal server web)

# Analisi dei campi dell'ordine

Per ogni comando relativo a un parametro, oltre ai campi standard di Jeedom sono presenti:

- LogicalID:
  - per i comandi di tipo info, pari al codice del parametro
  - per i comandi di azione, pari a 'A_' seguito dal codice del parametro
  - per i comandi di aggiornamento, pari a 'R_' seguito dal codice del parametro
- il flag di aggiornamento che consente di richiedere o meno l'aggiornamento del parametro
- per i comandi info, il campo scan che indica la frequenza di aggiornamento del parametro
- per i comandi di azione, il campo AGGIORNAMENTO che consente di specificare una modalità di aggiornamento specifica

# Widget

![BSBLAN_Widget](../images/BSBLAN_Widget.png)

Ecco un esempio di widget. È possibile modificare il nome dei comandi per renderli più intuitivi.

# Recensioni

![BSBLAN_recensione](../images/BSBLAN_avis.png)

Se ti piace questo plugin, ti invitiamo a lasciare una valutazione e un commento sul Jeedom Market, ci fa sempre piacere: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4424#>
