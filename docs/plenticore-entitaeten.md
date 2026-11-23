# Kostal Plenticore – Entitäten-Zuordnung

Stand: 28.10.2026 · Gerät „PlenticoreSeelen“ · Bereich „Energie“

## Energie-Dashboard (Einstellungen → Dashboards → Energie)

| Feld | Entität |
|---|---|
| Netzbezug | `sensor.plenticoreseelen_home_consumption_from_grid_total` (Netzbezug) |
| Netzeinspeisung | `sensor.plenticoreseelen_energy_to_grid_total` (Einspeisung) |
| Solarproduktion | `sensor.plenticoreseelen_energy_yield_total` (PV-Erzeugung) |
| Batterie – Energie hinein | `sensor.plenticoreseelen_battery_charge_from_pv_total` **und** `sensor.plenticoreseelen_battery_charge_from_grid_total` |
| Batterie – Energie heraus | `sensor.plenticoreseelen_battery_discharge_total` |

`energy_discharge_to_grid_total` („Batterie ins Netz“) ist bereits in der Einspeisung enthalten – nicht zusätzlich eintragen.

## Live-Leistung (W) – Power-Flow, Templates

| Zweck | Entität |
|---|---|
| PV gesamt (DC) | `sensor.plenticoreseelen_sum_power_of_all_pv_dc_inputs` |
| PV-String 1 / 2 | `sensor.plenticoreseelen_dc1_power` / `_dc2_power` |
| Netz (+ Bezug / − Einspeisung) | `sensor.plenticoreseelen_grid_power` |
| Hausverbrauch | `sensor.plenticoreseelen_home_power` |
| Haus aus PV / Batterie / Netz | `_home_power_from_pv` / `_home_power_from_battery` / `_home_power_from_grid` |
| Batterie (+ Entladen / − Laden) | `sensor.plenticoreseelen_battery_power` |
| PV → Batterie | `sensor.plenticoreseelen_pv_to_battery_power` |
| AC-Ausgang Wechselrichter | `sensor.plenticoreseelen_ac_power` |
| Speicher-Ladestand | `sensor.plenticoreseelen_battery_soc` |

## Tages-/Monats-/Jahreswerte (Karten, Statistik)

- Ertrag: `energy_yield_day / _month / _year / _total`
- Hausverbrauch: `home_consumption_day / _month / _year / _total`, aufgeteilt nach `_from_pv_`, `_from_battery_`, `_from_grid_`
- Einspeisung: `energy_to_grid_day / _month / _year / _total`
- Batterie: `battery_charge_from_pv_*`, `battery_charge_from_grid_*`, `battery_discharge_*`, `energy_discharge_to_grid_*`
- PV-Strings: `energy_pv1_*`, `energy_pv2_*` (`energy_pv3_*` immer 0 – Eingang nicht belegt)
- Quoten: `autarky_day / _month / _year / _total`, `own_consumption_rate_day / _month / _year / _total`

## Status & Steuerung

- `sensor.plenticoreseelen_inverter_state` – Wechselrichterstatus (z. B. Off nachts)
- `sensor.plenticoreseelen_energy_manager_state` – z. B. „Winter Mode Step 1“
- `sensor.plenticoreseelen_battery_cycles` – Batteriezyklen
- `select.plenticoreseelen_battery_charging_usage_mode` – None / SmartBatteryControl / TimeControl
- `switch.plenticoreseelen_battery_strategy` – Batteriestrategie

## Modbus-Steuerung Speicher (Register 1034)

- Sollwert Lade-/Entladeleistung, float32, Wortreihenfolge getauscht, negativ = laden
- Werte im Repository: `config/automations.yaml` → „Speicher: Steuerung Kostal“
- 0 W = halten; ohne neue Befehle fällt der Kostal nach seiner Rückfallzeit in die interne Regelung – bestätigt am 18.11.2026: nach Ende des Autoladens mit aktivem Entladeschutz entlud die Batterie wieder normal ins Haus
- „Ausgleichsladung“ (Kalibrierung, lädt notfalls aus dem Netz) macht der Kostal selbst und ignoriert externe Befehle – beobachtet am 18.11.2026 bei 13 % Ladestand

## Störungserkennung

- `binary_sensor.kostal_storung`, Logik in `config/custom_templates/kostal.jinja`
- Push über Automation „Kostal: Störungsmeldung“, Statuskarte im Energie-Dashboard (Reiter Live)

## Zählersprünge (Statistik)

- 08.11.2026 21:00: Kostal meldete kurz einen zu kleinen Gesamtzählerstand. HA wertete das als Zählertausch und addierte den kompletten Stand (Netzbezug +41.078,60 kWh, Einspeisung ~17.500 kWh). Korrigiert am 23.11. über Entwicklerwerkzeuge → Statistiken → Summe anpassen.
- Schutz: `sensor.netzbezug_gesamt_gefiltert` und `sensor.einspeisung_gesamt_gefiltert` (übernehmen nur Werte > 0, nie kleiner als der letzte Stand). Monatsdiagramme ab 01.01.2027 darauf umstellen.
- Meldung: Automation „Kostal: Zählersprung melden“ schickt eine Push-Nachricht, sobald einer der sechs Gesamtzähler kleiner wird oder auf 0 fällt.

