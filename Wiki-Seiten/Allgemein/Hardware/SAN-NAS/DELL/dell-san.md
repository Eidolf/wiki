---
title: dell-san
description: 
published: true
date: 2024-01-01T15:50:48.221Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:27:39.835Z
---

# DELL SAN

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-dell-md3000-%2F-md3000-1">DELL MD3000 / MD3000i</span>

Virtuelle Festplatten vergrößern, dies funktioniert nur über die Kommandokonsole.  
**CMD &gt; C:\\Programme(x86)\\DELL\\MD storage manager\\client**

  
Als Befehl eingeben:

```
smcli -n arrayname -p Passwort -c "set virtualdisk [\"virtualdisknametoresize\"] addCapacity=Kapazität_in_Byte";
```

Die Kapazität muss bei neueren Firmwareständen nicht unbedingt in Byte angegeben werden, man kann folgende Werte auch hernehmen (TB,GB,MB), also als Beispiel  
**addCapacity=50 GB";**

Quelle:
http://en.community.dell.com/support-forums/storage/f/1216/t/19307462.aspx
http://support.dell.com/support/edocs/systems/md3000/en/2ndGen/CLI/PDF/CLIMR2g.pdf (SMCLI Dokumentation)