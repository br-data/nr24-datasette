# nr24-datasette

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


Datenbank erstellen: https://datasette.io/tutorials/clean-data

https://sqlite-utils.datasette.io/en/stable/cli.html#inserting-csv-or-tsv-data

analog zu 

```
sqlite-utils insert mdbs.db mdb \
  diese_csv.csv --csv -d
```



https://datasette.io/plugins/datasette-explain

