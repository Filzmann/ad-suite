# Aktiver Ausführungsplan der AD-Suite

Stand: 26. Juli 2026
Status: Planung; jeder Lieferblock benötigt einen eigenen Auftrag und eine
eigene Abnahme.

Die dauerhaften Produktziele stehen in `../ROADMAP.md`. Dieser Plan beschreibt
lediglich die derzeit vorgesehene Reihenfolge.

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

- Produkt-/Menükatalog als eigenen Candidate prüfen und freigeben.
- Reihenfolge und Berechtigungen unverändert charakterisieren.
- Externe Links erst danach mit HTTPS-Validierung und serverseitiger
  Gruppensichtbarkeit anbinden.

## 5. Gruppenverträge bereinigen

- Gemeinsamen BR-Gruppenvertrag separat entscheiden und additiv migrieren.
- AD-Gruppenerkennung der Berechtigungsmatrix anschließend gegen einen
  read-only Organisationssnapshot prüfen.
- Rohgruppen und historische Snapshots revisionsfähig erhalten.

## 6. Datums- und Zeitbeschriftungen lokalisieren

- Mit einem charakterisierten Pilot in Assistenzplanung und AD Urlaub
  beginnen.
- Danach AD Raumplaner, AD Kalender und BRStunden umstellen.
- ISO-Daten, Monatsnummern und stabile Enum-Werte unverändert lassen.

## 7. Nextcloud-l10n vertikal einführen

- Werkzeug, Fallback und zunächst warnenden Check in einer Pilot-App
  etablieren.
- Danach OrgSuite/LocalBase und jede Fachapp einzeln migrieren.
- Einen verpflichtenden Fehler für neue Rohtexte erst nach abgeschlossener
  App-Migration aktivieren.

## 8. Weitere administrierbare Vorgaben prüfen

- Kalenderanbieterdefaults, sichtbare Kalendernamen, BR-Stammdaten,
  Dokumentvorlagen, Schicht-/Meetingdefaults und Aufbewahrung appweise
  klassifizieren.
- Eigentümer eindeutig zwischen Suite-Admin, App-Admin und persönlicher
  Einstellung trennen.
- Bestehende Werte additiv migrieren.

## 9. Externe Kalenderanbieter

- Google- und Apple-Abnahme wie geplant frühestens Mitte/Ende August 2026
  fortsetzen.
- OAuth, Secret-Verwaltung, Redirect-URLs und Providerfehler getrennt von
  Menü- und l10n-Umbauten liefern.

## 10. Abnahme je Lieferblock

- Jeder Schritt ist ein eigener rückbaubarer Commit-/Releaseblock.
- Öffentliche LocalBase-Verträge erhalten Provider- und Consumer-Tests sowie
  den vollständigen Workspace-Check.
- Releasekandidaten benötigen zusätzlich Delivery-Gate, frische
  Staging-Installation oder Updatepfad, sichtbare UI-Abnahme und Rückbau.

## Offene Entscheidungen

- Geltungsbereich persönlicher Zeitzonen gegenüber dem
  Organisationskalender.
- Erste vollständig unterstützte Sprache neben Deutsch.
- Abgrenzung organisationsspezifischer und fachlich unveränderbarer
  BR-Dokumenttexte.
- Bestehende Defaults: automatische additive Migration oder ausdrückliche
  Bestätigung im Adminbereich.
