# AGENTS.md – AD Suite

## Zweck

Dieses eigenständige Repository ist die öffentliche Produkt- und Releaseübersicht der AD-Suite für Nextcloud. Deploybarer App-Code verbleibt in den sechs getrennten App-Repositories.

Enthalten sind ausschließlich:

- Produktübersicht und Links zu den App-Repositories,
- Installations-, Betriebs-, Rückbau- und Abnahmeunterlagen,
- öffentlich geeignete Releasehinweise und Prüfsummen.

Releasearchive werden als GitHub-Release-Assets veröffentlicht und nicht dauerhaft im Git-Tree versioniert.

## Repository-Grenzen

- Kein App-Code und keine Nextcloud-Migrationen in dieses Repository verschieben.
- Keine internen DDEV-Pfade, Zugangsdaten, realen Personen-, Team- oder Falldaten veröffentlichen.
- Technische Beispiele verwenden ausschließlich neutrale Bezeichnungen wie Team A, Team B und Team C.
- Quellcodeänderungen werden im jeweils zuständigen App-Repository vorgenommen.

## Veröffentlichung

- Vor jedem Release das Delivery-Gate im lokalen Parent-Workspace erfolgreich ausführen.
- Versionen, Commit-IDs und SHA-256-Prüfsummen müssen mit dem erzeugten Manifest übereinstimmen.
- Releasekandidaten klar als nicht produktionsfreigegeben kennzeichnen.
- Installation auf einem frischen Staging-System, Rückbau, Datenschutzprüfung und fachliche Abnahme bleiben vor Production verpflichtend.

## Git

- Vor Commits `git status --short`, `git diff --stat` und `git diff --name-only` prüfen.
- Dateien gezielt stagen; niemals `git add .`.
- Push und GitHub-Releases nur nach ausdrücklicher Freigabe durch Simon. Die Freigabe für die aktuelle Erstveröffentlichung liegt vor.
