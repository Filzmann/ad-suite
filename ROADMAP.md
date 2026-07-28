# Roadmap – AD Suite

Diese Roadmap bündelt app-übergreifende Produkt- und Qualitätsziele. Die Fachregeln, Sicherheitsverträge und konkreten Umsetzungsschritte verbleiben in den jeweils zuständigen App-Repositories.

## Freigegebene Umsetzungsaufgabe

### ADS-AD-CATALOG-DOCS – Produktkatalog in öffentlichen Unterlagen abbilden

Status: bereit nach `LB-AD-CATALOG` und `PARENT-AD-CATALOG`

- Produktübersicht und Architekturunterlagen aus dem versionierten Katalog
  ableiten oder durch einen Parent-Contract-Test dagegen prüfen.
- `adcalendar`, `adplaner`, `adurlaub`, `adroom` und `adrecruitment` als
  katalogisierte Fachprodukte aufführen; LocalBase und OrgSuite getrennt als
  Infrastruktur kennzeichnen.
- Menü-/Katalogzugehörigkeit und tatsächliche Release-Bundle-Zugehörigkeit
  sichtbar unterscheiden. AD Recruitment wird ohne eigene Releasefreigabe
  nicht stillschweigend in bestehende Suite- oder Produktarchive aufgenommen.
- Installations-, Betriebs-, Abnahme- und Releaseunterlagen dürfen nur die
  Produkte als enthalten bezeichnen, deren Bundle-Eigenschaft dies
  ausdrücklich festlegt.
- Dokumentations-Contract, Manifest-/Archivprüfung und Links müssen grün sein.

## Verteilte Aufgabenübersicht

Die beim Hardcoding- und Lokalisierungsaudit freigegebenen Aufgaben werden in
den tatsächlich zuständigen Repositories gepflegt:

| Themenblock | Kanonische Aufgaben |
| --- | --- |
| Produkt- und Menükatalog | `PARENT-AD-CATALOG`, `LB-AD-CATALOG`, `ORGS-AD-CATALOG`, `RECR-AD-CATALOG`, `ADS-AD-CATALOG-DOCS` |
| Organisationssnapshot und Berechtigungsmatrix | `LB-AD-ORG-SNAPSHOT`, `BPM-AD-ORG-SNAPSHOT`, `BPM-FOLDER-RIGHTS` |
| BR-Gruppen | `LB-BR-GROUPS`, `BRT-BR-GROUPS`, `BRS-BR-GROUPS` |
| Kalenderdefaults | `ADC-ADMIN-DEFAULTS` |
| BR-Stammdaten und Vorlagen | `BRT-DOCUMENT-CONFIG`, `BRS-DOCUMENT-CONFIG` |
| Locale und Nextcloud-l10n | die jeweilige `*-L10N`-Aufgabe in jedem App-Repository |
| Technische Dokumentreferenzen | `PARENT-DOC-REFS` |
| Explizite RC-Bereinigung | `PARENT-RC-CLEANUP` |

Die Reihenfolge laufender Kalender-/Urlaubsabnahmen und sonstiger, nicht aus
den Learning Candidates stammender Arbeit steht weiterhin im zeitlich
begrenzten [`docs/IMPLEMENTATION-PLAN.md`](docs/IMPLEMENTATION-PLAN.md).
