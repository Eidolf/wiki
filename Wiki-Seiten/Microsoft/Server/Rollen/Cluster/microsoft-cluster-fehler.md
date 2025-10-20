---
title: microsoft-cluster-fehler
description: 
published: true
date: 2023-12-31T14:31:12.566Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:31:09.477Z
---

# Microsoft Cluster Fehler

# <span class="mw-headline" id="bkmrk-cluster-verbindungsf-1">Cluster Verbindungsfehler</span>

## <span class="mw-headline" id="bkmrk-beschreibung%3A-1">Beschreibung:</span>

Einzelne Clusterknote können keine Verbindung zum Cluster aufbauen  
  
Fehler in der Ereignisanzeige:

<div class="vector-body" id="bkmrk-microsoft-windows-fa"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- microsoft-windows-failoverclustering 1572

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://social.technet.microsoft.com/wiki/contents/articles/3829.failover-cluster-node-state-is-down-and-cluster-service-terminates-or-adding-a-new-failover-cluster-node-fails-with-time-out-error.aspx" rel="nofollow">http://social.technet.microsoft.com/wiki/contents/articles/3829.failover-cluster-node-state-is-down-and-cluster-service-terminates-or-adding-a-new-failover-cluster-node-fails-with-time-out-error.aspx</a>
<a class="external free" href="http://blogs.technet.com/b/askpfeplat/archive/2012/01/09/failover-cluster-communication-failures.aspx" rel="nofollow">http://blogs.technet.com/b/askpfeplat/archive/2012/01/09/failover-cluster-communication-failures.aspx</a>
```

# <span class="mw-headline" id="bkmrk-error-0x800703eb-whe-1">Error 0x800703eb when creating File Server role</span>

Bei einem Tier0 Modell kann es sein das man umständlich Clusterobjekte bei einem neuen Microsoft Cluster erstellen muss.  
Falls danach die Datei Server Rolle installiert wird und das erstellen des Netzwerknamens fehlschlägt muss man der Übergeordneten OU Berechtigungen erteilen.

Auf der OU bei dem das Cluster Server Objekt vorhanden ist muss man folgende Schritte durchführen.

<div class="vector-body" id="bkmrk-rechtsklick-auf-die-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Rechtsklick auf die OU &gt; Eigenschaften &gt; Sicherheit &gt; Erweitert
2. Computer Objekte markieren und danach nach dem Cluster Server Objekt suchen
3. Diesem folgende Rechte erteilen "Computer Objekte erstellen" und "Alle Eigenschaften lesen"
4. Danach sollte das Cluster Rollen Objekt erstellt werden können

</div></div></div>Englische Version im Quell Link

## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://fearthepanda.com/failovercluster/2018/09/16/FailoverCluster-Error-0x800703eb-When-Creating-File-Server-Role/" rel="nofollow">https://fearthepanda.com/failovercluster/2018/09/16/FailoverCluster-Error-0x800703eb-When-Creating-File-Server-Role/</a>
```