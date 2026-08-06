
<!--  
Ultima modifica: 27/07/2026 15:27:46
-->


# Plugin EcoLegrand

Plugin che consente di recuperare i dati dai contatori ecologici Legrand di vecchia generazione (codice 412000).

A differenza dei nuovi contatori intelligenti, i cui dati sono accessibili solo tramite il cloud, i vecchi contatori intelligenti sono accessibili tramite un'interfaccia web locale. In particolare, è possibile visualizzare direttamente il consumo istantaneo, cosa che non è possibile con i nuovi contatori intelligenti (è necessario visualizzare i dati direttamente sul contatore stesso).

I contatori ecologici 412000 non sono più in commercio dal 2020, ma mantengono tutto il loro interesse rispetto alla versione attuale.

La comunicazione tra il plugin e il contatore ecologico avviene tramite il recupero dei dati da file JSON definiti dall'utente. È l'utente stesso a definire nel file JSON i dati che desidera recuperare.

La funzione di base del plugin è il recupero dei dati dai contatori intelligenti. La loro elaborazione deve avvenire tramite altri mezzi (virtuali, scenari, ecc.) e richiede una certa padronanza di Jeedom per poter gestire i dati.

# Installazione e configurazione del contatore ecologico EcoLegrand

Il corretto funzionamento del plugin presuppone che l'ecocontatore sia operativo e accessibile tramite l'interfaccia web.

Il plugin è stato testato con la versione 3.0.17, che è l'ultima pubblicata e non subirà ulteriori aggiornamenti, dato che questo contatore ecologico non è più supportato.

# Definizione dei dati da recuperare in un file JSON

I dati da recuperare sono definiti in un file JSON che deve essere copiato sull'ecocontatore.

{   
"contatore_C1":~LG536 2 12724$,
"contatore_C2":~LG536 4 12724$,
"contatore_C3":~LG536 6 12724$,
"contatore_C4":~LG536 8 12724$,
"contatore_C5":~LG536 10 12724$,
"Contatore_EF":~LG538 0 12907$,
"Contatore_EC":~LG538 1 12907$
}

Il file JSON ha il formato sopra indicato. C'è una riga per ogni dato da recuperare (attenzione a non inserire una virgola nell'ultima riga e a utilizzare le virgolette doppie).

Ogni riga contiene il nome del dato e il codice interno definito nell'ecocontatore. Il file al link <https://github.com/bernard-dandrea/documentation/blob/main/jeedom-EcoLegrand/doc/fr_FR/JSON_codes.txt> fornisce un elenco non esaustivo dei codici utilizzabili.

Per ulteriori informazioni, potete consultare il seguente forum <https://easydomoticz.com/forum/viewtopic.php?t=1942&start=20>.

# Copia del file JSON sul contatore ecologico

La copia avviene tramite il protocollo FTP. È possibile utilizzare il programma FileZilla.

![FileZilla_Connect](../images/FileZilla_Connect.png)

Accedere indicando l'indirizzo IP e le credenziali di accesso (per impostazione predefinita: admin / password).

![FileZilla_SYS](../images/FileZilla_SYS.png)

Accedere alla directory SYS.

![FileZilla_COPY](../images/FileZilla_COPY.png)

Copia il file JSON. Tieni presente che il nome deve essere abbastanza breve, altrimenti la copia non va a buon fine.

Nella directory SYS si trovano i file HTML utilizzati dall'ecocontatore. Analizzandoli, è possibile individuare i riferimenti alle variabili di vostro interesse.

# Problema con i contatori di energia

Il forum qui sopra spiega molto bene il problema riscontrato con i contatori di energia (i contatori a impulsi non sono interessati).

Sembra che il software dell'ecocontatore gestisca internamente questi contatori con variabili di tipo float a 32 bit. Queste hanno una precisione di circa 7 cifre decimali.

Questi contatori vengono aggiornati ogni secondo e gestiti in kWh con 6 cifre decimali.

Di conseguenza, quando si superano i 10 kWh, si inizia a perdere precisione, soprattutto se il consumo sulla linea in questione è basso. Ciò diventa molto penalizzante quando si superano i 100 kWh.

Per risolvere questo problema, il plugin può azzerare i contatori a partire da una soglia definita dall'utente (in genere da 1 a 10 kWh). Il plugin gestisce lo scostamento e fornisce un valore cumulativo del contatore. Si noti che l'azzeramento del contatore interno può alterare le statistiche fornite dall'ecocontatore.

# Installazione del plugin

Una volta installato il plugin, è necessario attivarlo.


![Configurazione](../images/configuration.png)

È inoltre possibile specificare se utilizzare un cron autonomo. Ciò consente di evitare che gli altri cron vengano bloccati nel caso in cui il cron del plugin si blocchi e di non essere a propria volta bloccati da altri cron avviati per altri plugin.

È possibile attivare il livello di log "Debug" per monitorare l'attività del plugin e individuare eventuali problemi.

# Configurazione delle apparecchiature

La configurazione delle apparecchiature è accessibile dal menu del plugin (menu Plugins, Energia, quindi Ecocompteur Legrand).

Fare clic su Aggiungi per configurare un contatore ecologico.

![Apparecchiature](../images/Equipement.png)

Indicare la configurazione dell'ecocontatore:

-   **Nome**: nome del contatore ecologico
-   **Oggetto padre**: indica l'oggetto padre a cui appartiene l'apparecchio
-   **Categoria**: indica la categoria Jeedom dell'apparecchio
-   **Attiva**: consente di attivare l'apparecchio
-   **Visibile**: lo rende visibile sulla dashboard
-   **Indirizzo IP**: IP del dispositivo
-   **File JSON**: file JSON contenente la definizione dei dati da recuperare

I seguenti pulsanti consentono di eseguire le seguenti funzioni:

-   **Accedere all'ecocontatore**: consente di effettuare l'accesso via web all'ecocontatore
-   **Verifica del JSON**: consente di verificare il file JSON e controllare che i parametri restituiti siano corretti
-   **Creare i contatori**: genera i comandi info corrispondenti ai contatori

# Comandi associati alle apparecchiature

![Comandi](../images/Commandes.png)

Per impostazione predefinita, vengono creati due comandi:

- Ultimo aggiornamento: indica quando sono state aggiornate le ultime informazioni del contatore ecologico
- Aggiornamento: consente di forzare il recupero dei contatori. Un cron avvia l'aggiornamento ogni minuto.

Per ciascuno dei contatori viene creato un comando info. Per ciascuno di essi, oltre ai campi standard di Jeedom, sono presenti:

- l'opzione di aggiornamento che consente di scegliere se richiedere o meno l'aggiornamento del contatore
- la soglia, ovvero il valore a partire dal quale il contatore viene azzerato
- il comando che azzera il contatore
- l'offset, ovvero il valore cumulativo del contatore al momento dell'azzeramento
- il valore attuale del contatore (offset + valore del contatore nell'ecocontatore)

Il comando che consente di azzerare i contatori è del tipo http://192.168.1.xxx/wp.cgi?wp=536+X+12724+-1+-1+4+0.0 ovvero wp.cgi? seguito dai riferimenti dei contatori e dai valori fissi, ad esempio wp=536+2+12724+-1+-1+4+0.0 per il contatore_C1. Per ulteriori informazioni, consultare il forum <https://easydomoticz.com/forum/viewtopic.php?t=1942&start=120>.

Per i campi non numerici, modificare il tipo di campo da Numerico ad Altro (in questo caso la soglia e l'offset non hanno senso).

# Widget

![Widget](../images/Widget.png)

Ecco un esempio di widget. Si noti che è necessario specificare autonomamente le unità di misura nel comando.

# Analisi dei dati

Tramite scenari, virtuali o procedure PHP, è possibile generare i propri indicatori basati sui contatori.

![potenza](../images/puissance.png)

Ad esempio, è possibile generare un rapporto sulla potenza basato sul calcolo della potenza media tra due misurazioni.

![consumo_giornaliero](../images/conso_jour.png)

Oppure generare resoconti giornalieri sul consumo elettrico.

# Domande frequenti

Può capitare che il file JSON restituito dal contatore ecologico non possa essere decodificato.

![json_error](../images/json_error.png)

In questo caso, viene visualizzato un messaggio nel log.

![json_lint](../images/json_lint.png)

Per individuare l'origine dell'errore, recuperare dal log il file JSON restituito dal contatore ecologico e testarlo sul sito <https://jsonlint.com/>.

In questo caso, l'errore è dovuto al fatto che la routine di conversione non accetta lo 0 iniziale nell'input "Linky_Conso":024795944.

È possibile correggere questo errore racchiudendo il valore 024795944 tra virgolette.

A tal fine, modificare il file di definizione dei dati da recuperare e aggiungere le virgolette nella voce corrispondente:

"Linky_Conso":~LG526 1 12005$, --> "Linky_Conso":"~LG526 1 12005$",

La stringa "024795944" verrà quindi considerata come una stringa e non ci saranno più problemi durante la conversione.

# Recensioni

![EcoLegrand_recensioni](../images/EcoLegrand_avis.png)

Se ti piace questo plugin, ti invitiamo a lasciare una valutazione e un commento sul Jeedom Market, ci fa sempre piacere: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4430#>
