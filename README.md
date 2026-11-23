# Home Assistant – Familie Seelen

Archiv der Home-Assistant-Konfiguration. Privat. Keine Zugangsdaten im Repository (siehe `.gitignore`).

## Aufbau

| Ordner | Inhalt | Ziel in Home Assistant |
|---|---|---|
| `config/` | `configuration.yaml`, `automations.yaml` | `/config/` |
| `config/custom_templates/` | Jinja-Makros (Kostal, Müll, Schule, Vertrag) | `/config/custom_templates/` |
| `config/scripts/` | Skripte | Einstellungen → Skripte (YAML-Modus) |
| `dashboards/` | Dashboards im Raw-Format | Dashboard → ⋮ → Raw-Konfigurationseditor |
| `docs/` | Notizen zu Anlage und Netzwerk | – |

## Regeln

- **Dieses Repository ist die Quelle aller Stände.** Jede Anpassung startet mit dem Stand von hier und endet mit einem Commit hier.
- Dieses Repository ist ein Archiv. Änderungen in Home Assistant landen nicht automatisch hier.
- Vor jedem Neustart: Entwicklerwerkzeuge → YAML → Konfiguration prüfen.
- Passwörter gehören in den Passwortmanager, nicht in Dashboards oder Dateien hier.

## Offene Punkte

- 01.01.2027 Tarifwechsel, alles in einem Schritt:
  - Vertragswechsel läuft automatisch (Automation „Tarif: Vertragswechsel übernehmen“) – nur prüfen, ob Push kam und Werte stimmen
  - Monatsdiagramme (Energie → Monate) auf `sensor.netzbezug_gesamt_gefiltert` / `sensor.einspeisung_gesamt_gefiltert` umstellen
  - „Abschlag empfohlen“ umbauen: Jahresprognose ÷ 12 mit neuem Tarif statt Restbetrag auf Dezember
  - Hinweis: „Winter kWh/Monat“ realistisch ~1.450 (Okt 1.316, Nov ~1.450)
- Bewässerung: Pumpe läuft weiter, wenn die letzte Zone einzeln ausgeschaltet wird → Automation „Pumpe aus, wenn keine Zone an“
- Gute Nacht: Außenlicht-Schritte doppelt zu „Außen an“ → durch `script.aussen_an` ersetzen
- Archiv: Theme Glass Navy sowie Dashboards Tablet und Standorte im Live-Stand fehlen

## Stand

- 18.11.2026: Erstbestand. Energie-Automatik neu (Wallbox/Speicher), Kostal-Störungsmeldung, Statuskarte im Energie-Dashboard. HA-Version 2026.11 (HACS → Marketplace).
- 23.11.2026: Energie-Dashboard, Reiter Live: neue Karte „Energiefluss“ (Kopfzeile mit Status und Autarkie, Power-Flow in Originalfarben, Tageswerte PV/Haus/Netz/Speicher; dunkler Glow-Stil wieder entfernt, im hellen Theme unlesbar). Wallbox heißt dort „Auto“ und zeigt die geladene kWh der Sitzung; Auto-Ladestand fehlt (keine Tesla-Integration).
- 23.11.2026 (2): Energie Live aufgeräumt: vier Kacheln Solar/Haus/Netz/Batterie zeigen je „jetzt“ (Leistung) und „heute“ (Energie); Kopfzeile und Tageswerte-Zeile unter dem Energiefluss entfernt; Strompreis-Kachel in den Abschnitt Strompreis verschoben. Tesla-Anbindung bewusst verworfen.
- 23.11.2026 (3): Statistik-Ausreißer 08.11. 21:00 (Netzbezug +41.078 kWh, Einspeisung) von Hand korrigiert. Neu: gefilterte Zähler Netzbezug/Einspeisung (ignorieren Sprünge nach unten) und Automation „Kostal: Zählersprung melden“ für alle sechs Kostal-Gesamtzähler.
- 23.11.2026 (4): Tarifwechsel mit Datum: `input_datetime.tarifwechsel_ab` + Felder „Neuer Tarif“ (Arbeitspreis, Grundpreis, Abschlag); Automation übernimmt sie am Stichtag in die aktuellen Felder und meldet per Push (holt nach, falls HA aus war).
- 23.11.2026 (5): Tarif-Eingaben umgebaut zu „Aktueller Vertrag“ / „Nächster Vertrag“ (Anbieter, Beginn, Mindestlaufzeit 12/24 Monate, Kündigungsfrist, Preise, Abschlag). Sensoren Vertragsende / Kündigen bis / Tage bis Kündigungsfrist (`custom_templates/vertrag.jinja`), Push 60/30/14/7/1 Tage vor Kündigungsfrist. Wechsel wird auch nachträglich sofort übernommen; Felder „Nächster Vertrag“ werden danach geleert.
- 23.11.2026 (6): Fix Vertrag: Entitäts-IDs heißen `sensor.vertrag_kundigen_bis` / `sensor.vertrag_tage_bis_kundigungsfrist` (HA entfernt Umlaute). Vertragswechsel bei Eingabe nur, wenn Beginn in der Vergangenheit liegt; Prüfung nach der 3-Minuten-Wartezeit wiederholt.
