# Roadmap – AD Suite

Diese Roadmap bündelt app-übergreifende Produkt- und Qualitätsziele. Die Fachregeln, Sicherheitsverträge und konkreten Umsetzungsschritte verbleiben in den jeweils zuständigen App-Repositories.

## Verteilte Aufgabenübersicht

Die beim Hardcoding-Audit freigegebenen Aufgaben werden in den tatsächlich
zuständigen Repositories gepflegt:

| Themenblock | Kanonische Aufgaben |
| --- | --- |
| Organisationssnapshot und Berechtigungsmatrix | `LB-AD-ORG-SNAPSHOT`, `BPM-AD-ORG-SNAPSHOT`, `BPM-FOLDER-RIGHTS` |
| BR-Gruppen | `LB-BR-GROUPS`, `BRT-BR-GROUPS`, `BRS-BR-GROUPS` |
| Kalenderdefaults | `ADC-ADMIN-DEFAULTS` |
| BR-Stammdaten und Vorlagen | `BRT-DOCUMENT-CONFIG`, `BRS-DOCUMENT-CONFIG` |
| Technische Dokumentreferenzen | `PARENT-DOC-REFS` |
| Explizite RC-Bereinigung | `PARENT-RC-CLEANUP` |

## Zukunftsplanung – nicht freigegeben

Der appübergreifende L10n-Rollout ist ausschließlich als spätere
Zukunftsplanung vorgemerkt. Lokale `*-L10N`-Einträge sind keine freigegebenen
Umsetzungsaufgaben. Pilot-App, Reihenfolge, unterstützte Locales,
Fallbackvertrag und Rohtext-Gate benötigen vor jeder Umsetzung eine eigene
appübergreifende Freigabe.

Die Reihenfolge laufender Kalender-/Urlaubsabnahmen und sonstiger, nicht aus
den Learning Candidates stammender Arbeit steht weiterhin im zeitlich
begrenzten [`docs/IMPLEMENTATION-PLAN.md`](docs/IMPLEMENTATION-PLAN.md).
