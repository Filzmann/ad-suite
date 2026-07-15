# AD Suite für Nextcloud

Die AD Suite bündelt sechs eigenständige Nextcloud-Apps für Dienst-, Assistenz-, Urlaubs- und Raumplanung unter einer gemeinsamen Navigation und Organisationskonfiguration.

> Status: Release Candidate für ein kontrolliertes Staging auf Nextcloud 34 mit PHP ab 8.3. Vor einem produktiven Einsatz sind Neuinstallationstest, Datenschutz- und Mitbestimmungsprüfung, Sicherheitsreview und fachliche Abnahme erforderlich.

## Bestandteile

| App | Aufgabe | Quellcode |
| --- | --- | --- |
| LocalBase | Gemeinsame Organisations-, Rechte-, Kalender- und UI-Verträge | [nextcloud-localbase](https://github.com/Filzmann/nextcloud-localbase) |
| OrgSuite | Gemeinsame AD-/BR-Navigation und app-übergreifende Administration | [nextcloud-orgsuite](https://github.com/Filzmann/nextcloud-orgsuite) |
| AD Kalender | Dienste, Termine, Sperrtermine, Personensuche und Meetinglücken | [nextcloud-adcalendar](https://github.com/Filzmann/nextcloud-adcalendar) |
| Assistenzplanung | Monatliche Wunschdienstplanung für dynamische Assistenzteams | [nextcloud-adplaner](https://github.com/Filzmann/nextcloud-adplaner) |
| AD Urlaub | Geplante und genehmigte Urlaube mit Rechte- und Konfliktprüfung | [nextcloud-adurlaub](https://github.com/Filzmann/nextcloud-adurlaub) |
| AD Raumplaner | Zeitlich ausgerichtete, kollisionsfreie Raumbuchungen | [nextcloud-adroom](https://github.com/Filzmann/nextcloud-adroom) |

Die Apps bleiben fachlich und technisch getrennt. Navigation erteilt keine Rechte; schreibende und lesende Zugriffskontrollen werden serverseitig in den jeweiligen Apps durchgesetzt.

## Installation und Abnahme

- [Staging-Installation](docs/INSTALLATION.md)
- [Betrieb und Rückbau](docs/OPERATIONS.md)
- [Abnahmeprotokoll](docs/ACCEPTANCE.md)
- [Delivery-Gate und Testabdeckung](docs/DELIVERY-GATE.md)

Die fertigen App-Archive, das Suite-Bundle, das Versionsmanifest und SHA-256-Prüfsummen werden gemeinsam im jeweiligen GitHub-Release bereitgestellt.

## Lizenz und Leistungen

Der Quellcode der Apps steht unter der GNU Affero General Public License v3.0. Installation, Einführung, Anpassung, Schulung, Wartung und Support können unabhängig davon als nicht exklusive Dienstleistungen vereinbart werden.

Fehler und Funktionswünsche gehören in das jeweils betroffene App-Repository. Sicherheitsrelevante Hinweise bitte nicht öffentlich mit personenbezogenen Daten dokumentieren, sondern über die privaten Security-Advisories des betroffenen GitHub-Repositories melden.
