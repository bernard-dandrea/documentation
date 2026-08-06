
<!--  
Zuletzt geändert: 27.07.2026, 15:27:46
-->


# EcoLegrand-Plugin

Plugin zum Abrufen der Daten von Legrand-Energiezählern der älteren Generation (Artikelnummer 412000).

Im Gegensatz zu den neuen Ökozählern, deren Daten nur über die Cloud abrufbar sind, kann auf die alten Ökozähler über eine lokale Webschnittstelle zugegriffen werden. Insbesondere lässt sich der momentane Verbrauch direkt anzeigen, was bei den neuen Ökozählern nicht möglich ist (dort müssen die Daten direkt am Ökozähler abgelesen werden).

Die Öko-Zähler 412000 werden seit 2020 nicht mehr vertrieben, sind aber im Vergleich zur aktuellen Version nach wie vor sehr interessant.

Die Kommunikation zwischen dem Plugin und dem Stromzähler erfolgt durch das Abrufen von Daten aus benutzerdefinierten JSON-Dateien. Der Benutzer legt in der JSON-Datei selbst fest, welche Daten er abrufen möchte.

Die Grundfunktion des Plugins besteht im Abruf von Daten aus den Stromzählern. Die Auswertung dieser Daten muss auf andere Weise erfolgen (virtuell, über Szenarien usw.) und erfordert gewisse Kenntnisse in Jeedom, um die Daten bearbeiten zu können.

# Installation und Konfiguration des EcoLegrand-Ökozählers

Damit das Plugin ordnungsgemäß funktioniert, muss der Stromzähler betriebsbereit und über die Weboberfläche erreichbar sein.

Das Plugin wurde mit der Version 3.0.17 getestet, der aktuellsten veröffentlichten Version, die nicht mehr weiterentwickelt wird, da dieser Ökostromzähler nicht mehr gepflegt wird.

# Definition der Daten, die in einer JSON-Datei abgerufen werden sollen

Die abzurufenden Daten sind in einer JSON-Datei definiert, die auf den Ökozähler kopiert werden muss.

{   
\"Zähler_C1\":~LG536 2 12724$,
\"Zähler_C2\":~LG536 4 12724$,
\"Zähler_C3\":~LG536 6 12724$,
\"Zähler_C4\":~LG536 8 12724$,
\"Zähler_C5\":~LG536 10 12724$,
„Zähler_EF“:~LG538 0 12.907 $,
„EC-Zähler“:~LG538 1 12907$
}

Die JSON-Datei hat das oben gezeigte Format. Für jeden abzurufenden Datensatz gibt es eine Zeile (achten Sie darauf, in der letzten Zeile kein Komma zu setzen und doppelte Anführungszeichen zu verwenden).

Jede Zeile enthält den Namen der Daten und die im Ökozähler definierte interne Referenz. Die unter dem Link <https://github.com/bernard-dandrea/documentation/blob/main/jeedom-EcoLegrand/doc/fr_FR/JSON_codes.txt> verfügbare Datei enthält eine nicht vollständige Liste der verwendbaren Referenzen.

Weitere Informationen finden Sie im folgenden Forum: <https://easydomoticz.com/forum/viewtopic.php?t=1942&start=20>.

# Kopie der JSON-Datei auf den Ökozähler

Das Kopieren erfolgt über das FTP-Protokoll. Dazu kann das Programm FileZilla verwendet werden.

![FileZilla_Connect](../images/FileZilla_Connect.png)

Melden Sie sich mit der IP-Adresse und den Zugangsdaten an (Standard: admin / password).

![FileZilla_SYS](../images/FileZilla_SYS.png)

Wechseln Sie in das Verzeichnis SYS.

![FileZilla_COPY](../images/FileZilla_COPY.png)

Kopieren Sie die JSON-Datei. Beachten Sie, dass der Dateiname kurz genug sein muss, da das Kopieren sonst nicht funktioniert.

Im Verzeichnis SYS befinden sich die vom Ökozähler verwendeten HTML-Dateien. Wenn Sie diese analysieren, finden Sie die Verweise auf die Variablen, die Sie interessieren.

# Problem mit den Energiezählern

Das obige Forum erklärt sehr gut die Probleme, die bei Energiezählern auftreten (Impulszähler sind davon nicht betroffen).

Es scheint, dass die Software des Öko-Zählers diese Zähler intern mit Variablen vom Typ „float 32“ verwaltet. Diese haben eine Genauigkeit von etwa 7 Dezimalstellen.

Diese Zähler werden jede Sekunde aktualisiert und in kWh mit sechs Dezimalstellen verwaltet.

Wenn man also die 10-kWh-Marke überschreitet, nimmt die Genauigkeit ab, insbesondere wenn auf der betreffenden Leitung nur wenig Strom verbraucht wird. Dies macht sich besonders stark bemerkbar, sobald man die 100-kWh-Marke überschreitet.

Um dieses Problem zu beheben, kann das Plugin die Zähler ab einem vom Benutzer festgelegten Schwellenwert (typischerweise zwischen 1 und 10 kWh) zurücksetzen. Das Plugin verwaltet die Abweichung und liefert einen kumulierten Zählerstand. Beachten Sie, dass dieses Zurücksetzen des internen Zählers die vom Ökozähler bereitgestellten Statistiken beeinträchtigen kann.

# Installation des Plugins

Sobald das Plugin installiert ist, muss es aktiviert werden.


![Konfiguration](../images/configuration.png)

Sie können außerdem festlegen, ob ein eigenständiger Cron-Job verwendet wird. Dadurch werden andere Cron-Jobs nicht blockiert, falls der Cron-Job des Plugins abstürzt, und das Plugin selbst wird nicht durch andere Cron-Jobs blockiert, die für andere Plugins gestartet werden.

Sie können die Debug-Protokollstufe aktivieren, um die Aktivität des Plugins zu verfolgen und eventuelle Probleme zu identifizieren.

# Konfiguration der Geräte

Die Konfiguration der Geräte erfolgt über das Plugin-Menü (Menü „Plugins“, „Energie“ und dann „Ecocompteur Legrand“).

Klicken Sie auf „Hinzufügen“, um einen Stromzähler einzurichten.

![Ausstattung](../images/Equipement.png)

Geben Sie die Konfiguration des Öko-Zählers an:

-   **Name**: Name des Ökostromzählers
-   **Übergeordnetes Objekt**: Gibt das übergeordnete Objekt an, zu dem das Gerät gehört
-   **Kategorie**: Gibt die Jeedom-Kategorie des Geräts an
-   **Aktivieren**: Damit wird das Gerät aktiviert.
-   **Sichtbar**: Macht es auf dem Dashboard sichtbar
-   **IP-Adresse**: IP-Adresse des Geräts
-   **JSON-Datei**: JSON-Datei, die die Definition der abzurufenden Daten enthält

Die folgenden Tasten ermöglichen folgende Funktionen:

-   **Auf den Öko-Zähler zugreifen**: Ermöglicht die Anmeldung über das Web beim Öko-Zähler
-   **JSON testen**: Ermöglicht es, die JSON-Datei zu testen und zu überprüfen, ob die zurückgegebenen Parameter korrekt sind
-   **Zähler erstellen**: Erzeugt die den Zählern entsprechenden Info-Befehle

# Gerätebezogene Steuerungen

![Steuerungen](../images/Commandes.png)

Standardmäßig werden zwei Befehle erstellt:

- Letzte Aktualisierung: Gibt an, wann die letzten Daten des Stromzählers aktualisiert wurden
- Aktualisieren: Ermöglicht das erzwungene Abrufen der Zählerstände. Ein Cron-Job führt die Aktualisierung jede Minute durch.

Für jeden Zähler wird ein Info-Befehl erstellt. Für jeden davon gibt es zusätzlich zu den üblichen Jeedom-Feldern:

- Das Kontrollkästchen „Update“, mit dem festgelegt werden kann, ob eine Aktualisierung des Zählers angefordert werden soll oder nicht
- der Schwellenwert, ab dem der Zähler auf Null zurückgesetzt wird
- Der Befehl, der den Zähler zurücksetzt
- der Offset, d. h. der kumulierte Zählerstand zum Zeitpunkt des Zurücksetzens auf Null
- der aktuelle Zählerstand (Offset + Zählerstand im Ökozähler)

Der Befehl zum Zurücksetzen der Zähler lautet http://192.168.1.xxx/wp.cgi?wp=536+X+12724+-1+-1+4+0.0, d. h. wp.cgi?, gefolgt von den Zählernummern und festen Werten, zum Beispiel wp=536+2+12724+-1+-1+4+0.0 für Zähler_C1. Weitere Informationen finden Sie im Forum <https://easydomoticz.com/forum/viewtopic.php?t=1942&start=120>.

Bei nicht-numerischen Feldern den Feldtyp von „Numerisch“ auf „Sonstiges“ ändern (Schwellenwert und Offset sind in diesem Fall nicht relevant).

# Widget

![Widget](../images/Widget.png)

Hier ist ein Beispiel für ein Widget. Beachten Sie, dass Sie die Einheiten im Befehl selbst angeben müssen.

# Datenauswertung

Mithilfe von Szenarien – ob virtuell oder über PHP-Prozeduren – lassen sich eigene Anzeigen auf Basis der Zählerstände erstellen.

![Leistung](../images/puissance.png)

Beispielsweise kann man einen Leistungswert ermitteln, der auf der Berechnung der Durchschnittsleistung zwischen zwei Messungen basiert.

![conso_jour](../images/conso_jour.png)

Oder tägliche Stromverbrauchsberichte erstellen.

# Häufig gestellte Fragen

Es kann vorkommen, dass die vom Ökozähler zurückgesendete JSON-Datei nicht dekodiert werden kann.

![json_error](../images/json_error.png)

In diesem Fall wird eine Meldung im Protokoll angezeigt.

![json_lint](../images/json_lint.png)

Um die Ursache des Fehlers zu ermitteln, rufen Sie die vom Ökostromzähler zurückgegebenen JSON-Daten aus dem Protokoll ab und testen Sie diese auf der Website <https://jsonlint.com/>.

Der Fehler ist hier darauf zurückzuführen, dass die Konvertierungsroutine die führende 0 in der Eingabe „Linky_Conso“:024795944 nicht akzeptiert.

Dies lässt sich beheben, indem man den Wert 024795944 in Anführungszeichen setzt.

Ändern Sie dazu die Definitionsdatei für die abzurufenden Daten und fügen Sie Anführungszeichen in den entsprechenden Eintrag ein:

„Linky_Conso“:~LG526 1 12005$, --> „Linky_Conso“:„~LG526 1 12005$“,

Die Zeichenfolge „024795944“ wird dann als Zeichenfolge behandelt, und bei der Konvertierung treten keine Probleme mehr auf.

# Bewertungen

![EcoLegrand_Bewertung](../images/EcoLegrand_avis.png)

Wenn Ihnen dieses Plugin gefällt, hinterlassen Sie bitte eine Bewertung und einen Kommentar im Jeedom Market – das freut uns immer: <https://jeedom.com/market/index.php?v=d&p=market_display&id=4430#>
