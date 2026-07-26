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

- **Gemeinsamen Kalendervertrag abnehmen:** Der im aktuellen lokalen
  Arbeitsstand nach LocalBase verschobene Kalenderkontext und
  Ferien-/Feiertagsvertrag benötigt vollständige Provider-, Migrations-,
  Consumer- und Staging-Abnahme. Bis zu getrennten Commits und einem grünen
  Delivery-Gate ist er kein veröffentlichter Releasevertrag.
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

Die derzeit vorgesehene Reihenfolge, Abschlusskriterien und offenen
Entscheidungen stehen im zeitlich begrenzten
[`docs/IMPLEMENTATION-PLAN.md`](docs/IMPLEMENTATION-PLAN.md).
