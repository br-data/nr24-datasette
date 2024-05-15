# nr24-datasette

## Installation

- Github-Repo [br-data/nr24-datasette](https://github.com/br-data/nr24-datasette) forken (= in das eigene Github kopieren)
- Einen Codespace bei Github erstellen: Auf `Code` (grüner Button) -> Tab `Codespaces` -> `Create codespace on ...`
- Dann öffnet sich ein neuer Tab und darin befindet sich eine Entwicklungsumgebung, die auf dem Editor VS Code basiert
- Unten befindet sich ein Tab `Terminal`, dort installieren wir Befehlen als erstes datasette nach dieser Anleitung
    - `pipx install datasette`
    - hat's geklappt? Prüfen mit `datasette --version`
    - datasette braucht ein Plugin, um mit Codespaces arbeiten zu können, das installieren wir auch: `datasette install datasette-codespaces`