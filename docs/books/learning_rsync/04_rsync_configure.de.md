---
title: rsync-Konfigurationsdatei
author: tianci li
update: 2021-11-04
---

# /etc/rsyncd.conf

Im vorherigen Artikel [rsync demo 02](03_rsync_demo02.md) haben wir einige grundlegende Parameter vorgestellt. Dieser Artikel beschreibt zusätzliche Parameter.

| Parameter                           | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fake super = yes                    | 'yes' bedeutet, dass der Daemon nicht als Root ausgeführt werden muss, um die vollständigen Attribute der Datei zu speichern.                                                                                                                                                                                                                                                                                                       |
| uid =                               | Benutzer-ID                                                                                                                                                                                                                                                                                                                                                                                                                         |
| gid =                               | Zwei Parameter werden verwendet, um den Benutzer und die Gruppe anzugeben, die zum Übertragen von Dateien verwendet werden, wenn der rsync-Daemon als Root ausgeführt wird. Die Standardeinstellung ist nobody                                                                                                                                                                                                                      |
| use chroot = yes                    | Wenn das Root-Verzeichnis vor der Übertragung gesperrt werden muss, ja ja, nein nein. Um die Sicherheit zu erhöhen, ist rsync standardmäßig auf „yes“ eingestellt.                                                                                                                                                                                                                                                                  |
| max connections = 4                 | Die maximal zulässige Anzahl von Verbindungen, der Standardwert ist 0, d. h. es gibt keine Einschränkungen                                                                                                                                                                                                                                                                                                                          |
| lock file = /var/run/rsyncd.lock    | Die angegebene Sperrdatei, die dem Parameter „max connection“ zugeordnet ist                                                                                                                                                                                                                                                                                                                                                        |
| exclude = lost+found/               | Verzeichnisse ausschließen, die nicht verschoben werden müssen                                                                                                                                                                                                                                                                                                                                                                      |
| transfer logging = yes              | Legt fest, ob das FTP-ähnliche Protokollformat zum Aufzeichnen von Rsync-Uploads und -Downloads aktiviert werden soll                                                                                                                                                                                                                                                                                                               |
| timeout = 900                       | Legt die Timeout-Zeit fest. Werden innerhalb der angegebenen Zeit keine Daten übertragen, bricht rsync direkt ab. Die Einheit ist Sekunden, der Standardwert 0 bedeutet, dass es niemals zu einem Timeout kommt                                                                                                                                                                                                                     |
| ignore nonreadable = yes            | Legt fest, ob Dateien ignoriert werden sollen, für die der Benutzer keine Zugriffsrechte hat                                                                                                                                                                                                                                                                                                                                        |
| motd file = /etc/rsyncd/rsyncd.motd | Wird verwendet, um den Pfad zur Nachrichtendatei anzugeben. Standardmäßig gibt es keine motd-Datei. Dies ist die Willkommensnachricht, die beim Anmelden eines Benutzers angezeigt wird.                                                                                                                                                                                                                                            |
| hosts allow = 10.1.1.1/24           | Wird verwendet, um anzugeben, auf welche IP-Adresse oder auf welches Netzwerksegment Clients zugreifen dürfen. Sie können IP-Adresse, Netzwerksegment, Hostname und Host unter domain eingeben und diese durch Leerzeichen trennen. Erlaubt standardmäßig jedem den Zugriff                                                                                                                                                         |
| hosts deny = 10.1.1.20              | Der Zugriff auf die vom Benutzer angegebene IP-Adresse oder welches Netzwerksegment ist untersagt. Wenn Hosts, die zulassen, und Hosts, die dies verweigern, das gleiche übereinstimmende Ergebnis haben, kann der Client letztendlich nicht zugreifen. Wenn die Adresse des Clients weder in „hostsallow“ noch in „hostsdeny“ enthalten ist, wird dem Client der Zugriff gestattet. Standardmäßig gibt es keinen solchen Parameter |
| auth users = li                     | Virtuelle Benutzer aktivieren, mehrere Benutzer werden durch Kommas getrennt                                                                                                                                                                                                                                                                                                                                                        |
| syslog facility = daemon            | Bestimmt den Systemprotokolllevel. Diese Werte können festgelegt werden: auth, authpriv, cron, daemon, ftp, kern, lpr, mail, news, security, syslog, user, uucp, local0, local1, local2 local3, local4, local5, local6 und local7. Das Default-Wert ist daemon                                                                                                                                                                      |

## Empfohlene Konfiguration

```ini title="/etc/rsyncd.conf"
uid = nobody
gid = nobody
address = 192.168.100.4
use chroot = yes
max connections = 10
syslog facility = daemon
pid file = /var/run/rsyncd.pid
log file = /var/log/rsyncd.log
lock file = /var/run/rsyncd.lock
[file]
  comment = rsync
  path = /rsync/
  read only = no
  dont compress = *.gz *.bz2 *.zip
  auth users = li
  secrets file = /etc/rsyncd users.db
```
