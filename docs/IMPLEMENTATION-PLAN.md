# Aktiver Ausführungsplan der AD-Suite

Stand: 27. Juli 2026
Status: Planung; die am 27. Juli 2026 freigegebenen Learning Candidates wurden
in repository-eigene Roadmap-Aufgaben verschoben. Jeder Lieferblock benötigt
weiterhin einen eigenen Auftrag und eine eigene Abnahme.

Die dauerhaften Produktziele stehen in `../ROADMAP.md`. Dieser Plan beschreibt
lediglich die derzeit vorgesehene Reihenfolge.

Der appübergreifende L10n-Rollout ist nicht Teil dieses aktiven
Ausführungsplans. Er bleibt als nicht freigegebene Zukunftsplanung in der
Roadmap vorgemerkt; lokale `*-L10N`-Einträge begründen keinen Auftrag.

## 1. Laufenden Kalender- und Urlaubsstand abschließen

- Begonnene Änderungen an Ansichten, Terminserien, Kalenderkontext,
  Providerdiagnose und Ferien-/Feiertagsdarstellung vollständig testen.
- Browser-Smokes für Scrollen, Sticky-Verhalten, Beschriftungen und
  Fensterbreiten ergänzen.
- Staging-Abnahme und Updatepfad abschließen, bevor weitere
  Cross-App-Verträge umgebaut werden.

## 2. Hardcoding-Inventar aktualisieren

- Inventar gegen den tatsächlichen Commitstand jedes Repositorys neu erheben.
- Funde als Codevertrag, gemeinsamer Vertrag, Administration, persönliche
  Einstellung, Übersetzung oder Dokumentvorlage klassifizieren.
- Vor Verhaltensänderungen bestehende Defaults charakterisieren.

## 3. Gemeinsamen Kalenderkontext abnehmen

- LocalBase-Provider-, Cache-, Migrations- und Vertragsprüfungen abschließen.
- AD Urlaub, AD Kalender und AD Raumplaner als Consumer vollständig prüfen.
- Erst nach grünen Consumer-Verträgen verbliebene parallele Implementierungen
  entfernen.

## 4. Suite- und Navigationskatalog konsolidieren

- Umsetzung und Abnahme über `PARENT-AD-CATALOG`, `LB-AD-CATALOG`,
  `ORGS-AD-CATALOG`, `RECR-AD-CATALOG` und `ADS-AD-CATALOG-DOCS`.
- Externe Links bleiben davon getrenntes OrgSuite-Roadmapziel.

## 5. Gruppenverträge bereinigen

- BR-Gruppen über `LB-BR-GROUPS`, `BRT-BR-GROUPS` und `BRS-BR-GROUPS`.
- Organisationssnapshot und Matrix über `LB-AD-ORG-SNAPSHOT`,
  `BPM-AD-ORG-SNAPSHOT` und `BPM-FOLDER-RIGHTS`.

## 6. Weitere administrierbare Vorgaben prüfen

- Kalenderdefaults über `ADC-ADMIN-DEFAULTS`.
- BR-Stammdaten und Vorlagen über `BRT-DOCUMENT-CONFIG` und
  `BRS-DOCUMENT-CONFIG`.
- Andere Defaults bleiben erst nach eigener Evidenzprüfung neue Aufgaben.

## 7. Externe Kalenderanbieter

- Google- und Apple-Abnahme wie geplant frühestens Mitte/Ende August 2026
  fortsetzen.
- OAuth, Secret-Verwaltung, Redirect-URLs und Providerfehler getrennt von
  Menü-Umbauten liefern.

## 8. Abnahme je Lieferblock

- Jeder Schritt ist ein eigener rückbaubarer Commit-/Releaseblock.
- Öffentliche LocalBase-Verträge erhalten Provider- und Consumer-Tests sowie
  den vollständigen Workspace-Check.
- Releasekandidaten benötigen zusätzlich Delivery-Gate, frische
  Staging-Installation oder Updatepfad, sichtbare UI-Abnahme und Rückbau.

## Offene Entscheidungen

Offene Candidate-Entscheidungen stehen jetzt direkt an den betroffenen
Aufgaben: BR-Gruppenmitgliedschaft und Umbenennung vorhandener Kalender.
Dokumentsprache und erste vollständig unterstützte Sprache neben Deutsch
gehören ausschließlich zur nicht freigegebenen L10n-Zukunftsplanung.
