
<!--  
Ultima modifica: 26/07/2026 18:45:10
-->


# Plugin EcoNetatmo

Plugin che consente di rilevare i consumi dai contatori ecologici Legrand modello Drivia con NetAtmo (codice 41203x).

Questo plugin è stato sviluppato sulla base del plugin standard netatmoWeather.

Questo plugin utilizza le API fornite da Netatmo (vedi il seguente link <https://dev.netatmo.com/apidocumentation/control>).

# Recupero delle credenziali di accesso

Per accedere ai dati del tuo Ecocompteur, devi disporre di un client\_id e di un client\_secret generati sul sito <https://dev.netatmo.com>.

Se non l'hai ancora fatto, crea un account <https://auth.netatmo.com/fr-fr/access/signup?next_url=https%3A%2F%2Fdev.netatmo.com%2Fbusiness-showcase>

![app](../images/apps.png)

Una volta effettuato l'accesso, vai al menu delle applicazioni ( <https://dev.netatmo.com/apps/> ) e clicca su "Crea".

![app](../images/app.png)

Compila il modulo e clicca su "Salva".

![segreto](../images/secret.png)

Il "client ID" e il "client secret" sono stati generati. È possibile utilizzarli per configurare il plugin.


# Recupero dei token

I token consentono l'accesso ai tuoi dati sui server Netatmo (vedi lo standard di autorizzazione OAuth2).

È possibile generarli direttamente nella pagina dell'applicazione.

![genera_token](../images/generate_token.png)

Selezionare lo scope "read_magellan" e fare clic su "Generate Token".

![token](../images/tokens.png)

Dopo aver autorizzato l'accesso ai propri dati, vengono generati i token.

# Configurazione del plugin

Una volta installato il plugin, è necessario attivarlo e inserire le proprie credenziali di accesso a Netatmo:

![configurazione](../images/configuration.png)

-   **ID cliente**: il tuo ID cliente (vedi sezione configurazione)
-   **Client segreto**: il tuo client segreto (vedi sezione configurazione)
-   **Token di accesso**: token che consente l'accesso ai tuoi dati sui server Netatmo
-   **Token di aggiornamento**: token che consente di aggiornare il token di accesso

La gestione dei token è gestita dal plugin. Nel caso in cui questi diventassero non validi (vedere i log), ad esempio dopo un lungo periodo di inattività, sarebbe necessario generarne di nuovi e aggiornare la configurazione del plugin con i nuovi token.

È inoltre possibile specificare se utilizzare un cron autonomo. Ciò consente di evitare che gli altri cron vengano bloccati nel caso in cui il cron del plugin si blocchi e di non essere a propria volta bloccati da altri cron avviati per altri plugin.

![registro](../images/log.png)

È possibile attivare il livello di log "Debug" per monitorare l'attività del plugin e individuare eventuali problemi.

# Configurazione delle apparecchiature

La configurazione dei dispositivi Netatmo è accessibile dal menu del plugin (menu Plugins, Energia, quindi EcoNetAtmo):

![sincronizzazione](../images/synchronisation.png)

Fare clic su Sincronizzazione per avviare la creazione dei dispositivi. L'API /homesdata viene utilizzata per recuperare le informazioni (vedere <https://dev.netatmo.com/apidocumentation/control#homesdata>).

![apparecchiature](../images/equipements.png)

Sono stati creati i contatori delle linee elettriche. C'è un dispositivo per ogni linea.

![apparecchiature](../images/equipement.png)

Nella scheda "Apparecchiature" troverete tutte le impostazioni relative alle vostre apparecchiature:

-   **Nome**: nome del contatore (ripreso dalla configurazione di Netatmo)
-   **Oggetto padre**: indica l'oggetto padre a cui appartiene l'apparecchio
-   **Categoria**: indica la categoria Jeedom dell'apparecchio
-   **Attiva**: consente di attivare il dispositivo
-   **Visibile**: lo rende visibile sulla dashboard
-   **ID modulo**: indica l'identificativo univoco del dispositivo presso Netatmo
-   **Tipo di consumo**: indica il tipo di dispositivo in uso su Netatmo
-   **Tipo di fonte**: indica la fonte di energia delle tue apparecchiature Netatmo
-   **Icona**: consente di selezionare un tipo di icona per il proprio dispositivo nel pannello di controllo
  
Il pulsante "Importa contatori" consente di creare i comandi corrispondenti all'apparecchiatura. Questa operazione viene eseguita al momento della creazione dell'apparecchiatura ed è utile solo se è stato eliminato un comando.

![comandi](../images/comandi.png)

Nella scheda "Comandi" è riportato l'elenco dei comandi (questi vengono generati al momento della creazione dell'apparecchio).

Il comando "Refresh" consente di avviare il recupero immediato dei valori dei contatori. Per impostazione predefinita, il recupero viene avviato ogni 10 minuti.

Gli altri comandi corrispondono ai contatori aggiornati da Netatmo (vedere l'API /getmesure <https://dev.netatmo.com/apidocumentation/control#getmeasure>). Per ciascuno di essi, oltre ai valori standard di Jeedom, sono disponibili:

-   il nome visualizzato sulla dashboard
-   il logicalID corrispondente al "tipo" nell'API di Netatmo
-   la possibilità di attivare o meno la lettura del contatore
-   il periodo corrispondente al parametro "scale" nell'API di Netatmo (per il quale si desidera recuperare i dati; vengono visualizzati solo i valori consentiti dall'API di Netatmo)

# Widget

![widget](../images/widget.png)

Ecco il widget standard.

# Domande frequenti

>**Qual è la frequenza di aggiornamento?**
>
>Il plugin recupera le informazioni ogni 10 minuti. Tuttavia, il contatore ecologico invia le letture circa ogni 3 ore, pertanto è possibile osservare questo sfasamento nel recupero dei dati.

>**Posso ritirare i contatori del gas e dell'acqua?**
>
>Il plugin è in grado di farlo. Purtroppo, l'API di Netatmo non specifica quale "tipo" utilizzare per il recupero di questi valori. È stata inviata una richiesta al team responsabile dello sviluppo dell'API, ma non è ancora stata fornita alcuna risposta.

# Traduzione

L'interfaccia, i messaggi inviati nei log e la documentazione sono tradotti nelle 5 lingue supportate da Jeedom (grazie a @mips per lo sviluppo di ga-translation e docs-translations). Se si riscontrano errori di traduzione, è possibile aprire una richiesta di assistenza e, se possibile, allegare il file di traduzione corretto (che si trova nella directory core/i18n del plugin).

# Recensioni

![EcoNetatmo_recensione](../images/EcoNetatmo_avis.png)

Se ti piace questo plugin, ti invitiamo a lasciare una valutazione e un commento sul Jeedom Market, ci fa sempre piacere: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4413#>
