---
title: s4b-mediation-server-einrichten
description: 
published: true
date: 2023-12-31T13:33:57.821Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:33:54.781Z
---

# S4B Mediation Server Einrichten

<div class="vector-body" id="bkmrk-skype-for-business-s"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-skype-for-business-s-1" lang="de"><div class="mw-parser-output">1. Skype for Business Server-Topologie-Generator öffnen und aktuelle Topologie laden
2. Mit Rechtsklick auf den vorhandenen Vermittlungspool klicken und die Eigenschaften davon bearbeiten  
    [![S4BMediation01.png](https://wiki.eidolf.de/images/0/0f/S4BMediation01.png)](https://wiki.eidolf.de/index.php/Datei:S4BMediation01.png)
3. Den Port auf TCP Seite zu 5060 ändern  
    [![S4BMediation02.png](https://wiki.eidolf.de/images/b/bc/S4BMediation02.png)](https://wiki.eidolf.de/index.php/Datei:S4BMediation02.png)
4. Topologie veröffentlichen  
    [![S4BMediation09.png](https://wiki.eidolf.de/images/9/90/S4BMediation09.png)](https://wiki.eidolf.de/index.php/Datei:S4BMediation09.png)
5. Falls der **"Skype for Business Server-Vermittlung (eng.:Mediation"** Dienst nicht schon vorhanden ist muss an diesem Punkt noch einmal der Konfigurationswizard von S4B durchgeführt werden.
6. Ein neues PSTN Gateway erstellen  
    [![S4BMediation03.png](https://wiki.eidolf.de/images/c/cb/S4BMediation03.png)](https://wiki.eidolf.de/index.php/Datei:S4BMediation03.png)
7. Als FQDN die FreePBX Adresse eintragen (DNS Eintrag muss vorhanden sein, sonst lokaler Hosteintrag)  
    [![S4BMediation04.png](https://wiki.eidolf.de/images/9/9a/S4BMediation04.png)](https://wiki.eidolf.de/index.php/Datei:S4BMediation04.png)
8. Bei IP-Adresse definieren einfach auf Standard lassen  
    [![S4BMediation05.png](https://wiki.eidolf.de/images/1/11/S4BMediation05.png)](https://wiki.eidolf.de/index.php/Datei:S4BMediation05.png)
9. SIP Transportprotokoll auf TCP umstellen und den Port 5061 eintragen  
    [![S4BMediation06.png](https://wiki.eidolf.de/images/0/0b/S4BMediation06.png)](https://wiki.eidolf.de/index.php/Datei:S4BMediation06.png)
10. Überprüfen ob das PSTN-Gateway und der Trunk vorhanden sind  
    [![S4BMediation07.png](https://wiki.eidolf.de/images/8/85/S4BMediation07.png)](https://wiki.eidolf.de/index.php/Datei:S4BMediation07.png)
11. Abermals wie unter Punkt 2 die Eigenschaften vom Vermittlungspool öffnen und den jetzt vorhandenen Trunk als Standard setzen  
    [![S4BMediation08.png](https://wiki.eidolf.de/images/6/64/S4BMediation08.png)](https://wiki.eidolf.de/index.php/Datei:S4BMediation08.png)
12. Topologie veröffentlichen  
    [![S4BMediation09.png](https://wiki.eidolf.de/images/9/90/S4BMediation09.png)](https://wiki.eidolf.de/index.php/Datei:S4BMediation09.png)

</div></div></div>