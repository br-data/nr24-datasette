# nr24-datasette


## todo

- datum


## Intro

### Was ist eine Datenbank?

Ein paar simple Erklärungen: 

- eine Datenbank sind mehrere Tabellen, die miteinander in Zusammenhang stehen (können).
- strukturierte Sammlung von Daten
- der Zweck ist die Organisation, das Speichern und das Abrufen von Daten 

Es gibt viele verschiedene Datenbanksysteme, zu den bekanntesten gehören `MySQL`, `Postgres` oder `sqlite`, mit der wir arbeiten. 
Der Vorteil von `sqlite`: Die ganze Datenbank ist nur eine einzige Datei. 


### Was ist datasette?

Die datasette ist eine Software, um Daten zu erkunden und zu veröffentlichen. 
Sie basiert auf einer `sqlite`-Datenbank und es existieren eine Reihe von [Erweiterungen](https://datasette.io/plugins), etwa um Daten zu visualisieren. 
Und - nicht zu vernachlässigen - es gibt [sehr gute Anleitungen](https://datasette.io/tutorials). 

[Webseite datasette.io](https://datasette.io/)

Wer's auch benutzt: Bellingcat, hier zu sehen: https://www.bellingcat.com/news/rest-of-world/2022/04/01/food-delivery-leak-unmasks-russian-security-agents/

### Was ist ein Terminal/die Kommandozeile (Command Line Interface, CLI)?

Die Eingabeaufforderung, dieses schwarze Fenster mit heller Schrift...  

Aber keine Angst: Nur der Kommunikationsweg mit dem Computer ist ein anderer. Texteingaben statt Klicks. 

## Installation

- Github-Repo [br-data/nr24-datasette](https://github.com/br-data/nr24-datasette) forken (= in das eigene Github kopieren)
- Einen Codespace bei Github erstellen: Auf `Code` (grüner Button) -> Tab `Codespaces` -> `Create codespace on ...`
- Dann öffnet sich ein neuer Tab und darin befindet sich eine Entwicklungsumgebung, die auf dem Editor VS Code basiert
- Unten befindet sich ein Tab `Terminal`, dort installieren wir datasette:

```bash
pipx install datasette
```

Prüfen, ob es geklappt hat: 

```bash
datasette --version
```

Datasette braucht das Plugin [datasette-codespace](https://datasette.io/plugins/datasette-codespaces), um in und mit Github Codespaces arbeiten zu können:

```bash
datasette install datasette-codespaces
```

Datasette 
```bash
datasette data.db --create
```

Simon Willison hat auch ein CLI-Tool geschrieben, um mit `sqlite`-Datenbank zu arbeiten. Es heißt `sqlite-utils`.

Installation
```bash
pipx install sqlite-utils
```

https://datasette.io/plugins/datasette-explain


## Daten in die Datenbank importieren

Wir haben da was vorbereitet...

Daten zu Abgeordneten des Bundestags.

Anleitung, um eine Datenbank zu erstellen: https://datasette.io/tutorials/clean-data

https://sqlite-utils.datasette.io/en/stable/cli.html#inserting-csv-or-tsv-data

Wir benutzen `insert` Befehl, um eine CSV-Tabelle als eine Tabelle in eine Datenbank zu schreiben: https://sqlite-utils.datasette.io/en/stable/cli.html#inserting-csv-or-tsv-data


```bash
sqlite-utils insert mdbs.db mdb data/mdbs.csv --csv --detect-types --pk=mdb_id
sqlite-utils insert mdbs.db rel_mdb_ausschuesse data/rel_mdbs_ausschuesse.csv --csv --detect-types --pk=mdb_id --pk=ausschuss_id
sqlite-utils insert mdbs.db ausschuesse data/ausschuesse.csv --csv --detect-types --pk=ausschuss_id
sqlite-utils insert mdbs.db adresses data/mdb_addresses.csv --csv --detect-types
```

Vorschau auf der Kommandozeile:

```bash
sqlite-utils schema mdbs.db
```

## Datasette starten

Kommando: `datasette pfad_zur_datenbank`

```bash
datasette mdbs.db
```

## Full-text search

Lohnt sich so gut wie immer: Alles durchsuchbar machen mit einer Suchleiste oben

https://docs.datasette.io/en/stable/full_text_search.html

In der DB mdbs.db und der Tabelle mdb die Spalten `name wahlkreis_name fraktion mandatsart` durchsuchbar machen:

```bash
sqlite-utils enable-fts mdbs.db mdb name wahlkreis_name fraktion mandatsart
```

## Plugins installieren und verwenden

Liste aller vorhandenen Plugins: https://datasette.io/plugins

### Karte

https://datasette.io/plugins/datasette-cluster-map

Wenn sich etwa Koordinaten in den Tabellen befinden, werden sie automatisch auf eine Karte gezeichnet, solange wie Standardnamen haben wie `lat`, `long`.

Erst das Paket installieren, dann wieder datasette starten und die Tabelle öffnen mit den Koordinaten:

```bash
datasette install datasette-cluster-map
datasette mdbs.db
```

### Charts

https://datasette.io/plugins/datasette-plot

Nach der Installation gibt es einen "Show Plot"-Button an jeder Tabelle und dort kann man sich was zurecht klicken. 

Aber Vorsicht: Plotten kann man vieles, ob es Sinn ergibt, muss man selbst prüfen. 

## Abfragen

```sql
select * from mdbs 
inner join rel_mdb_ausschuesse on mdbs.mdb_id=rel_mdb_ausschuesse.mdb_id
inner join ausschuesse on rel_mdb_ausschuesse.ausschuss_id=ausschuesse.ausschuss_id
```

Interessante Abfrage als View abspeichern
- SQL query irgendwie in metadata.json reinpacken


## datasette konfigurieren

> Data loves metadata. Any time you run Datasette you can optionally include a JSON file with metadata about your databases and tables. Datasette will then display that information in the web UI.

https://docs.datasette.io/en/stable/metadata.html

Über eine bestimmte Datei kann man datasette weiter konfiguieren, entweder als `json` oder `yaml` - Geschmackssache. 
Beispiele: 
- Beschreibungen der Datenbank, einzelnen Tabellen oder Spalten hinterlegen
- Wichtige Abfragen hinterlegen, die dann wie Tabellen anzusteuern sind

Um die datasette mit diesen Konfigurationen zu starten, muss man mit dem `--metadata`-Parameter auf die Datei verweisen. 

```bash
datasette mdbs.db --metadata metadata.json
```

### Aufgabe

Eine der Abfrage als sogenannte "canned query" in der `metadata.json` hinterlegen.

## Abfrage nach Datum

Das Geburtstdatum der MdB's steht so in den Ausgangsdaten: `07.07.1986` - so wird das nicht von der `sqlite` als Datum erkannt. 

Mit der `convert`-Funktion von `sqlite-utils` können wir die Angaben in echte Zeitangaben umwandeln.

```bash
sqlite-utils convert mdbs.db mdb geburtsdatum 'r.parsedatetime(value, dayfirst=True)'
```
Mehr dazu: 
- https://datasette.io/tutorials/clean-data
- https://sqlite-utils.datasette.io/en/stable/cli.html#sqlite-utils-convert-recipes


## Links

- [datasette.io](https://datasette.io/)
- Verschiedenste Tutorials: https://datasette.io/tutorials/
- SQL-Cheatsheet: https://pbs.twimg.com/media/GSiaz9SXcAAY_y2?format=jpg&name=medium