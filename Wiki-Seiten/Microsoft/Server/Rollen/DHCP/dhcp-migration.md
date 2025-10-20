---
title: dhcp-migration
description: 
published: true
date: 2023-12-31T13:32:07.332Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:32:04.308Z
---

# DHCP Migration

# <span class="mw-headline" id="bkmrk-server-2003-zu-serve-1">Server 2003 zu Server 2008</span>

## <span class="mw-headline" id="bkmrk-ablauf-1">Ablauf</span>

<div class="vector-body" id="bkmrk-eingabe-server-2003%3A"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Eingabe Server 2003: " netsh dhcp server export c:\\dateiname " <dl><dd>Exportiert die DHCP konfiguration in eine Datei unter C: mit dem entsprechenden Dateinamen</dd><dd>Im Fehlerfall bei einem Import mit der entsprechenden Datei müssen die Bereiche des DHCP´s direkt angegeben werden, mit z.B. dem Befehl " c:\\dateiname 192.168.1.0 172.16.31.0 " werden nur die Bereiche **192.168.1.0** und **172.16.31.0** in die Datei ausgegeben.</dd></dl>
2. Eingabe Server 2003: " net stop dhcp-server "
3. Eingabe Server 2008: " netsh dhcp server import c:\\dateiname "

</div></div></div>## <span class="mw-headline" id="bkmrk-fehlermeldung-1">Fehlermeldung</span>

z.B. Fehler beim Importieren von Option "6"

<div class="vector-body" id="bkmrk-l%C3%B6sung%3A-im-dhcp-die-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Lösung: Im DHCP die Serveroption (Option "6" = DNS) haken herausnehmen

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.eggheadcafe.com/microsoft/Windows-Server/35630160/dhcp-export--import-unter-w2k8.aspx" rel="nofollow">http://www.eggheadcafe.com/microsoft/Windows-Server/35630160/dhcp-export--import-unter-w2k8.aspx</a>
```

# <span class="mw-headline" id="bkmrk-server-2008-zu-serve-1">Server 2008 zu Server 2012</span>

## <span class="mw-headline" id="bkmrk-ablauf-3">Ablauf</span>

<div class="vector-body" id="bkmrk-dhcp-rolle-am-zielse"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. DHCP Rolle am Zielserver installieren, somit wird das Powershell CMDLets freigeschaltet
2. PowerShell am Server 2012 öffnen
3. Export-DhcpServer –ComputerName win2k8r2-dhcp.corp.contoso.com -Leases -File C:\\export\\dhcpexp.xml -verbose
4. Import-DhcpServer –ComputerName DHCP1.corp.contoso.com -Leases –File C:\\export\\dhcpexp.xml -BackupPath C:\\dhcp\\backup\\ -Verbose
5. DHCP Rolle am alten Server deaktivieren oder entfernen

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="http://blogs.technet.com/b/teamdhcp/archive/2012/09/11/migrating-existing-dhcp-server-deployment-to-windows-server-2012-dhcp-failover.aspx" rel="nofollow">http://blogs.technet.com/b/teamdhcp/archive/2012/09/11/migrating-existing-dhcp-server-deployment-to-windows-server-2012-dhcp-failover.aspx</a>
```