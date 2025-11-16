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

