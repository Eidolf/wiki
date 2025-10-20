---
title: hyper-v-clusterlaufwerk-verschieben
description: 
published: true
date: 2023-12-31T14:34:35.054Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:34:31.958Z
---

# Hyper-V Clusterlaufwerk verschieben

# <span class="mw-headline" id="bkmrk-server-2008-1">Server 2008</span>

Bei einem Server 2008 Hyper-V Cluster ist es nicht möglich eine nicht zugeordnete LUN auf einen andern Hyper-V Knoten über die **Failoverclusterverwaltung** zu verschieben. Es funktioniert nur über folgenden Konsolenbefehl:

  
**" cluster group "Clusterlaufwerksname" /move "**

  
Beispiel: Den Freien Speicher verschieben

```
cluster group "available storage" /move
```

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://social.technet.microsoft.com/Forums/en-US/winserverClustering/thread/77b5514f-5514-4be6-a5ce-f5ecb0463542/" rel="nofollow">http://social.technet.microsoft.com/Forums/en-US/winserverClustering/thread/77b5514f-5514-4be6-a5ce-f5ecb0463542/</a>
```

# <span class="mw-headline" id="bkmrk-server-2008r2-1">Server 2008R2</span>

Bei Server 2008R2 kann man das Clusterlaufwerk mit dem Server 2008 Konsolenbefehl und über die **Failoverclusterverwaltung** verschieben