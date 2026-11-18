# Entitäten – Nachschlagewerk

Quelle: Entwicklerwerkzeuge → Zustände. Nur feste Angaben (IDs, Namen, Bereiche, Fähigkeiten); keine Momentwerte.

## Stand 18.11.2026 (Auszug, Ende der Liste ab `update.`)

| Entität | Name | Bereich | Hinweise |
|---|---|---|---|
| `update.usg_firewall_firmware` | USG Firewall Firmware | HWR | Firmware 4.4.57.5578372, aktuell; Auto-Update aus |
| `update.waste_collection_schedule_update` | Waste Collection Schedule | – | v2.36.0, aktuell; kommt über den Marketplace (ehem. HACS) |
| `vacuum.roborock_q7_max` | Roborock Q7 Max | Reinigung | Erdgeschoss; Saugstufen: quiet, balanced, turbo, max, gentle |
| `vacuum.robot` | Robot | Reinigung | Obergeschoss, im Dashboard „TIKOM“; Saugstufen: gentle, normal, strong |
| `weather.forecast_home` | Home | – | Wetterdienst met.no (Norwegisches Meteorologisches Institut) |
| `zone.home` | Home | – | Radius 100 m; Koordinaten bewusst nicht im Repository |

## Auffälligkeiten

- `zone.home` listet zwei Tracker für die Firewall: `device_tracker.usg_firewall` und `device_tracker.usgfirewall` – vermutlich ein Überbleibsel, prüfen und einen entfernen.
- `vacuum.robot` heißt in Home Assistant nur „Robot“; zur Klarheit in „TIKOM Obergeschoss“ umbenennen (Name, nicht Entitäts-ID, damit Dashboards weiter funktionieren).
- Sonos-Tracker `device_tracker.sonoszp` bis `_7`: sieben Einträge, im Sonos-Dashboard sind sieben Lautsprecher – passt.
