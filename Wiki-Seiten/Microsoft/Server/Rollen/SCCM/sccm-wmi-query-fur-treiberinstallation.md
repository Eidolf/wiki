# SCCM WMI Query für Treiberinstallation

# <span class="mw-headline" id="bkmrk-am-client-1">Am Client</span>

Wenn man einem bestimmten Computer ein Paket zuordnen will z.B. ein Treiberpaket. Muss man eine WMI Abfrage ausführen die den PC als eindeutig makiert. Die einfachste möglichkeite ist es über das Computermodell zu deklarieren, hier kann aber sein das bei vielen Mainboards kein Name übertragen wird (praktisch wenn man Gruppenzugehörig aufsetzen will, z.B. alle DELL D430). Um es eindeutig zu haben sollte man die UUID des Rechners benutzen.

<div class="vector-body" id="bkmrk-computermodell"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Computermodell

</div></div></div>```
WMIC ComputerSystem GET Model
```

<div class="vector-body" id="bkmrk-uuid"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- UUID

</div></div></div>```
WMIC CSProduct GET UUID
```

# <span class="mw-headline" id="bkmrk-am-sccm-1">Am SCCM</span>

Am SCCM wird eine Select Abfrage eingetragen.

<div class="vector-body" id="bkmrk-computermodell-1"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- Computermodell

</div></div></div>```
SELECT * FROM Win32_ComputerSystem WHERE Model LIKE “%D430%”
```

<div class="vector-body" id="bkmrk-uuid-1"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- UUID

</div></div></div>```
SELECT * FROM Win32_ComputerSystemProduct WHERE UUID LIKE “%D43014-63452-3986F%”
```

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

[http://systemmanagement.ro/blog/2009/05/22/install-drivers-by-computer-model-using-wmi-query/](http://systemmanagement.ro/blog/2009/05/22/install-drivers-by-computer-model-using-wmi-query/)