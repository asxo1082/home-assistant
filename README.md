# Home Assistant – Familie Seelen

Archiv der Home-Assistant-Konfiguration. Privat. Keine Zugangsdaten im Repository (siehe `.gitignore`).

## Aufbau

| Ordner | Inhalt | Ziel in Home Assistant |
|---|---|---|
| `config/` | `configuration.yaml`, `automations.yaml` | `/config/` |
| `config/custom_templates/` | Jinja-Makros (Kostal, Müll, Schule) | `/config/custom_templates/` |
| `config/scripts/` | Skripte | Einstellungen → Skripte (YAML-Modus) |
| `dashboards/` | Dashboards im Raw-Format | Dashboard → ⋮ → Raw-Konfigurationseditor |
| `docs/` | Notizen zu Anlage und Netzwerk | – |

## Regeln

- **Dieses Repository ist die Quelle aller Stände.** Jede Anpassung startet mit dem Stand von hier und endet mit einem Commit hier.
- Dieses Repository ist ein Archiv. Änderungen in Home Assistant landen nicht automatisch hier.
- Vor jedem Neustart: Entwicklerwerkzeuge → YAML → Konfiguration prüfen.
- Passwörter gehören in den Passwortmanager, nicht in Dashboards oder Dateien hier.

## Offene Punkte

- 01.01.2027: Monatsdiagramme (Energie → Monate) von `plenticoreseelen_home_consumption_from_grid_total` / `_energy_to_grid_total` auf `sensor.netzbezug_gesamt_gefiltert` / `sensor.einspeisung_gesamt_gefiltert` umstellen
- Bewässerung: Pumpe läuft weiter, wenn die letzte Zone einzeln ausgeschaltet wird → Automation „Pumpe aus, wenn keine Zone an“
- Gute Nacht: Außenlicht-Schritte doppelt zu „Außen an“ → durch `script.aussen_an` ersetzen
- Archiv: Theme Glass Navy sowie Dashboards Tablet und Standorte im Live-Stand fehlen

## Stand

- 18.11.2026: Erstbestand. Energie-Automatik neu (Wallbox/Speicher), Kostal-Störungsmeldung, Statuskarte im Energie-Dashboard. HA-Version 2026.11 (HACS → Marketplace).
- 23.11.2026: Energie-Dashboard, Reiter Live: neue Karte „Energiefluss“ (Kopfzeile mit Status und Autarkie, Power-Flow in Originalfarben, Tageswerte PV/Haus/Netz/Speicher; dunkler Glow-Stil wieder entfernt, im hellen Theme unlesbar). Wallbox heißt dort „Auto“ und zeigt die geladene kWh der Sitzung; Auto-Ladestand fehlt (keine Tesla-Integration).
- 23.11.2026 (2): Energie Live aufgeräumt: vier Kacheln Solar/Haus/Netz/Batterie zeigen je „jetzt“ (Leistung) und „heute“ (Energie); Kopfzeile und Tageswerte-Zeile unter dem Energiefluss entfernt; Strompreis-Kachel in den Abschnitt Strompreis verschoben. Tesla-Anbindung bewusst verworfen.
- 23.11.2026 (3): Statistik-Ausreißer 08.11. 21:00 (Netzbezug +41.078 kWh, Einspeisung) von Hand korrigiert. Neu: gefilterte Zähler Netzbezug/Einspeisung (ignorieren Sprünge nach unten) und Automation „Kostal: Zählersprung melden“ für alle sechs Kostal-Gesamtzähler.
