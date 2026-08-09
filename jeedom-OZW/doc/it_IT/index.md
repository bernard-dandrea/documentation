<!--  
Ultima modifica: 28/07/2026 15:35:23
-->


# Plugin OZW

Plugin che consente l'interfacciamento con le centrali di comunicazione SIEMENS di tipo OZW.

Le centrali di comunicazione OZW vengono utilizzate per comunicare con le schede che controllano numerose caldaie, pompe di calore e altri dispositivi industriali. Queste dispongono di un server web integrato dal quale è possibile controllare i dispositivi ad esse collegati.

Esistono due modelli con un funzionamento pressoché identico:

- OZW672 per la comunicazione con i dispositivi direttamente sul bus LPB, BSB
- OZW772 per la comunicazione con i dispositivi tramite il protocollo KNX

La comunicazione tra il plugin e l'OZW avviene tramite le API web fornite da SIEMENS, che consentono di simulare le interazioni normalmente effettuate sul server web.

Questo plugin rappresenta un'importante evoluzione del plugin OZW672 (vedi https://github.com/NextDom/plugin-ozw672), che non viene più aggiornato e non funziona nella versione attuale di Jeedom.

# Installazione e configurazione del controller OZW

Per l'installazione della centrale di comunicazione WEB, fare riferimento alla documentazione SIEMENS corrispondente.

![OZW_WEB_ACCESS](../images/OZW_WEB_ACCESS.png)

Abilitare l'accesso alle API web (menu Home > 0.5 OZWx72.01 > Impostazioni > Comunicazione > Servizi).

Il plugin è stato testato con la versione 12 del server web. A priori, il plugin dovrebbe funzionare anche con le versioni precedenti, poiché le chiamate alle API sono piuttosto semplici e dovrebbero essere presenti già da diverse versioni.

![OZW_home](../images/OZW_accueil.png)

Una volta completata l'installazione, dovrebbe apparire una pagina web simile a questa.

In questa configurazione sono presenti 2 dispositivi:

-   il primo rappresenta una scheda LMS14 che controlla una caldaia
-   il secondo rappresenta la centrale di comunicazione OWZ672 e ne consente la configurazione

![OZW_dispositivo](../images/OZW_device.png)

È possibile accedere ai vari dati definiti per la mappa. È possibile visualizzarli ed eventualmente modificarli.

Nelle API fornite da SIEMENS, i datapoint devono essere specificati tramite il loro riferimento WEB, che è possibile trovare nell'interfaccia WEB.

![OZW_datapoint_reference](../images/OZW_datapoint_reference.png)

Per individuarla, posizionarsi sulla riga corrispondente e avviare l'ispezione dell'elemento (in genere clic destro e poi "Ispeziona"). Nel codice corrispondente, si trova un numero nell'istruzione 'openDialog('xxx')' o 'id='dpxxx'' che indica il riferimento WEB, 591 nell'esempio sopra riportato.

![OZW_ID_menu](../images/OZW_ID_menu.png)

Allo stesso modo, potrebbe essere necessario l'ID di un menu, che si trova allo stesso modo: 590 nell'esempio sopra riportato.

# Configurazione del plugin

Una volta installato il plugin, è necessario attivarlo.

![Configurazione](../images/OZW_configuration.png)

È inoltre possibile specificare se utilizzare un cron autonomo. Ciò consente di evitare che gli altri cron vengano bloccati nel caso in cui il cron del plugin si blocchi e di non essere bloccati a loro volta da altri cron avviati per altri plugin.

È possibile attivare il livello di log "Debug" per monitorare l'attività del plugin e individuare eventuali problemi.

# Configurazione delle apparecchiature

La configurazione dei dispositivi è accessibile dal menu del plugin (menu Plugins, Oggetti connessi, quindi OZW).

Fare clic su Aggiungi per definire l'OZW.

![OZW_Attrezzature_OZW](../images/OZW_Equipement_OZW.png)

Indicare la configurazione dell'OZW:

-   **Nome**: nome dell'OZW
-   **Oggetto padre**: indica l'oggetto padre a cui appartiene l'apparecchio
-   **Categoria**: indica la categoria Jeedom dell'apparecchio
-   **Attiva**: consente di attivare l'apparecchio
-   **Visibile**: lo rende visibile sulla dashboard
-   **Indirizzo IP**: IP del dispositivo
-   **Account e password**: codici di accesso al server web
-   **Durata di una sessione**: periodo trascorso il quale l'ID della sessione viene rinnovato
-   **Icona**: consente di selezionare un tipo di icona per l'apparecchio nel pannello di configurazione

Dopo aver salvato l'OZW, sono attivi i seguenti pulsanti:

-   **Accedi all'OZW**: consente di effettuare l'accesso via web all'OZW
-   **Importa i dispositivi**: consente di importare le apparecchiature corrispondenti ai dispositivi collegati all'OZW.

![OZW_Apparecchiature_OZW_dispositivi](../images/OZW_Equipement_OZW_devices.png)

Nell'esempio sopra riportato, dopo l'importazione dei dispositivi si trova:

- l'OZW672 come dispositivo principale
- l'OZW672.01 come dispositivo
- la scheda LMS14 che gestisce la caldaia

![OZW_Apparecchiature_OZW_dispositivo](../images/OZW_Equipement_OZW_device.png)

È possibile associare un'icona specifica al dispositivo. È inoltre possibile personalizzare un'icona di tipo "perso" aggiungendo l'immagine corrispondente (ad esempio perso1.png per l'icona perso1) nella cartella plugin_info del plugin.

# Comandi associati alle apparecchiature

![OZW_Comandi](../images/OZW_Commandes.png)

Per l'OZW vengono creati 2 comandi di tipo info:

- Stato: pari a 1 quando la comunicazione con il server WEB è stabilita, 0 in caso contrario
- SessionID: ID utilizzato dalle API web

![OZW_Comandi_device_initial](../images/OZW_Commandes_device_initial.png)

Per i dispositivi collegati all'OZW, vengono creati 2 comandi:

- Ultimo aggiornamento: comando di tipo "info" che indica quando è stata aggiornata l'ultima informazione relativa al dispositivo
- Refresh: comando di tipo azione che consente di aggiornare tutti i datapoint per i quali è attivata l'opzione di aggiornamento

![OZW_Importa_Menu_principale](../images/OZW_Importer_Menu_principal.png)

Il pulsante "Importa comandi principali" nella scheda "Apparecchiature" consente di importare tutti i datapoint dal menu denominato "mobile". Quest'ultimo è disponibile nell'applicazione Android fornita da SIEMENS e non è disponibile per tutti i dispositivi. La creazione dei comandi può richiedere diversi minuti. Una volta completata l'operazione, i principali datapoint del dispositivo vengono definiti come comandi di tipo "info".

![OZW_import_menu_specifico](../images/OZW_import_menu_specifique.png)

Allo stesso modo, il pulsante "Importa menu" nella scheda "Apparecchiature" consente di importare tutti i dati di un menu specifico. A tal fine, è necessario fornire il riferimento web del menu.


![OZW_pulsanti_importazione_ordine](../images/OZW_boutons_import_commande.png)

Nella scheda "Comandi" sono disponibili i seguenti pulsanti:

- Importa un datapoint: consente di creare un comando informativo per un datapoint specifico
- Aggiungi un'azione: consente di modificare il valore del datapoint (quando consentito dal server web)
- Aggiungi un comando di aggiornamento: consente di forzare il recupero del valore del datapoint

**Attenzione**: inserire il codice WEB del datapoint e non il numero di riga visualizzato sulla riga del datapoint.

# Analisi dei campi dell'ordine

![OWZ_Analisi_ordine](../images/OWZ_Analyse_commande.png)

Per ogni comando relativo a un datapoint, oltre ai campi standard di Jeedom sono presenti:

- LogicalID:
  - per i comandi di tipo info, pari al riferimento WEB del datapoint
  - per i comandi di azione, pari a 'A_' seguito dal riferimento WEB del datapoint
  - per i comandi di aggiornamento, pari a 'R_' seguito dal riferimento WEB del datapoint
- il flag "update" che consente di richiedere o meno l'aggiornamento del datapoint
- il campo "scan" che indica la frequenza di aggiornamento del datapoint

# Widget

![OZW_widget](../images/OZW_widget.png)

Ecco un esempio di widget. È possibile modificare il nome dei comandi in modo che corrisponda al numero di riga indicato nel server web.

# Traduzione

L'interfaccia, i messaggi inviati nei log e la documentazione sono tradotti nelle 5 lingue supportate da Jeedom (grazie a @mips per lo sviluppo di ga-translation e docs-translations). Se si riscontrano errori di traduzione, è possibile aprire una richiesta di assistenza e, se possibile, allegare il file di traduzione corretto (che si trova nella directory core/i18n del plugin).

# Recensioni

![OZW_recensione](../images/OZW_avis.png)

Se ti piace questo plugin, ti invitiamo a lasciare una valutazione e un commento sul Jeedom Market, ci fa sempre piacere: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4414#>
