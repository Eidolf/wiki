# SCCM Servicepoint Cache Zeit ändern

<div class="vector-body" id="bkmrk-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"></div></div>Der PXE Servicepoint (WDS) unter SCCM speichert(e) Anfragen von Clients eine Stunde lang zwischen, was zu Problemen bei OS Deployments führt(e).

Hier sollte folgender Wert auf z.B. 2min geändert werden (Wert: 120)

```
HKLM\Software\Microsoft\SMS\PXE\CacheExpire
```

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.mssccmfaq.de/2010/03/02/wds-pxe-servicepoint-cacheexpire/" rel="nofollow">http://www.mssccmfaq.de/2010/03/02/wds-pxe-servicepoint-cacheexpire/</a>
```