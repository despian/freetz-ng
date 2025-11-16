# Addhole (for dnsmasq)
  - Package: [master/make/pkgs/addhole/](https://github.com/Freetz-NG/freetz-ng/tree/master/make/pkgs/addhole/)
  - Maintainer: [@fda77](https://github.com/fda77)

Addhole ist eine Erweiterung für Dnsmasq.<br>

[![screenshot](../screenshots/000-PKG_addhole_md.png)](../screenshots/000-PKG_addhole.png)

 - Mit Addhole können Listen mit Hostnamen in Dnsmasq eingebunden werdend die blockiert werden. Es gibt Listen mit Hosts die Werbung, Malware, Viren etc verbreiten.
   Das Package ist vergleichbar mit Pi-Hole, nur ohne Webinterface mit den Zugriffsstatistiken und ohne ein zusätzliches Gerät.

 - In der Standardkonfiguration von Addhole benötig Dnsmasq etwa 10MB Ram, werden alle vordefinierten Listen aktiviert 25MB Ram.
   Hinweis: Dies macht keinen Sinn auf einer Fritzbox mit 32 MB Ram!
   Die Listen können automatisch mit Cron aktualisert werden. Es könne zusätzlich eigene Hosts angegeben werden

 - Vorsicht: Die Namensauflösung wird auf dem Server (Dnsmasq), dem Client (Linux, Windows, Android, ...) und im Anwendungsprogramm (zb Webbrowser) gecacht!
   Falls sich nicht sofort eine Änderung zeigt, alle Caches löschen - oder einfach alles rebooten...

## Dual-Stack Blockierung / Dual-Stack Blocking

Seit Version 1.1 kann Addhole gleichzeitig A- und AAAA-Anfragen blockieren.
Standard: Nur IPv4 (ADDHOLE_SINK_IP4 = 0.0.0.0). Setzt man zusätzlich `ADDHOLE_SINK_IP6='::'`, werden auch AAAA-Einträge erzeugt.
Leer lassen (`ADDHOLE_SINK_IP6=''`) deaktiviert die AAAA-Blockierung. Alte Konfigurationen mit `ADDHOLE_SINK` werden automatisch zu `ADDHOLE_SINK_IP4` migriert.
Beachten: Aktiviert man beide, verdoppelt sich ungefähr die Größe der generierten Host-Datei (RAM-Verbrauch steigt entsprechend).

Since version 1.1 Addhole can block both A and AAAA queries.
Default: Only IPv4 (ADDHOLE_SINK_IP4 = 0.0.0.0). Set `ADDHOLE_SINK_IP6='::'` (or another IPv6) to also generate AAAA blocking entries.
Leave empty (`ADDHOLE_SINK_IP6=''`) to disable AAAA blocking. Legacy single variable `ADDHOLE_SINK` is auto-migrated to `ADDHOLE_SINK_IP4`.
Note: Enabling both roughly doubles the host list size and memory usage.

## CNAME-Modus / CNAME Mode

Im CNAME-Modus wird nur eine Zeile pro blockierter Domain erzeugt: `cname=domain,addhole-sink.invalid` plus wenige `address=` Zeilen für den gemeinsamen Sink-Host.
Das reduziert Speicherbedarf und Dateigröße gegenüber dem Hosts-Modus (kein doppelter Eintrag für A/AAAA).
Konfiguration: `ADDHOLE_MODE='cname'`, `ADDHOLE_SINK_HOST`, `ADDHOLE_SINK_IP4`, optional `ADDHOLE_SINK_IP6`. Leerer IPv6-Wert deaktiviert AAAA.
Hinweis: Ein zusätzlicher CNAME-Resolution-Schritt ist erforderlich, aber im LAN vernachlässigbar.

In CNAME mode only one line per blocked domain is generated: `cname=domain,addhole-sink.invalid` plus a couple of `address=` lines for the shared sink host.
This lowers memory/file size versus hosts mode (no duplicate A/AAAA lines).
Configure via `ADDHOLE_MODE='cname'`, `ADDHOLE_SINK_HOST`, `ADDHOLE_SINK_IP4`, optionally `ADDHOLE_SINK_IP6` (empty disables AAAA). Minor extra CNAME lookup latency is negligible locally.

