# Fernzugriff über DuckDNS (statt Nabu Casa)

Stand: 03.11.2026 · läuft

## Kette
Internet → FritzBox 6890 (Telekom, öffentliche IPv4) → USG (WAN 192.168.178.21) → HA 192.168.1.77 → NGINX-Add-on (443) → HA (8123)

- Extern: https://ha-asxo.duckdns.org
- Intern: http://192.168.1.77:8123 (alternativ http://homeassistant.local:8123)
- Zertifikat: Let's Encrypt über Duck-DNS-Add-on (DNS-Challenge, kein Port 80 nötig), liegt in /ssl/fullchain.pem + privkey.pem

## Konfiguration
- **Duck DNS Add-on:** domains ha-asxo.duckdns.org, lets_encrypt accept_terms true, seconds 300, Start beim Booten an
- **NGINX Home Assistant SSL proxy:** domain ha-asxo.duckdns.org, client_max_body_size_megabytes 64, sonst Standard; Port 443
- **HA HTTP-Server** (seit HA 2026.8 nur noch per UI: Einstellungen → System → Netzwerk → HTTP-Server, NICHT mehr per `http:` in configuration.yaml):
  - Port 8123
  - X-Forwarded-For vertrauen: an
  - Vertrauenswürdige Proxys: 172.30.33.0/24
  - IP-Sperre an, 5 Versuche (Sperren in ip_bans.yaml löschen)
- **HA-URLs:** Einstellungen → System → Netzwerk → Internet = https://ha-asxo.duckdns.org (nicht Cloud)
- **Companion-App:** externe URL auf DuckDNS umgestellt
- **FritzBox:** Portfreigabe „HA HTTPS“ TCP 443 → USGFirewall 192.168.178.21:443 (nur IPv4)
- **USG (UniFi Controller 9.0.114):** Routing → Port Forwarding „HA HTTPS“ WAN 443 → 192.168.1.77:443 TCP

## Backups (Ersatz für Nabu-Casa-Cloud-Backup)
- OneDrive-Integration (privates Microsoft-Konto), Ordner in OneDrive: **Eigene Dateien → Backup_Home** (je Backup eine .tar + .metadata.json, beide nötig)
- HA wählt die Uhrzeit bei „täglich“ selbst (ca. 04:45–05:20)
- Plan: täglich, 7 behalten; Speicherorte „Dieses System“ + OneDrive
- OneDrive: „Dateien endgültig löschen“ aus (30 Tage Papierkorb)
- Backup vor App-Updates aktiv
- Verschlüsselungsschlüssel im Passwortmanager ablegen
- Automation „Backup fehlgeschlagen“ auf event.backup_automatic_backup (event_type == failed)
- Updates: keine Auto-Updates für Core/OS

## Stolpersteine
- 400 Bad Request = Proxy nicht vertraut → HTTP-Server-Einstellungen prüfen
- Externe Adresse aus dem eigenen WLAN funktioniert trotz doppeltem NAT. Falls das nach Router-Updates kippt: NAT-Loopback bzw. lokalen DNS-Eintrag auf der USG prüfen
- Bei LTE-Fallback der FritzBox kein Fernzugriff
- Änderungen an der USG nur bei laufendem UniFi Controller (Raspberry 192.168.1.15)

## Offen (Stand 03.11.2026)
- OneDrive-Ordner Backup_Home ist als „Freigegeben“ markiert – prüfen, wer Zugriff hat
- Nabu Casa abschalten und kündigen
- 2FA für alle Benutzer
- Feste IPs: HA .77, Controller-Raspberry .15
- Controller-Backup (.unf) sichern; neue SD-Karte oder Umzug ins HA-Add-on
