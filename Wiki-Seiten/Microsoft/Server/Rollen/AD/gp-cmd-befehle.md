---
title: gp-cmd-befehle
description: 
published: true
date: 2023-12-31T14:30:28.829Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:30:25.390Z
---

# GP CMD Befehle

## <span class="mw-headline" id="bkmrk-gpupdate-1">gpupdate</span>

<div class="vector-body" id="bkmrk-%2Ftarget%3A%7Bcomputer-%7C-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- /Target:{Computer | User}  
    Führt die Aktualisierung nur für den COMPUTER bzw. den aktuellen BENUTZER durch. Standardmäßig, werden die Richtlinieneinstellungen für beide aktualisiert.
- /Force  
    Wendet alle Richtlinieneinstellungen erneut an. Standardmäßig werden nur geänderte Richtlinie angewendet.
- /Wait:{Wert}  
    Legt die Wartezeit für die Richtlinienverarbeitung in Sekunden fest. Der Standard ist 600 Sekunden. Der Wert "0" bedeutet keine Wartezeit und "-1" bedeutet eine unbegrenzte Wartezeit. Wenn das Zeitlimit &#129;berschritten wird, wird die Eingabeauf-forderung angezeigt, während die Richtlinien-verarbeitung weiterhin fortgesetzt wird.
- /Logoff  
    Verursacht eine Abmeldung nach Aktualisierung der Gruppenrichtlinieneinstellungen. Dies ist für clientseitige Erweiterungen der Gruppenrichtlinie erforderlich, die die Gruppenrichtlinie nicht in einem Hintergrundaktualisierungszyklus, aber bei der Benutzeranmeldung vearbeiten, wie z. B. die Softwareinstallation oder Ordnerumleitung für Benutzer. Die Option hat keine Auswirkungen, wenn keine Erweiterungen aufgerufen werden, die eine Abmeldung erfordern.
- /Boot  
    Verursacht einen Neustart nach Anwendung der Gruppenrichtlinieneinstellungen. Dies ist für clientseitige Erweiterungen der Gruppenrichtlinie erforderlich, die die Gruppenrichtlinie nicht in einem Hintergrundaktualisierungszyklus, aber beim Start des Computers vearbeiten, wie z. B. die Softwareinstallation auf dem Computer. Die Option hat keine Auswirkungen, wenn keine Erweiterungen aufgerufen werden, die einen Neustart erfordern.
- /Sync  
    Verursacht, dass die nächste Richtlinienanwendung im Vordergrund synchron ausgeführt wird. Richtlinien-anwendungen im Vordergrund treten beim Systemstart und bei der Benutzeranmeldung auf. Sie können dies nur für Benutzer bzw. Computer oder für beide mit dem Parameter /Target festlegen. Die Parameter /Force und /Wait werden gegebenenfalls ignoriert.

</div></div></div>## <span class="mw-headline" id="bkmrk-secedit-1">secedit</span>

<div class="vector-body" id="bkmrk-%2Frefreshpolicy-machi"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- /refreshpolicy machine\_policy /enforce  
    Bei **Windows 2000** muss dieser Befehl eingeben werden anstatt " gpupdate /force "

</div></div></div>## <span class="mw-headline" id="bkmrk-gpresult-1">gpresult</span>

<div class="vector-body" id="bkmrk-%2Fs-systemremotesyste"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">- /S System  
    Remotesystem für die Verbindungsherstellung.
- /U \[Domäne\\\]Benutzer  
    Bestimmt den Benutzerkontext, unter dem der Befehl ausgef&#129;hrt wird. Kann nicht mit /X, /H verwendet werden.
- /P \[Kennwort\]  
    Bestimmt das Kennwort für den Benutzerkontext. Auslassung fordert zur Kennworteingabe auf. Kann nicht mit /X, /H verwendet werden.
- /SCOPE  
    Legt fest, ob die Benutzer- oder die Computereinstellungen angezeigt werden sollen. Gültige Werte: "USER", "COMPUTER".
- /USER \[Domäne\\\]Benutzer  
    Legt den Benutzername fest, für den die RSOP-Daten angezeigt werden sollen.
- /X &lt;Dateiname&gt;  
    Speichert den Bericht im XML-Format an dem Speicherort und mit dem Dateinamen, der durch nach dem Parameter &lt;Dateiname&gt; (gültig in Windows Vista SP1 und höher und Windows Server 2008 und höher)
- /H &lt;Dateiname&gt;  
    Speichert den Bericht im HTML-Format an dem Speicherort und mit dem Dateinamen, der durch den Parameter &lt;Dateiname&gt; angegeben ist (gültig in Windows Vista SP1 und höher und Windows Server 2008 und höher)
- /F  
    Zwingt gpresult zum šberschreiben des im Befehl "/X" oder "/H" angegebenen Dateinamens.
- /R  
    Zeigt RSoP-Zusammenfassungsdaten an.
- /V  
    Legt fest, dass ausf&#129;hrliche Informationen angezeigt werden sollen. Dadurch werden zusätzliche detaillierte Einstellungen, die mit 1 angewendet wurden.
- /Z  
    Legt fest, dass besonders ausführliche Informationen angezeigt werden sollen. Diese Informationen zeigen zusätzliche detaillierte Einstellungen, die mit 1 oder höher angegeben wurden, an. Somit können Sie erkennen, ob eine Einstellung an mehreren Stellen gesetzt wurde. Weitere Informationen erhalten Sie in den Gruppenrichtlinien-Onlinehilfethemen.
- /?  
    Zeigt diese Hilfe an.

</div></div></div>  
Beispiele:

```
GPRESULT /R
GPRESULT /H GPReport.html
GPRESULT /USER Zielbenutzername /V
GPRESULT /S System /USER Zielbenutzername /SCOPE COMPUTER /Z
GPRESULT /S System /U Benutzername /P Kennwort /SCOPE USER /V
```

  
Für den Seitweisen vorschub: Ein " |more " anhängen

```
GPRESULT /S System /USER Zielbenutzername /Z |more
```