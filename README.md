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

- Bewässerung: Pumpe läuft weiter, wenn die letzte Zone einzeln ausgeschaltet wird → Automation „Pumpe aus, wenn keine Zone an“
- Gute Nacht: Außenlicht-Schritte doppelt zu „Außen an“ → durch `script.aussen_an` ersetzen
- Speicher: Rückfall des Kostal in die interne Regelung nach „Automatik Aus“ noch nicht getestet
- Archiv: Theme Glass Navy sowie Dashboards Tablet und Standorte im Live-Stand fehlen

## Stand

- 18.11.2026: Erstbestand. Energie-Automatik neu (Wallbox/Speicher), Kostal-Störungsmeldung, Statuskarte im Energie-Dashboard. HA-Version 2026.11 (HACS → Marketplace).
