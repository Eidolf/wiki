# Hyper-V Cluster Netzwerkkonfiguration

# <span class="mw-headline" id="bkmrk-hyper-v-cluster-und--1">Hyper-V Cluster und Netzwerkkarten Bindungen</span>

## <span class="mw-headline" id="bkmrk-management-netzwerk-1">Management-Netzwerk</span>

Über das Management-Netzwerk wird mit den Hyper-V Hosts kommuniziert. Hierzu gehört unteranderem Dateizugriff, WMI-Zugriffe, das Backup und Kommunikation der Systemcenter Agenten. Dieses Netz hat deshalb sowohl IP wie auch den “Client für Microsoft Netzwerk” und auch die “Datei und Druckfreigabe” gebunden.

## <span class="mw-headline" id="bkmrk-csv--oder-heartbeat--1">CSV- oder Heartbeat-Netzwerk</span>

Über dieses Netz stellt der Cluster einerseits fest, ob es den anderen Cluster Knoten gut geht (Heartbeat) und andererseits wird hierrüber bei Ausfall einer Storageanbindung eines Host der Traffic zum Storage über einen anderen Host umgeleitet. Ebenso wird dieses Netz benutzt, wenn z.B. beim Backup eines CSV Volumes über den Koordinator-Node exclusive auf den Storage zugegriffen wird. Dieses Netz muss deshalb sowohl IP, wie auch den “Client für Microsoft Netzwerk” und auch die “Datei und Druckfreigabe” gebunden haben.

## <span class="mw-headline" id="bkmrk-vm-netzwerk-1">VM-Netzwerk</span>

Über das VM-Netzwerk kommunizieren die virtuellen Maschinen mit ihrem “normalen” Netzwerk. Im Hyper-V sollten diese Karten nichts gebunden haben, außer das ”Protokoll für Microsoft virtuelle Netzwerk-Switch”.

## <span class="mw-headline" id="bkmrk-livemigrations-netzw-1">Livemigrations-Netzwerk</span>

Über dieses Netz wird bei der Livemigration der Speicher der VM von einem Cluster Knoten auf den anderen übertragen. Da die VM weiterläuft und dadurch noch während der ersten Übertragung des Speichers schon wieder Veränderungen an dem Speicher vorkommen, sollte dieses Netz sehr performant angebunden sein. In der Beispielkonfiguration ist sowohl IP wie auch die “Client für Microsoft Netzwerk” und auch die “Datei und Druckfreigabe” gebunden. Die letzten beiden sind aus gründen der Redundanz für das CSV Netz vorhanden und können auch wegfallen.

## <span class="mw-headline" id="bkmrk-iscsi-netzwerk-1">iSCSI-Netzwerk</span>

Das letzte Netzwerk ist die iSCSI Anbindung. Sie dient als Anbindung der Cluster Knoten an ein vorhandenes iSCSI Storage System. Hier ist nur die IP Bindung notwendig.

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.hyper-v-server.de/management/hyper-v-cluster-und-netzwerkkarten-bindungen/" rel="nofollow">http://www.hyper-v-server.de/management/hyper-v-cluster-und-netzwerkkarten-bindungen/</a>
```