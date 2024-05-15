# nr24-datasette

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

Datasette braucht ein Plugin, um in und mit Github Codespaces arbeiten zu können:

```bash
datasette install datasette-codespaces
```
