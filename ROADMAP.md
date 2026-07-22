# Roadmap – AD Suite

Diese Roadmap bündelt app-übergreifende Produkt- und Qualitätsziele. Die Fachregeln, Sicherheitsverträge und konkreten Umsetzungsschritte verbleiben in den jeweils zuständigen App-Repositories.

## Suiteweiter Audit: Hardcodierungen und Lokalisierung

Der statische Audit umfasst die PHP- und JavaScript-Klassen von AD Kalender, Assistenzplanung, AD Urlaub, AD Raumplaner, LocalBase, OrgSuite, BRTop, BR-Stunden und Berechtigungsmatrix. Vor jeder Auslagerung wird unterschieden:

- **Stabile technische Verträge bleiben im Code:** App-IDs, Routen, Status- und Capability-Schlüssel, Datenbankschema-Grenzen, erlaubte Protokolle und Formate, Sicherheitslimits sowie RFC-/DAV-Marker sind keine administrativen Einstellungen.
- **Fachliche Startwerte bleiben nur Bootstrap-Defaults:** Eine Neuinstallation darf sinnvolle Vorgaben mitbringen; Organisationsnamen, Gruppen-IDs, Bereiche, Hierarchien, Standorte und Anbieteradressen müssen danach über die zuständige Administration änderbar sein.
- **Gemeinsam verwendete Kataloge erhalten genau eine Quelle:** Eine gemeinsame Klasse oder ein kleiner LocalBase-Vertrag ist nur vorgesehen, wenn mindestens zwei Apps denselben semantischen Vertrag benötigen. Fachapps behalten ihre eigene Fachlogik und erzwingen ihre Rechte selbst.
- **Benutzertexte werden lokalisiert:** Übersetzbare Beschriftungen, Meldungen, Datumsnamen und Dokumenttexte werden nicht als Maschinenvertrag oder Konfigurationsschlüssel verwendet.

## Priorität 1 – Duplizierte gemeinsame Verträge beseitigen

- **Suite- und Navigationskatalog:** Die AD-/BR-Appreihenfolge, Labels und Zielrouten sind derzeit in mehreren OrgSuite-PHP-Klassen, im JavaScript-Menü und teilweise in LocalBase getrennt hinterlegt. Ein kanonischer serverseitiger Katalog soll Navigation, Weiterleitung und ausgelieferte Menüdaten speisen. Die geplanten externen, gruppenbeschränkten Links werden an denselben validierten Menüvertrag angebunden.
- **BR-Gruppenvertrag:** Die Gruppen `Betriebsrat`, Vorsitz und Stellvertretung sind in BRTop und BR-Stunden mehrfach definiert; BR-Stunden verwendet zusätzlich einen zweiten eigenen Mitgliedsgruppenwert. Ein gemeinsamer, konfigurierbarer BR-Gruppenvertrag soll diese Duplizierung ersetzen. Bestehende Installationen benötigen eine additive Migration und Deny-by-default-Tests.
- **AD-Gruppenmuster:** Die Berechtigungsmatrix erkennt Assistenz-, EB- und PFK-Gruppen derzeit erneut über fest codierte reguläre Ausdrücke. Sie soll stattdessen einen read-only Katalog aus der konfigurierten `AdOrganizationDefinition` beziehungsweise einen kleinen Capability-Vertrag konsumieren. Rohgruppen bleiben für Revision und Export erhalten.
- **Produktkatalog:** Die Liste der AD-Fachprodukte und Anzeigenamen soll nicht unabhängig in LocalBase, OrgSuite und Installationslogik gepflegt werden. Der gemeinsame Katalog darf ausschließlich Installation und Navigation beschreiben und keine Fachberechtigung erteilen.

## Priorität 2 – Administrierbare Betriebs- und Organisationswerte

- **Kalenderregion und Zeitzone:** `Europe/Berlin` und `DE-BE` sind derzeit in Kalender-, Urlaubs- und Raumfunktionen festgelegt. Land, Bundesland/Region und fachliche Zeitzone sollen als gemeinsame, validierte Suite-Administration mit Berlin als Migrationsdefault geführt werden. Nextcloud-Systemwerte werden bevorzugt, sofern sie den benötigten organisationsweiten Vertrag eindeutig abbilden.
- **Ferien- und Feiertagsquelle:** AD Urlaub besitzt einen dynamischen OpenHolidays-Abruf mit Cache, während AD Raumplaner und AD Kalender Feiertage separat im Code berechnen. Provider, Validierung, Cache, Aktualitätsstatus und read-only Kalenderdaten sollen in einen kleinen gemeinsamen Vertrag überführt werden; Fachapps entscheiden weiterhin selbst über ihre Darstellung und fachliche Wirkung. Manuell gepflegte Feiertagslisten werden danach entfernt.
- **Externe Kalender:** Kopano-/CalDAV-Vorgabe, sichtbarer Zielkalendername und weitere betreiberabhängige Defaults sollen im AD-Kalender-Adminbereich konfigurierbar sein. Sichere Protokoll-Allowlist, OAuth-Endpunkte und interne DAV-Kennungen bleiben Codeverträge.
- **BR-Stammdaten und Dokumentvorgaben:** Gremienname, Anschrift, Ort, Standardtexte, Ablagebezeichnungen sowie relevante Gruppenbezüge sollen aus validierten BRTop-/BR-Stunden-Einstellungen oder versionierten Dokumentvorlagen stammen. Gesetzlich oder technisch feste Semantik bleibt nachvollziehbar im Fachcode; organisationsspezifische Adressen und Formulierungen nicht.
- **Sichere Defaults prüfen:** Schichtzeiten, Meetingzeit/-ort, Aufbewahrung, Erinnerungsintervalle und Kalender-Synchronisationsintervalle werden je Wert klassifiziert. Nutzer- oder organisationsabhängige Vorgaben werden administrierbar; technische Schutzgrenzen und robuste Ausführungsintervalle bleiben dokumentierte Konstanten.

## Priorität 3 – Datumsnamen aus Locale erzeugen

- Manuelle Monatslisten in Assistenzplanung, AD Urlaub und BR-Stunden sowie manuelle Wochentagslisten in AD Kalender, Assistenzplanung und AD Raumplaner werden durch locale-fähige Datumsformatierung ersetzt.
- JavaScript verwendet `Intl.DateTimeFormat` mit der aktiven Nextcloud-/Dokumentsprache. Für reine Kalenderdaten werden UTC-basierte Referenzdaten genutzt, damit die Zeitzone keinen Monats- oder Tageswechsel verursacht.
- PHP verwendet die Nextcloud-Lokalisierung beziehungsweise `IntlDateFormatter` mit der aktiven Benutzersprache. API-Payloads behalten ISO-Daten, Monatsnummern und stabile Enum-Werte; lokalisierte Namen entstehen erst in der Darstellung.
- Abkürzungen werden nicht durch Abschneiden erzeugt. Die Locale bestimmt Schreibweise und Interpunktion, beispielsweise `Mär`, `Mrz` oder `Mar`.
- Tests decken mindestens deutsche Ausgabe, einen englischen Fallback, Jahresgrenzen und eine nichtdeutsche Locale ab.

## Priorität 4 – Nextcloud-l10n schrittweise einführen

- Jede App erhält einen eigenen Nextcloud-Übersetzungsbereich unter `l10n/`; es entsteht keine globale Klasse mit allen Texten. LocalBase übersetzt nur tatsächlich gemeinsam gerenderte LocalBase-Texte.
- Templates verwenden den Nextcloud-Übersetzer, PHP-Klassen injizieren `IL10N`, und JavaScript nutzt den Nextcloud-Übersetzungsaufruf mit der jeweiligen App-ID. Pluralformen und Platzhalter werden statt zusammengesetzter Satzfragmente gepflegt.
- In Datenbank und API bleiben technische Schlüssel sprachneutral. Konfigurierte Eigennamen und frei eingegebene Texte werden nicht automatisch übersetzt; Systemlabels werden bei der Ausgabe lokalisiert.
- Die Einführung erfolgt vertikal je App: zuerst Navigation und häufige Oberflächentexte, danach Validierungs- und Fehlermeldungen, E-Mails und Dokumente. Jeder Schritt enthält Fallback-, Platzhalter-, Plural- und JavaScript/PHP-Contract-Tests.
- Ein CI-Check soll neue direkt sichtbare Rohtexte melden, zunächst warnend und nach abgeschlossener Migration verbindlich. Technische Logs, Testfixtures und bewusst konfigurierte Inhalte benötigen eine enge, dokumentierte Ausnahme.

## Kommende Schritte in sinnvoller Reihenfolge

### 1. Laufenden Kalender- und Urlaubsstand abschließen

- Die bereits begonnenen Änderungen an Wochen-/Monatsansicht, fixierten Personenachsen, wiederholenden Terminen, Kopano-Diagnose sowie Ferien-, Feiertags- und Jahresenddarstellung vollständig testen und als getrennten Releasekandidaten liefern.
- Browser-Smokes für horizontales/vertikales Scrollen, feste Tagesspalten, Sticky-Verhalten, Tooltip-Beschriftungen und unterschiedliche Fensterbreiten ergänzen.
- Staging-Abnahme und automatisches Update für diesen Stand abschließen, bevor app-übergreifende Verträge umgebaut werden. Dadurch bleibt ein klarer Rückfallpunkt erhalten.

### 2. Hardcoding-Inventar vervollständigen und klassifizieren

- Das erste suiteweite, maschinenlesbare Inventar liegt unter [`docs/HARDCODING-INVENTORY.json`](docs/HARDCODING-INVENTORY.json). Es wird bei jedem abgeschlossenen Lieferblock aktualisiert und je Fund als `Codevertrag`, `gemeinsamer Vertrag`, `Administration`, `persönliche Einstellung`, `Übersetzung` oder `Dokumentvorlage` klassifiziert.
- Vor jeder Verhaltensänderung Charakterisierungs- und Contract-Tests für bestehende Defaults anlegen.
- Technische Konstanten nicht vorsorglich konfigurierbar machen; nur belegte fachliche oder organisatorische Variabilität auslagern.

### 3. Gemeinsamen Kalenderkontext schaffen

- Zuerst Region, Zeitzone und den read-only Ferien-/Feiertagsvertrag in LocalBase entwerfen und ausdrücklich als öffentlichen Cross-App-Vertrag freigeben.
- Den bereits ausfallsicheren Provider-/Cache-Ansatz aus AD Urlaub in den gemeinsamen Vertrag überführen; bestehende AppConfig-Daten additiv übernehmen oder kontrolliert neu aufbauen.
- Danach AD Urlaub, AD Kalender und AD Raumplaner nacheinander als Consumer umstellen und erst anschließend die manuellen Feiertagsberechnungen entfernen.
- Provider-, Cache-, Altbestand-, Ausfall- und Consumer-Contract-Tests bilden ein gemeinsames Liefergate.

### 4. Suite- und Navigationskatalog konsolidieren

- Die derzeit duplizierten OrgSuite-/LocalBase-Produkt- und Menükataloge durch eine kanonische serverseitige Quelle ersetzen, ohne Reihenfolge oder Berechtigungen zu verändern.
- Anschließend die geplanten externen Links mit HTTPS-Validierung, Reihenfolge, Aktivstatus und serverseitiger Gruppensichtbarkeit auf diesem Vertrag aufbauen.
- Navigation bleibt reine Sichtbarkeit; interne Zielcontroller und externe Zielsysteme erzwingen ihre Rechte weiterhin selbst.

### 5. Gruppenverträge bereinigen

- Zuerst den gemeinsamen BR-Gruppenvertrag für BRTop und BR-Stunden mit Bestandsmigration und Deny-by-default-Tests einführen.
- Danach die fest codierte AD-Gruppenerkennung der Berechtigungsmatrix durch den read-only Organisations-/Capability-Vertrag ersetzen.
- Rohgruppen und historische Snapshots bleiben revisionsfähig; keine vorhandenen Gruppennamen werden automatisch umbenannt.

### 6. Datums- und Zeitbeschriftungen locale-fähig machen

- Mit einem kleinen, charakterisierten Pilot in Assistenzplanung und AD Urlaub beginnen, weil dort dieselbe manuelle Monatskopf-Logik sichtbar ist.
- Danach AD Raumplaner, AD Kalender und BR-Stunden umstellen; bestehende Arrays erst nach grünen Locale-Regressionstests entfernen.
- API-Verträge bleiben bei ISO-Daten, Monatsnummern und stabilen Enum-Werten. PHP und JavaScript erzeugen nur die Darstellung locale-abhängig.

### 7. Nextcloud-l10n vertikal einführen

- Zuerst Übersetzungswerkzeug, Verzeichnisstruktur, Fallback und CI-Warnung in einer kleinen Pilot-App etablieren.
- Danach OrgSuite/LocalBase und anschließend jede Fachapp einzeln migrieren: Navigation und UI, Validierung, E-Mails, Dokumente.
- Deutsch bleibt während jeder Zwischenstufe vollständig; ein verpflichtender CI-Fehler für neue Rohtexte wird erst nach der jeweiligen App-Migration aktiviert.

### 8. Weitere administrierbare Vorgaben auslagern

- Kalenderanbieter-Defaults, sichtbare Kalendernamen, BR-Stammdaten, Dokumentvorlagen, Schicht-/Meetingdefaults und Aufbewahrungswerte anhand des Inventars appweise bearbeiten.
- Jede Einstellung erhält einen klaren Eigentümer: suiteweit in OrgSuite/LocalBase, app-spezifisch im Adminbereich der Fachapp, persönlich im Einstellungstab der Fachapp.
- Bestehende Werte werden additiv migriert und behalten bis zur bewussten Änderung ihr bisheriges Verhalten.

### 9. Zurückgestellte externe Kalenderanbieter

- Google- und Apple-Anbindung wie vereinbart frühestens ab Mitte/Ende August fortsetzen.
- Vorher müssen der stabile Kopano-/CalDAV-Vertrag, die Admin-Diagnose, der locale-fähige AD-Kalender-Basissatz und die zugehörigen Sicherheits-/Staging-Smokes grün sein.
- OAuth-Registrierung, Secret-Verwaltung, Redirect-URLs und Providerfehler werden getrennt vom allgemeinen Menü- und l10n-Umbau geliefert.

### 10. Abnahme je Lieferblock

- Jeder Schritt bleibt ein eigener, rückbaubarer Commit-/Releaseblock; keine suiteweite Großmigration in einem Durchgang.
- Für öffentliche LocalBase-Verträge laufen Provider- und Consumer-Contract-Tests in allen betroffenen Apps sowie der vollständige Workspace-Check.
- Releasekandidaten benötigen zusätzlich Delivery-Gate, frische Staging-Installation beziehungsweise Updatepfad, sichtbare UI-Abnahme und dokumentierten Rückbau.

## Vor der Umsetzung zu entscheiden

- Soll die gemeinsame Kalenderregion ausschließlich administrativ gelten oder dürfen Nutzer*innen für reine Anzeigezwecke eine persönliche Zeitzone wählen?
- Welche Sprache ist neben Deutsch die erste vollständig unterstützte Übersetzung?
- Welche BR-Dokumenttexte sind organisationsspezifische Vorlagen und welche müssen als versionierter fachlicher Standard unveränderbar bleiben?
- Welche heutigen Defaults sollen bei bestehenden Installationen unverändert migriert und welche beim ersten Administrationsaufruf ausdrücklich bestätigt werden?
