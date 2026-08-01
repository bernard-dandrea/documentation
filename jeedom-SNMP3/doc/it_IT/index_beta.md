
<!--  
Ultima modifica: 25/07/2026 18:39:50
-->

# Plugin SNMP3

Plugin che consente di leggere e scrivere gli OID dei dispositivi che supportano il protocollo SNMP.

SNMP è uno dei protocolli più diffusi per la gestione e l'analisi dei dispositivi di rete. La maggior parte dei dispositivi di rete di livello professionale è dotata di un agente SNMP integrato.

Il plugin utilizza il pacchetto php-snmp (vedi <https://www.php.net/manual/fr/book.snmp.php>), che è un wrapper della libreria Net-SNMP (vedi <http://www.net-snmp.org>). Il plugin consente di interrogare (comando get) e aggiornare (comando set) gli OID che lo supportano.

# AVVISO

Questo plugin è destinato a chi ha familiarità con il protocollo.

Questo argomento non è particolarmente complicato, ma richiede comunque una buona padronanza dei concetti che ne stanno alla base (autenticazione, OID, MIB, ...).

Prima di contattare lo sviluppatore per eventuali problemi, verificare innanzitutto che le impostazioni per la comunicazione con il dispositivo SNMP siano corrette.

A tal fine, in una sessione SSH è possibile utilizzare, ad esempio, il comando snmpget:

snmpget -v 3 -n "" -u admin_snmp_2024 -a MD5 -A "xxxxxx" -x DES -X "yyyyy" -l authPriv 192.168.1.5 .1.3.6.1.2.1.1.6.0

![SNMP3_snmp_get](../images/SNMP3_snmp_get.png)

# Installazione e configurazione dei dispositivi SNMP

Il corretto funzionamento del plugin presuppone che il protocollo SNMP sia correttamente installato e configurato sul sistema di destinazione. Per effettuare questa configurazione, consultare la documentazione del produttore.

Si consiglia di utilizzare il protocollo v3 per garantire la sicurezza della connessione.

![SNMP3_Synology](../images/SNMP3_Synology.png)

Di seguito è riportato un esempio di configurazione su un NAS Synology.

Verificare le impostazioni di connessione con il comando snmpget (vedere il paragrafo precedente) o altre utilità.

# Configurazione del plugin

Una volta installato il plugin, è necessario attivarlo. Il pacchetto php-snmp viene installato durante l'installazione delle dipendenze.

È possibile attivare il livello di log "Debug" per monitorare l'attività del plugin e individuare eventuali problemi.


![SNMP3_Apparecchiature](../images/SNMP3_cron.png)

È inoltre possibile specificare se utilizzare un cron autonomo. Ciò consente di evitare che gli altri cron vengano bloccati nel caso in cui il cron del plugin si blocchi e di non essere a propria volta bloccati da altri cron avviati per altri plugin.

# Gestione dei MIB

È possibile identificare gli OID tramite il loro codice numerico, ad esempio .1.3.6.1.4.1.6574.1.1.0, oppure utilizzando la MIB corrispondente, ad esempio SYNOLOGY-SYSTEM-MIB::systemStatus.0.

Durante l'installazione del pacchetto php-snmp, vengono installati alcuni MIB (normalmente nella directory /usr/share/snmp/mibs) che possono essere utilizzati direttamente.

Il plugin consente di installare MIB specifiche inserendo i file corrispondenti, ad esempio SYNOLOGY-SYSTEM-MIB.txt, nella directory plugins/SNMP3/data/mibs.

È inoltre possibile copiare i file nella directory comune (in genere /usr/share/snmp/mibs). Si noti che sarà necessario ripetere l'operazione in caso di ripristino di Jeedom.

Se si riscontrano difficoltà nell'implementazione delle MIB, è possibile testarle con il comando snmptranslate (vedere <https://net-snmp.sourceforge.io/tutorial/tutorial-5/commands/snmptranslate.html>). Attenzione: in questo caso, le MIB presenti nella directory plugins/SNMP3/data/mibs non vengono prese in considerazione.

# Configurazione delle apparecchiature

La configurazione delle apparecchiature è accessibile dal menu del plugin (menu Plugins, Oggetti connessi, quindi SNMP3).

Fare clic su Aggiungi per configurare il dispositivo SNMP.

![SNMP3_Apparecchiature](../images/SNMP3_Equipement.png)

Specificare la configurazione del dispositivo SNMP:

-   **Nome**: nome del dispositivo SNMP
-   **Oggetto padre**: indica l'oggetto padre a cui appartiene l'apparecchio
-   **Categoria**: indica la categoria Jeedom dell'apparecchio
-   **Attiva**: consente di attivare l'apparecchio
-   **Versione**: versione di SNMP
-   **localhost**: indirizzo IP del dispositivo
-   **Impostazioni di sicurezza**: vedi <https://www.php.net/manual/fr/snmp.setsecurity.php>
-   **timeout**: durata massima durante la quale si attende una risposta alla richiesta SNMP
-   **tentativi**: numero di volte in cui il comando viene inviato in caso di errore (3 se il campo è vuoto)
-   **Icona**: consente di selezionare un tipo di icona per l'apparecchio nel pannello di configurazione

È possibile personalizzare un'icona specifica aggiungendo l'immagine corrispondente (ad esempio perso1.png per l'icona perso1) nella cartella plugin_info del plugin.

Il pulsante **Verifica connessione a SNMP3** consente di verificare se le impostazioni di connessione sono corrette (ricordarsi di accendere l'apparecchio e salvare la configurazione prima di fare clic sul pulsante).

# Comandi associati alle apparecchiature

![SNMP3_Comandi](../images/SNMP3_Commandes.png)

Per impostazione predefinita, vengono creati due comandi:

- Ultimo aggiornamento: comando info che indica quando sono state aggiornate le ultime informazioni relative al dispositivo SNMP
- Refresh: comando che consente di aggiornare tutti gli OID per i quali è attivata l'opzione di aggiornamento

Sono disponibili i seguenti pulsanti:

- Importa un OID: consente di creare un comando informativo per un OID
- Aggiungi un comando di aggiornamento: consente di creare un comando di azione per forzare il recupero del valore dell'OID
- Aggiungi un'azione: consente di creare un comando di azione per modificare il valore dell'OID (quando consentito dal dispositivo SNMP)

# Analisi dei campi dell'ordine

Per ogni comando relativo a un OID, oltre ai campi standard di Jeedom sono presenti:

- LogicalID:
  - per i comandi di tipo info, pari all'OID
  - per i comandi di aggiornamento, pari a 'R_' seguito dall'OID
  - per i comandi di azione, pari a 'A_' seguito dall'OID
- la casella di selezione che consente di richiedere o meno l'aggiornamento dell'OID
- il campo "scan" che indica la frequenza di aggiornamento dell'OID

Per i comandi che consentono l'aggiornamento dell'OID, il sottotipo del comando di azione determina il formato del valore trasmesso al dispositivo SNMP. Quando il sottotipo è "Message", il titolo indica il formato e il contenuto del messaggio fornisce il valore (viene trasmessa solo la prima riga). Consultare <https://www.php.net/manual/fr/function.snmpset.php> per conoscere i formati supportati.

# Widget

![SNMP3_Widget](../images/SNMP3_Widget.png)

Ecco un esempio di widget. È possibile modificare il nome dei comandi per renderli più intuitivi.

# Recensioni

![SNMP3_recensione](../images/SNMP3_avis.png)

Se ti piace questo plugin, ti invitiamo a lasciare una valutazione e un commento sul Jeedom Market, ci fa sempre piacere: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4484#>
