# nr24-datasette

## todo

- in rel_mdb_ausschuesse, col funktion: - zu echtem NA

- adresses: NA als echte leere Felder, nicht NA-String

## Intro

### Was ist eine Datenbank?

Eine simple Erklärung: Eine Datenbank sind mehrere Tabellen, die miteinander in Zusammenhang stehen (können).

Es gibt viele verschiedene Datenbanksysteme, zu den bekanntesten gehören `MySQL`, `Postgres` oder `sqlite`, mit der wir arbeiten. 
Der Vorteil von `sqlite`: Die ganze Datenbank ist nur eine einzige Datei. 


### Was ist datasette?

Die datasette ist eine Software, um Daten zu erkunden und zu veröffentlichen. 
Sie basiert auf einer `sqlite`-Datenbank und es existieren eine Reihe von [Erweiterungen](https://datasette.io/plugins), etwa um Daten zu visualisieren. 
Und - nicht zu vernachlässigen - es gibt [sehr gute Anleitungen](https://datasette.io/tutorials). 

[Webseite datasette.io](https://datasette.io/)

Wer's auch benutzt: Bellingcat, hier zu sehen: https://www.bellingcat.com/news/rest-of-world/2022/04/01/food-delivery-leak-unmasks-russian-security-agents/

### Was ist ein Terminal/die Kommandozeile?

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

```
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


pipx install sqlite-utils

https://datasette.io/plugins/datasette-explain


## Daten importieren

Eine Datenbank besteht aus  

Wir haben da was vorbereitet...

Daten zu Abgeordneten des Bundestags.

Datenbank erstellen: https://datasette.io/tutorials/clean-data

https://sqlite-utils.datasette.io/en/stable/cli.html#inserting-csv-or-tsv-data

analog zu 

```
sqlite-utils insert mdbs.db mdb \
  diese_csv.csv --csv -d
```


sqlite-utils insert mdbs.db rel_mdb_ausschuesse data/rel_mdbs_ausschuesse.csv --csv --detect-types --pk=mdb_id  

sqlite-utils insert mdbs.db ausschuesse data/ausschuesse.csv --csv --detect-types --pk=ausschuss_id 

sqlite-utils insert mdbs.db adresses data/mdb_addresses.csv --csv --detect-types   

Ansehen:  sqlite-utils schema mdbs.db


run: datasette mdbs.db

## Plugins installieren und verwenden

## Abfragen


```sql
select * from mdbs 
inner join rel_mdb_ausschuesse on mdbs.mdb_id=rel_mdb_ausschuesse.mdb_id
inner join ausschuesse on rel_mdb_ausschuesse.ausschuss_id=ausschuesse.ausschuss_id
```

Interessante Abfrage als View abspeichern
- SQL query irgendwie in metadata.json reinpacken


## Metadata.json/Metadata.yaml: Einstellungen

