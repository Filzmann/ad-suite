# Delivery-Gate der AD-Suite

Das Delivery-Gate bündelt die wiederholbaren Prüfungen vor jedem Releasekandidaten. Ein Release darf nur aus sauberen App-Repositories gebaut werden.

## Stufe 1: lokale Pflichtprüfung

```bash
cd ~/projects/br-nextcloud-apps
scripts/verify-ad-suite-delivery.sh
```

Geprüft werden:

- App-Metadaten und die festgelegte Nextcloud-34-/PHP-8.3-Kompatibilität,
- AGPL-Lizenz, README, Changelog und App-Anweisungen,
- fehlende lokale, ungültige oder WordPress-spezifische Produktionsverweise,
- Symlinks und Shell-Syntax,
- alle schnellen PHP- und JavaScript-Tests,
- reproduzierbarer Paketbau, Archivwurzeln und SHA-256-Prüfsummen.

## Stufe 2: Nextcloud-Container

```bash
RUN_DDEV_CHECKS=1 scripts/verify-ad-suite-delivery.sh
```

Diese Stufe ergänzt den Nextcloud-Status und prüft, ob alle sechs Apps aktiviert sind.

## PHP-Abdeckung

Die isolierten PHP-Tests können zusätzlich mit Xdebug/PHPCOV gemessen werden. Die Entwicklungsabhängigkeit wird aus dem versionierten Lockfile installiert und nicht in die App-Archive gepackt:

```bash
cd ~/projects/br-nextcloud-apps/nextcloud-dev
ddev xdebug on
cd ..
scripts/measure-ad-suite-php-coverage.sh
cd nextcloud-dev
ddev xdebug off
```

Der Bericht liegt unter `build/coverage/php-summary.tsv`. Das Skript erzwingt standardmäßig mindestens 40 Prozent Gesamt-Line-Coverage; ein bewusst höherer Grenzwert kann über `MIN_TOTAL_COVERAGE` gesetzt werden.

Zusätzlich vergleicht das Gate jede App und den Gesamtwert mit der versionierten Baseline unter `scripts/ad-suite-php-coverage-baseline.tsv`. Ein Rückgang einer einzelnen App wird damit auch dann abgelehnt, wenn eine andere App den Gesamtwert ausgleicht. Die Baseline wird bei nachweislich verbesserter Testabdeckung angehoben; ein Absenken ist keine reguläre Lösung für einen fehlgeschlagenen Testlauf und muss als bewusste Ausnahme begründet werden.

## TDD-Vertrag

Neue Fachlogik, Fehlerkorrekturen, Berechtigungen, Validierungen und Konfliktregeln werden grundsätzlich test-first nach Rot – Grün – Refactor entwickelt. Für neuen oder wesentlich geänderten ausführbaren Code werden mindestens 85 Prozent Line-Coverage angestrebt. Sicherheitskritische Regeln benötigen unabhängig von der Prozentzahl relevante Allow-, Deny- und Grenzfälle.

Technische Spikes und reine UI-Erkundungen dürfen vorübergehend ohne vorgelagerten Test entstehen. Vor der Übernahme in Produktivcode werden sie verworfen oder durch passende Unit-, Contract-, Integrations-, Layout- oder Browsertests abgesichert. PHP- und JavaScript-Coverage werden nicht zu einer gemeinsamen Kennzahl vermischt.

Baseline vom 15. Juli 2026 nach den Raum-API-Vertragstests:

| App | ausführbare Zeilen | abgedeckt | Line-Coverage |
| --- | ---: | ---: | ---: |
| LocalBase | 325 | 301 | 92,62 % |
| OrgSuite | 67 | 57 | 85,07 % |
| AD Kalender | 813 | 271 | 33,33 % |
| AD Planer | 764 | 338 | 44,24 % |
| AD Urlaub | 523 | 146 | 27,92 % |
| AD Raum | 416 | 175 | 42,07 % |
| Gesamt | 2.908 | 1.288 | 44,29 % |

JavaScript ist über Syntax-, Komponenten-, Contract- und Fake-DOM-Smokes abgesichert. Dafür wird noch keine Prozentzahl ausgewiesen: Ein V8-Wert wäre bei den teilweise statischen DOM-/Quellverträgen keine belastbare Aussage über tatsächlich ausgeführte Browserlogik. Browsernahe JS-Line-Coverage bleibt ein eigener Ausbaupunkt und wird nicht mit der PHP-Zahl vermischt.

## Stufe 3: authentifizierte HTTP-Smokes

Das verwendete Konto muss Nextcloud-Admin sein, weil der OrgSuite- und Raum-Smoke auch administrative Schutzgrenzen prüfen.

```bash
AD_SUITE_BASE_URL=https://nextcloud-dev.ddev.site \
AD_SUITE_USER=admin \
AD_SUITE_PASSWORD='…' \
RUN_DDEV_CHECKS=1 \
RUN_HTTP_SMOKES=1 \
scripts/verify-ad-suite-delivery.sh
```

Die HTTP-Smokes prüfen DOM-Verträge, API-Payloads, CSRF-Ablehnung, Adminschutz sowie selbstbereinigende Urlaub- und Raumbuchungsvorgänge.

## Stufe 4: Rechtematrizen

```bash
RUN_DDEV_CHECKS=1 \
RUN_ACCESS_MATRICES=1 \
scripts/verify-ad-suite-delivery.sh
```

Die Rechtematrizen erzeugen temporäre Konten und Gruppenmitgliedschaften für typische Allow-/Deny-Fälle und räumen sie auch bei Fehlern wieder auf. Sie verändern keine vorhandenen Fachdatensätze.

## Releaseentscheidung

Vor einer externen Übergabe müssen mindestens Stufe 1 bis 3 erfolgreich sein. Stufe 4 ist verpflichtend, wenn Organisation, Gruppen, Hierarchie oder Berechtigungen verändert wurden.

Ein erfolgreiches Gate ersetzt nicht:

- die Neuinstallation auf dem Ziel-Staging-System,
- Backup und geprüften Rückbau,
- Datenschutz- und Mitbestimmungsfreigabe,
- eine externe Sicherheitsprüfung,
- die fachliche Abnahme durch die Auftraggeberin.
