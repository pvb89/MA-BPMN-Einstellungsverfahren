**Dokumentation - Bewerbungsverfahren - Camunda**

****

# 1) Einleitung


In Hinblick auf den demografischen Wandel sowie dem damit verbundenen War for
Talents wird es heutzutage für Unternehmen zunehmend wichtiger, ihre neuen
potentielle Mitarbeiter gut überlegt einzustellen und natürlich die
Wissensträger und Top- Mitarbeiter zu gewinnen.

Im Rahmen des Kurses „Implementierung von Prozessen“ im Sommersemester 2019
haben wir uns für einen Bewerbereinstellungsprozess als Thema unseres
Hauptprojektes entschieden. Den Prozess galt es als BPMN-Modell, inklusive DMN,
zu modellieren sowie anschließenden technisch zu implementieren.

Dabei gab es folgende technische Anforderungen an das BPMN-Modell:

-   mindestens 8 Aktivitäten, davon zwei Decision Tasks
-   4 verschiedene Aktivitätstypen
-   eine Call Activity
-   zwei verschiedene Gateways
-   ein Throw Catch Pattern
-   drei verschiedene Event-Spezifikationen
-   technische Anforderungen an DMN in Summe wie im Seminar

# 2) Fachliches Modell

Der BPMN-Prozess zur Bewerbereinstellung besteht aus einem Pool (Unternehmen),
indem der Abteilungsleiter agiert sowie einem zugeklappten Pool (Bewerber).
Insgesamt umfasst der Prozess zehn Aktivitäten und sieben Ereignisse
verschiedener Ausführungen.

Der Prozess beginnt, indem bei einem Unternehmen, genauer gesagt bei dem
Abteilungsleiter, eine Bewerbung eingeht, die ein Bewerber gesendet hat. Nach
dem Nachrichten-Startereignis folgt eine Benutzer-Aufgabe, bei der der
Abteilungsleiter die Bewerbungen sammelt, bis er 50 Stück hat. Anschließend
läuft der Prozess mit einem aufgeklappten Teilprozess weiter. Da es jedoch auch
möglich ist, dass bei gewissen Stellenausschreibungen nicht bis zu 50
Bewerbungen eingehen, ist an der Benutzer- Aufgabe ein unterbrechendes Timer-
Zwischenereignis angeheftet. Falls also nicht 50 Bewerbungen zusammenkommen,
läuft der Prozess nach drei Monaten weiter. Es folgt eine weitere
Benutzer-Aufgabe, in der der Abteilungsleiter die Anzahl der eingegangenen
Bewerbungen einträgt, da dies im weiteren Verlauf benötigt wird. Anschließend
findet innerhalb eines aufgeklappten Teilprozesses mit sequentieller multipler
Instanziierung eine Vorauswahl der Bewerber statt. Dabei startet der Prozess
durch ein leeres Startereignis, indem die Bewerbungen eingesammelt wurden. Es
folgt eine Benutzer-Aufgabe, wo die Bewerberdaten durch den Abteilungsleiter
eingegeben werden. Danach kommt eine Geschäftsregelaufgabe, bei der die
Vorauswahl der Bewerber ausgeführt wird. Dies stellt unsere erste Decision Task
dar.

Nachdem ein Ergebnis der Vorauswahl feststeht, werden abschließend in einer
Script Task die Bewerberdaten gespeichert, bevor der aufgeklappte Teilprozess
der Vorauswahl abgeschlossen ist. Da der Abteilungsleiter nun schon einige
potenzielle Mitarbeiter vorausgewählt hat, ist der nächste Schritt einen
Gesprächstermin für ein Bewerberinterview zu finden. Daher stellt die nächste
Aktivität eine Call-Activity dar, die ebenfalls sequentiell multiple
instanziierend gekennzeichnet ist, da für jeden Bewerber, der die Vorauswahl
bestanden hat, ein Termin gefunden werden muss.

![](https://abload.de/img/c352522621dfd50609c195pkh9.png) 

Die Call-Activity der Terminfindung beginnt mit einem leeren Startereignis,
indem die Vorauswahl der Bewerber abgeschlossen ist. Der Mitarbeiter aus dem
Bewerbermanagement vereinbart folgend in einer *„manuel task“* Gesprächstermine
mit dem Bewerber, der wieder als zugeklappter Pool dargestellt wird. Nachdem ein
Termin vereinbart wurde, wird dieser Termin eingegeben durch den Mitarbeiter des
Bewerbermanagements. Es folgt eine *„service task“*, bei der die Gesprächstermine
in den Kalender eingetragen werden. Anschließend erfolgt durch ein exklusives
Gateway die Überprüfung, ob ein Konflikt bei der Eintragung des Termins
aufgetreten ist. Falls ja, wird durch eine User Task durch den Mitarbeiter ein
manueller Eintrag gesetzt. Im weiteren Prozessschritt folgt ein paralleles
Gateway, bei dem zwei Service Tasks gleichzeitig ablaufen, zum einen wird der
Gesprächstermin zur Bestätigung an den Bewerber versendet und zum anderen zur
Bestätigung an den Abteilungsleiter. Der Prozess der Call Activity endet durch
ein leeres Endereignis, indem ein Gesprächstermin gefunden wurde.

![](https://abload.de/img/bd9b9a516331b4a75f5fcsnk2e.png)

Nach der Call Activity folgt im Prozess der Bewerbereinstellung erneut ein
aufgeklappter Teilprozess, indem die Endauswahl für einen Bewerber stattfindet.
Dieser ist wieder durch eine sequentielle multiple Instanziierung
gekennzeichnet, da er für jeden Bewerber ausgeführt wird, der es in die
Endauswahl geschafft hat. Dabei startet der Teilprozess durch ein leeres
Startereignis, nämlich nachdem die Gesprächstermine gefunden wurden. Der
Abteilungsleiter führt nun in einer Manuel Task sozusagen manuell die
Bewerbungsinterviews, bevor er folglich Daten zu den Gesprächen der Bewerber
eingibt, was durch eine User Task modelliert wurde. Es folgt unsere zweite
Decision Task, in der die Entscheidung für einen Bewerber getroffen wird.

Der Abteilungsleiter erhält danach in einer User Task das Ergebnis der
Entscheidung und der Teilprozess der Endauswahl endet mit einem leere
Endereignis, indem ein Bewerber ausgewählt wurde.

Schließlich endet auch der gesamte Prozess mit einem leeren Endereignis, da ein
Bewerber eingestellt ist.

![](https://abload.de/img/adf3cf0a5619e1a2bcb448qjvl.png)

**DRD und Entscheidungstabellen**

Das DMN-Diagramm der Vorauswahl beinhaltet zwei Decision Tables, vier Inputs,
ein Business Knowledge Model und eine Knowlegde Source.

Das Decision Requirement Diagram (DRD) der Vorauswahl beinhaltet eine
Hauptdecision *„Vorauswahl des Bewerbers“* sowie eine Subdecisions. Außerdem
werden vier Inputs, eine Knowledge Source und ein Business Knowledge Model in
dem DRD modelliert.

Zunächst werden über die Subdecision *„Interne Anforderungen an Bewerber“* anhand
der Inputs *„Abgeschlossenes Studium?“*, „Notendurchschnitt“ sowie *„Jahre
Berufserfahrung“* überprüft, ob der Bewerber die internen Richtlinien des
Unternehmens zu Bewerbungen (Business Knowledge Model), die durch den Leiter des
Personalmanagements (Knowledge Source) festgelegt werden können, erfüllt.

Die verwendeten Datentypen bei den Inputfeldern sind einmal *„boolean“* und
zweimal *„string“*. Die hierbei angewendete Hit Policy ist Unique (U), wobei nur
eine Zeile bzw. nur eine Regel zutreffen darf. Falls der Bewerber kein
abgeschlossenes Studium besitzt, erfüllt er die Anforderungen bereits nicht
mehr. Hat der Bewerber ein abgeschlossenes Studium und ist der Notendurchschnitt
schlechter als 3,0 (*„c“*), ist der Bewerber ebenfalls raus. Ist der
Notendurchschnitt zwischen 2,0 und 3,0 (*„b“*), erfüllt er die Anforderungen auch
nicht, es sei denn er weist Berufserfahrung von genau oder mehr als zwei Jahren
(*„b“*) auf. Bewerber, die ein abgeschlossenes Studium und einen Notendurchschnitt
haben, der besser als 2,0 (*„a“*) ist, erfüllen die internen Anforderungen und
bestehen die Vorauswahl. Der Datentyp des Outputs ist ebenfalls boolean, ob die
Anforderungen erfüllt sind oder nicht.

Ob diese erfüllt sind oder nicht, bildet den Input für die Hauptdecision
*„Vorauswahl des Bewerbers“* zusätzlich zu dem Input über die Gehaltsvorstellung,
da oftmals manche Bewerber aufgrund ihrer zu hohen Gehaltsvorstellung
ausgeschlossen werden. Die Gehaltsvorstellung ist vom Datentyp long. Sind die
Anforderungen erfüllt und genau oder weniger als 80.000€ vorgestellt, wird der
Bewerber zum Gespräch eingeladen, falls seine Vorstellungen mehr als 80.000€
entsprechen, wird er nicht eingeladen. Sind die Anforderungen sowieso nicht
erfüllt, spielt auch das Gehalt keine Rolle und der Bewerber wird nicht
eingeladen.

![](https://abload.de/img/6253d67703ea83642cc8dxpjqu.png)

Das zweite DRD, die Endauswahl für einen Bewerber beinhaltet ebenfalls eine
Hauptdecision *„Entscheidung für einen Bewerber treffen“* sowie eine Subdecisions.
Die Subdecision *„Score Bewerber“*, ergibt sich aus den drei Inputs *„Ehrenamt“*,
*„Fachliche Eignung“* sowie *„Aufgabe aus dem Vorstellungsgespräch“*. Der
angewendete Hit Policy ist Collect mit der Aggregation SUM (C+). Hierbei werden
für die Bewerber jeweils ein Score ermittelt. Wenn sie ein Ehrenamt nachweisen,
erhalten sie +10 Punkte, falls nicht -10 Punkte. Da oftmals in einem
Bewerbungsinterview auch kleine Fragen oder Aufgaben gestellt werden, erhält der
Bewerber +30 Punkte, wenn er die Aufgabe gelöst hat, andernfalls -30. Sollte er
fachlich geeignet sein, gibt’s für ihn +50 Punkte, falls nicht -50. Hier gibt es
die höchste Punktzahl, da die fachliche Eignung die wichtigste ist. Die
verwendeten Datentypen sind bei allen Inputs boolean. Als Output steht der Score
mit dem Datentyp integer. Bei der Hauptdecision stellen die aufsummierten Punkte
des Scores einen Input dar, mit dem Datentyp integer. Da oftmals der persönliche
Eindruck auch eine Rolle spielt, stellt dieser hier ebenfalls einen Input für
die Hauptdecision dar. Dabei wird der Datentyp string verwendet (a, b und c), um
zwischen sehr gut, gut oder befriedigend auswählen zu können. Wenn der Bewerber
mehr als 50 Punkte erreicht, wird er bei gutem oder sehr gutem Eindruck
eingestellt, sonst nicht. Wenn er unter 50 Punkte erreicht, muss er noch einen
sehr guten persönlichen Eindruck hinterlassen um eingestellt zu werden. Bei
weniger als 20 Punkten, ist auch der Eindruck egal. Der Bewerber ist nicht gut
genug und wird nicht eingestellt. Der Output wird nochmal als string
dargestellt, um ein klares „Ja“ oder „Nein“ zur Einstellung auszugeben.

![](https://abload.de/img/8748cc8fd1370f0c7159719j18.png)

# 3) Technische Implementierung

## 3.1) Startvariablen und JSON Model Definieren

In der User Task *„50 Bewerbungen sammeln“* werden die Startvariablen und
das JSON Model definiert. Die Implementierung erfolgte über
den *„Listener“* als *„Execution Listener“*. Das Script Format ist *„JavaScript“* und
der Type *„Inline Script“*.

```javascript
// SpinJSON erstellen
var jsonObject = S('{ "bewerber":[]}');
 
// JSON Object speichern
execution.setVariable("bewerberJSON", jsonObject);
 
// Zähler erstellen
var zahler = 0;
 
// Zähler speichern
execution.setVariable("zahlerAuslesen", zahler);
execution.setVariable("zahlerEndauswahl", zahler);
execution.setVariable("zahlerTerminfindung", zahler);
```

Des Weiteren enthält die Tasks einen Output Parameter mit der Bezeichnung
*„anzahlBewerbung“* mit dem Inhalt *„2“*. Dieser wird für die Sequenzielle Multiple
Instanziierung in der Vorauswahl benötigt und gibt an, wie viele Bewerbungen
gesammelt werden sollen. In diesem Fall sind es zwei.

## 3.2) Zeitliche Begrenzung (Timer) einstellen

Die User Task *„50 Bewerbungen sammeln“* hat ein angeheftetes
unterbrechendes Timer Zwischenereignis. Die Einstellungen sind unter *„General
-\> Details“* zu finden. In unserem Prozess ist die *„Timer Definition Type“*
*„Duration“* und die *„Timer Definition“* nach _**„ISO  8601“**_ Norm auf _**„PT5M“**_ gestellt.
Damit läuft der Timer alle 5 Minuten.

Die 5 Minuten sind wie die *„anzahlBewerbungen“* zu testzwecken nicht synchron mit
dem Fachlichen Modell.

## 3.3) Schleifenoperatoren für die Vorauswahl einstellen

Der Schleifenoperator für die Vorauswahl ist über *„General -\> Multi Instance“*
einstellbar. Wir haben die Option *„Loop Cardinality“* gewählt, in der die
Variable *„anzahlBewerbung“* ausgelesen wird. Somit wird die die Sequenzielle
Multiple Instanziierung zweimal durchlaufen, falls das angeheftetes
unterbrechendes Timer Zwischenereignis nicht ausgelöst wurde und eine manuelle
Eingabe erfolgte.

## 3.4) Dateneingabe

Die Dateneingabe erfolgt in der User Task *„Bewerberdaten eingeben“* und enthält
folgende Felder:

| **ID**             | **Type** |
|--------------------|----------|
| vorname            | string   |
| nachname           | string   |
| geburtsdatum       | date     |
| studium            | boolean  |
| nc                 | enum\*   |
| berufserfahrung    | enum\*   |
| gehaltsvorstellung | long     |
| ehrenamt           | boolean  |
| mail               | string   |

 

\*Leider unterstützt die Camunda Engine keine Double Werte. Somit sind
Fließkommazahlen nicht möglich. Wir haben uns daher entschieden
auf enum auszuweichen und dem Anwender die Möglichkeit gegeben für *„nc“* und
*„berufserfahrung“* aus einem vordefinierten Bereich auszuwählen.

| **ID** | **Name**  |
|--------|-----------|
| a      | \< 2.0    |
| b      | 2.0 – 3.0 |
| c      | \> 3.0    |

| **ID** | **Name**                    |
|--------|-----------------------------|
| a      | Weniger als 2 Jahre         |
| b      | Genau oder mehr als 2 Jahre |

## 3.5) Multiple Daten im JSON Format speichern

Das Nutzen von JSON als Datenspeicher erlaubt uns mehrere Bewerber
abzuspeichern. Die Engine selbst würde bei jedem Durchgang die vorherige
Variable überschreiben. Damit wäre es uns nicht möglich bei den späteren
Aktivitäten auf diese Informationen zurückzugreifen und Bezug zu dieser Person
zu nehmen. Wie zum Beispiel bei der *„Terminfindung“* oder der *„Endauswahl“*.

Dafür haben wir die Script Task *„Bewerberdaten speichern“* erstellt. Hier wird
über JavaScript als Inline Script die Multiplen Daten im JSON Format
gespeichert.

Dazu wird der Output vom DMN *„Vorauswahl des Bewerbers durchführen“* ausgelesen
und in die Variable *„einladung“* gespeichert, wie man es Programmcode 1 - Script
Task *"Bewerberdaten speichern"* entnehmen kann. Im DMN selbst wird eine
Vorauswahl für einen Bewerber durchgeführt. Falls er diese besteht, wird der DMN
Output auf *„true“* gesetzt, andernfalls auf *„False“*.

Anschließend wird geprüft ob die Variable „einladung“ den Wert „true“ enthält,
also ob der Bewerber eingeladen wurde.  Falls ja, werden die eingegebenen
Variablen gezogen und in der Engine zwischengespeichert. Danach wird mit den
zwischengespeicherten Variablen ein String für das JSON Model generiert. Der
String muss dann via „S()“ in´s JSON Format konvertiert werden. Nun kann man das
JSON Objekt dem bisherige anhängen über den Befehl „append“. Über den „prop“
Parameter kommen wir an die gewünschte Stelle im Model.

Zuletzt wird das alte Model, mit dem neuen Model und dem neuen Datensatz,
überschrieben.

```javascript
// DMN Output auslesen
var einladung = execution.getVariable("einladung", einladung);
 
// Selbsterklärend
if (einladung == true) {
 
var bewerberJSON = execution.getVariable("bewerberJSON");
 
// Variablen ziehen
variableVorname = execution.getVariable("vorname");
variableNachname = execution.getVariable("nachname");
variableGeburtsdatum = execution.getVariable("geburtsdatum");
variableStudium = execution.getVariable("studium");
variableNC = execution.getVariable("nc");
variableBerufserfahrung = execution.getVariable("berufserfahrung");
variableGehaltsvorstellung = execution.getVariable("gehaltsvorstellung");
variableEhrenamt = execution.getVariable("ehrenamt");
variableMail = execution.getVariable("mail");
 
// String fürs JSON generieren
var str = "{\"vorname\":\"" + variableVorname + "\",\"nachname\":\"" + variableNachname+ "\",\"geburtsdatum\":\"" + variableGeburtsdatum + "\",\"studium\":" + variableStudium+ ",\"nc\":\"" + variableNC + "\",\"berufserfahrung\":\"" + variableBerufserfahrung +"\",\"gehaltsvorstellung\":" + variableGehaltsvorstellung + ",\"ehrenamt\":" +variableEhrenamt + ",\"eMail\":\"" + variableMail + "\"}";
 
// String zu JSON convertieren
var bewerberEntry = S(str);
 
// JSON dem bisherigen SpinJSON anhängen
bewerberJSON.prop("bewerber").append(bewerberEntry);
 
// Neue JSON speichern
execution.setVariable("bewerberJSON", bewerberJSON);
 
}
```

## 3.6) Schleifenoperatoren für die Terminfindung / Endauswahl

Die zweite und dritte sequenzielle multiple Instanziierung sind diesmal anders
als die erste implementiert. Die *„Call Activity“ „Termin finden“* greift dabei
auf das zuvor gespeicherte JSON Objekt zurück und liest dessen länge aus. Die
Lösung erlaubt es uns die Terminfindung und Endauswahl dynamisch auf der Anzahl
bestehender Bewerber der Vorauswahl zu durchlaufen.

 

Dazu wurde diesmal nicht die *„Loop Cardinality“* genutzt, sondern die
*„Collection“*. Mit dem Befehl „\${bewerberJSON.prop("bewerber").elements()}“
greifen wir auf das  JSON zurück, dass in der Variable bewerberJSON gespeichert
ist. Prop wurde zuvor schon erläutert und durch den *Parameter elements()*
erhalten wir die Anzahl der Elemente unseres Objekts.

## 3.7) Datenübergabe zwischen Hauptprozess und Call Activity

Dazu muss in der *„Call Activity”* unter *“Variables”* bei *“In Mapping”* und *“Out
Mapping”* eine Variable hinzugefügt werden und auf *„all“* gestellt werden, wie in
Abbildung 5 zu sehen.

![](https://abload.de/img/ef46c252c4a13dbecc1b3k2jd8.png)

## 3.8) Termin eingeben und eMail Adresse vorbereiten

In der *„Call Activity“* *“Termin finden”* gibt es die *„User Task"* *„Termin eingeben“*
in dem die Daten aus der Tabelle 4 eingegeben werden. Des Weiteren werden die
Daten aus dem JSON gelesen und unter dem selbigen Namen in eine Variable
gespeichert. Anschließend werden die Informationen „vorname, nachname und eMail“
aus dem JSON geladen. Aus dem *„vornamen“* und *„nachnamen“* wird der Bewerbername
erstellt und unter der Variablen *„bewerber“* gespeichert. Dieser wird im Formular
für den Anwender angezeigt um zu wissen, für welchen Kandidaten er den Termin
ausmacht. Dabei wird mit der Variablen *„zahlerAuslesen“* gearbeitet, die ganz zu
Beginn im Hauptprozess erstellt wurde und unter *Kapitel 3.1* behandelt wurde.
Damit erreichen wir, dass wir das JSON Objekt Datensatz für Datensatz
durchgehen.

Zusätzlich wird die Variable *„eMail“* aus dem JSON geladen, um in *Kapitel 3.10*
dem Bewerber eine Personalisierte eMail zustellen zu können.

```javascript
// JSON laden
var bewerberJSON = execution.getVariable("bewerberJSON");
 
// Zähler laden - um immer an das naechste Array im JSON zu gelangen
var zahlerAuslesen = execution.getVariable("zahlerTerminfindung");
 
// Erstes Array des JSON auslesen
var jsonVorname =S(bewerberJSON).prop("bewerber").elements().get(zahlerAuslesen).prop("vorname").value();
var jsonNachname =S(bewerberJSON).prop("bewerber").elements().get(zahlerAuslesen).prop("nachname").value();
var jsonMail =S(bewerberJSON).prop("bewerber").elements().get(zahlerAuslesen).prop("eMail").value();
 
// Speichern des Bewerbernamen als einzelne Datensatz
var bewerber = jsonVorname + " " + jsonNachname;
 
// Ausgabevariablen setzen
execution.setVariable("bewerber", bewerber);
execution.setVariable("mail", jsonMail);
 
// Zahler erhöhen und neuen Datensatz speichern
newZahler = zahlerTerminfindung + 1 ;
execution.setVariable("zahlerTerminfindung", newZahler);
```

| **ID**       | **Type** | 
|--------------|----------| 
| bewerber     | string   | 
| eingabeDatum | date     | 
| eingabeZeit  | enum\*   | 

| **ID** | **Value**     |
|--------|---------------|
| a      | 8:00 - 9:00   |
| b      | 9:00 - 10:00  |
| c      | 10:00 - 11:00 |
| d      | 11:00 - 12:00 |

## 3.9) Google API Abfrage

Für den Eintrag in den Google Kalender wurde auf die Google API und den
http-connector von Camunda zurückgegriffen.

Es handelt sich dabei um ein POST Request. Für den Request muss im Header der
Authentifizierungscode für den Kalender eingetragen werden. Für unsere
Testzwecke wurde ein temporärer Zugang über den
*„[oauthplayground](https://developers.google.com/oauthplayground/)“* von google
erstellt. Beim Payload wird über JavaScript ein JSON Objekt generiert, das als
Body des POST Requests dient. Dazu wird das eingegebene Datum aus der „User
Task“ *„Termin eingeben“* über eine eigene Funktion in eine ISO konforme Form
gebracht, so dass man diese im Request nutzen kann. Des Weiteren wird die *„enum“*
Variable *„eingabeZeit“* ausgelesen und der Eingabe entsprechend den Start und
Endzeit gesetzt.

```javascript
// Variablen laden
var eingabeDatum = execution.getVariable("eingabeDatum");
var eingabeZeit = execution.getVariable("eingabeZeit");
var bewerber = execution.getVariable("bewerber");

// Datum in String formatieren
var eingabeDatumString = getFormattedDate(eingabeDatum);

execution.setVariable("eingabeDatumString", eingabeDatumString);

if (eingabeZeit == "a") {

var eingabeStartZeit = "06:00:00";
var eingabeEndZeit = "06:59:00";

} else if (eingabeZeit == "b") {

var eingabeStartZeit = "07:00:00";
var eingabeEndZeit = "07:59:00";

} else if (eingabeZeit == "c") {

var eingabeStartZeit = "08:00:00";
var eingabeEndZeit = "08:59:00";

} else if (eingabeZeit == "d") {

var eingabeStartZeit = "09:00:00";
var eingabeEndZeit = "09:59:00";

}

execution.setVariable("eingabeStartZeit", eingabeStartZeit);
execution.setVariable("eingabeEndZeit", eingabeEndZeit);

// String im JSON Format erstellen
var str = "{\"start\": {\"timeZone\": \"Europe/Berlin\",\"dateTime\": \"" + eingabeDatumString + "T" + eingabeStartZeit + "Z\"},\"end\": {\"timeZone\": \"Europe/Berlin\",\"dateTime\":\"" + eingabeDatumString + "T" + eingabeEndZeit + "Z\"}, \"summary\": \"Bewerbungsgespräch " + bewerber + "\"}";

// Eigene FN um aus dem Date ein ISO-Date konformen String zu generieren
function getFormattedDate(date) {

var day = date.getDate();
var month = date.getMonth()+1;
var year = date.getYear() + 1900;

if (month < 10) {
month = "0" + month;
}

return year + '-' + month + '-' + day ;

}

str;
```

## 3.10) eMail Versand

Der eMail Versand erfolgt über den mail-send Connector von Camunda. Das Modul
ist nicht standardmäßig in der Camunda Engine enthalten und muss nachträglich
eingebunden werden. Dazu gibt es eine offizielle Dokumentation, an die auch wir
uns gehalten haben:

<https://github.com/camunda/camunda-bpm-mail>

Die „Service Task“ „Bewerber den Gesprächstermin senden“ greift bei der eMail
Adresse auf die in Kapitel 3.8 geladene eMail Adresse zurück. Zusätzlich ist der
Inhalt der eMail personalisiert auf den Bewerber zugeschnitten und enthält
Informationen wie dessen Vorname/Nachname und das Datum/Uhrzeit des
Bewerbungsgespräches.

## 3.11) JSON Datensatz erweitern

 Zuletzt werden noch alle wichtigen Daten, die im Verlauf des Prozesses
entstanden sind, in das JSON Modell gespeichert. Dazu werden zunächst alle
benötigten Daten geladen und anschließend in das JSON Modell, an der
entsprechenden Stelle, angehängt.

```javascript
// DMN Output in Variable speichern
execution.setVariable("einstellen", einstellen);

// Variablen Laden
var bewerberJSON = execution.getVariable("bewerberJSON");
var zahlerEndauswahl = execution.getVariable("zahlerEndauswahl")

// Variablen ziehen
execution.getVariable("bewerber", bewerber);
variableFachlicheEignung = execution.getVariable("fachlicheEignung");
variableAufgabenErgebnis = execution.getVariable("aufgabenErgebnis");
variablePersonlicherEindruck = execution.getVariable("personlicherEindruck");
variableEinstellen = execution.getVariable("einstellen");

// JSON dem bisherigen JSON anhängen
bewerberJSON.prop("bewerber").elements().get(zahlerEndauswahl).prop("fachlicheEignung", variableFachlicheEignung);
bewerberJSON.prop("bewerber").elements().get(zahlerEndauswahl).prop("aufgabenErgebnis", variableAufgabenErgebnis);
bewerberJSON.prop("bewerber").elements().get(zahlerEndauswahl).prop("personlicherEindruck", variablePersonlicherEindruck);
bewerberJSON.prop("bewerber").elements().get(zahlerEndauswahl).prop("einstellung", variableEinstellen);

// Zähler erhöhen
newZahler = zahlerEndauswahl + 1 ;
execution.setVariable("zahlerEndauswahl", newZahler);

// Neue JSON speichern
execution.setVariable("bewerberJSON", bewerberJSON);
```

# 4) Ausblick

Eine fachliche Optimierung am BPMN-Einstellungsprozess wäre theoretisch von der
Call Activity zum zugeklappten Pool des Bewerbers ebenfalls Nachrichtenflüsse
verlaufen zu lassen, da in dem Prozess “Terminfindung” ebenfalls
Nachrichtenflüsse zum Bewerber geführt werden. Dies ist durch die Camunda Engine
jedoch (noch) nicht möglich.

Durch die Implementierung der Datenhaltung im JSON Format, ist eine Anbindung an
externe Systeme leicht zu realisieren, da es einer der gängigsten Standards für
den Datenaustausch ist. So könnte der Prozess in einer Bewerbungsmanagement
Software eingebunden werden um Daten vom System zu empfangen, sie teils
automatisiert zu verarbeiten und anschließend wieder auszugeben.

Eine Optimierung am Prozess wäre bei der Terminauswahl für das
Bewerbungsgespräch möglich. Denn aktuell können Terminplätze doppelt belegt
werden. Die Abfrage könnte so erweitert werden, dass bestehende Termine im
Kalender geprüft werden und darauf basierend dem Anwender freie Zeitblöcke zur
Auswahl anbieten.
