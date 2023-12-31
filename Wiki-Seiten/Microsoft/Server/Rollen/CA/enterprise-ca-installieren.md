# Enterprise CA installieren

<div class="vector-body" id="bkmrk-server-manager-%C3%B6ffne"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-server-manager-%C3%B6ffne-1" lang="de"><div class="mw-parser-output">1. Server Manager öffnen
2. Rollen und Funktionen hinzufügen  
    [![Install-Enterprise-CA 001.png](https://wiki.eidolf.de/images/thumb/c/c4/Install-Enterprise-CA_001.png/700px-Install-Enterprise-CA_001.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_001.png)
3. Rolle hinzufügen  
    [![Install-Enterprise-CA 002.png](https://wiki.eidolf.de/images/thumb/2/23/Install-Enterprise-CA_002.png/700px-Install-Enterprise-CA_002.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_002.png)
4. Server auswählen für Enterprise CA  
    [Datei:Install-Enterprise-CA 003.png](https://wiki.eidolf.de/index.php?title=Spezial:Hochladen&wpDestFile=Install-Enterprise-CA_003.png "Datei:Install-Enterprise-CA 003.png")
5. Active Directory-Zertifikatsdienste auswählen  
    [![Install-Enterprise-CA 004.png](https://wiki.eidolf.de/images/thumb/9/97/Install-Enterprise-CA_004.png/700px-Install-Enterprise-CA_004.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_004.png)
6. Zusätzlich als Rollendienst die Zertifizierungsstellen-Webregistrierung auswählen  
    [Datei:Install-Enterprise-CA 005.png](https://wiki.eidolf.de/index.php?title=Spezial:Hochladen&wpDestFile=Install-Enterprise-CA_005.png "Datei:Install-Enterprise-CA 005.png")
7. Übesicht der ausgewählten Rollen  
    [![Install-Enterprise-CA 006.png](https://wiki.eidolf.de/images/thumb/a/a5/Install-Enterprise-CA_006.png/700px-Install-Enterprise-CA_006.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_006.png)
8. Die Webregistrierung installiert den IIS  
    [![Install-Enterprise-CA 007.png](https://wiki.eidolf.de/images/thumb/6/65/Install-Enterprise-CA_007.png/700px-Install-Enterprise-CA_007.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_007.png)
9. IIS Rollendienste sollte schon alles benötigte ausgewählt sein  
    [![Install-Enterprise-CA 008.png](https://wiki.eidolf.de/images/thumb/7/7f/Install-Enterprise-CA_008.png/700px-Install-Enterprise-CA_008.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_008.png)
10. Wie immer hake ich "Zielserver bei Bedarf automatisch neu starten" an (wird aber nicht benötigt)  
    [![Install-Enterprise-CA 009.png](https://wiki.eidolf.de/images/thumb/b/bd/Install-Enterprise-CA_009.png/700px-Install-Enterprise-CA_009.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_009.png)
11. Wenn die Installation durchgelaufen ist, konfiguriert man die AD-Zertifikatsdienste  
    [![Install-Enterprise-CA 010.png](https://wiki.eidolf.de/images/thumb/4/4a/Install-Enterprise-CA_010.png/700px-Install-Enterprise-CA_010.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_010.png)
12. Für die Installation wird ein Domänen Administrator verwendet oder ein Benutzer mit den speziell benötigten Rechten  
    [![Install-Enterprise-CA 011.png](https://wiki.eidolf.de/images/thumb/3/3f/Install-Enterprise-CA_011.png/700px-Install-Enterprise-CA_011.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_011.png)
13. Konfigurieren von beiden Rollen auswählen  
    [![Install-Enterprise-CA 012.png](https://wiki.eidolf.de/images/thumb/e/e0/Install-Enterprise-CA_012.png/700px-Install-Enterprise-CA_012.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_012.png)
14. Unternehmenszertifizierunsstelle auswählen  
    [![Install-Enterprise-CA 013.png](https://wiki.eidolf.de/images/thumb/a/aa/Install-Enterprise-CA_013.png/700px-Install-Enterprise-CA_013.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_013.png)
15. Untergeordnete Zertifizierungsstelle (Sub-CA) auswählen  
    [![Install-Enterprise-CA 014.png](https://wiki.eidolf.de/images/thumb/0/0d/Install-Enterprise-CA_014.png/700px-Install-Enterprise-CA_014.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_014.png)
16. Neuen privaten Schlüssel generieren  
    [![Install-Enterprise-CA 015.png](https://wiki.eidolf.de/images/thumb/2/24/Install-Enterprise-CA_015.png/700px-Install-Enterprise-CA_015.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_015.png)
17. Verschlüsselungsstärke auswählen 4096 / SHA256  
    [![Install-Enterprise-CA 016.png](https://wiki.eidolf.de/images/thumb/e/ed/Install-Enterprise-CA_016.png/700px-Install-Enterprise-CA_016.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_016.png)
18. Benamung der Zertifizierungsstelle, andernfalls wird der Rechnername mitverwendet  
    [![Install-Enterprise-CA 017.png](https://wiki.eidolf.de/images/thumb/9/92/Install-Enterprise-CA_017.png/700px-Install-Enterprise-CA_017.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_017.png)
19. Da die Root CA in keiner Domäne ist muss ein Request erstellt werden der danach eingereicht werden kann  
    [![Install-Enterprise-CA 018.png](https://wiki.eidolf.de/images/thumb/8/83/Install-Enterprise-CA_018.png/700px-Install-Enterprise-CA_018.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_018.png)
20. Standardeinstellung bei den Datenbanken lassen  
    [![Install-Enterprise-CA 019.png](https://wiki.eidolf.de/images/thumb/6/68/Install-Enterprise-CA_019.png/700px-Install-Enterprise-CA_019.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_019.png)
21. Ergebnis der Konfiguration  
    [![Install-Enterprise-CA 020.png](https://wiki.eidolf.de/images/thumb/b/b5/Install-Enterprise-CA_020.png/700px-Install-Enterprise-CA_020.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_020.png)
22. Root CA öffnen  
    [![Install-Enterprise-CA 021.png](https://wiki.eidolf.de/images/thumb/9/90/Install-Enterprise-CA_021.png/700px-Install-Enterprise-CA_021.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_021.png)
23. Neue Anforderung einreichen  
    [![Install-Enterprise-CA 022.png](https://wiki.eidolf.de/images/thumb/b/bf/Install-Enterprise-CA_022.png/700px-Install-Enterprise-CA_022.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_022.png)
24. Den gerade erstellten Request einreichen  
    [![Install-Enterprise-CA 023.png](https://wiki.eidolf.de/images/6/6a/Install-Enterprise-CA_023.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_023.png)
25. Ausstehende Anforderung, ausstellen  
    [![Install-Enterprise-CA 024.png](https://wiki.eidolf.de/images/thumb/1/16/Install-Enterprise-CA_024.png/700px-Install-Enterprise-CA_024.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_024.png)
26. Das ausgestellte Zertifikat als Datei abspeichern  
    [![Install-Enterprise-CA 025.png](https://wiki.eidolf.de/images/thumb/5/51/Install-Enterprise-CA_025.png/700px-Install-Enterprise-CA_025.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_025.png)
27. Der Assistent öffnet sich  
    [![Install-Enterprise-CA 026.png](https://wiki.eidolf.de/images/9/96/Install-Enterprise-CA_026.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_026.png)
28. Als .p7b Datei abspeichern inkl. aller benötigten Zertifikate  
    [![Install-Enterprise-CA 027.png](https://wiki.eidolf.de/images/1/14/Install-Enterprise-CA_027.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_027.png)
29. Speicherpfad auswählen  
    [![Install-Enterprise-CA 028.png](https://wiki.eidolf.de/images/e/eb/Install-Enterprise-CA_028.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_028.png)
30. Fertig stellen  
    [![Install-Enterprise-CA 029.png](https://wiki.eidolf.de/images/a/a7/Install-Enterprise-CA_029.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_029.png)
31. MMC am Verwaltungsrechner öffnen  
    [![Install-Enterprise-CA 030.png](https://wiki.eidolf.de/images/thumb/1/18/Install-Enterprise-CA_030.png/700px-Install-Enterprise-CA_030.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_030.png)
32. Snap-In hinzufügen  
    [![Install-Enterprise-CA 031.png](https://wiki.eidolf.de/images/thumb/4/46/Install-Enterprise-CA_031.png/700px-Install-Enterprise-CA_031.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_031.png)
33. Zertifikate Snap-In auswählen  
    [![Install-Enterprise-CA 032.png](https://wiki.eidolf.de/images/thumb/c/cc/Install-Enterprise-CA_032.png/700px-Install-Enterprise-CA_032.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_032.png)
34. Computerkonto auswählen  
    [![Install-Enterprise-CA 033.png](https://wiki.eidolf.de/images/1/1a/Install-Enterprise-CA_033.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_033.png)
35. Den Enterprise CA Server eintragen  
    [![Install-Enterprise-CA 034.png](https://wiki.eidolf.de/images/a/aa/Install-Enterprise-CA_034.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_034.png)
36. Aufklappen und unter den Vertrauenswürdigen Stammzertifizierungsstellen,Importieren auswählen  
    [![Install-Enterprise-CA 035.png](https://wiki.eidolf.de/images/thumb/1/17/Install-Enterprise-CA_035.png/700px-Install-Enterprise-CA_035.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_035.png)
37. Der Assistent öffnet sich  
    [![Install-Enterprise-CA 036.png](https://wiki.eidolf.de/images/b/b0/Install-Enterprise-CA_036.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_036.png)
38. Das Root CA Zertifikat auswählen  
    [![Install-Enterprise-CA 037.png](https://wiki.eidolf.de/images/9/96/Install-Enterprise-CA_037.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_037.png)
39. Ziel Zertifikatsspeicher überprüfen  
    [![Install-Enterprise-CA 038.png](https://wiki.eidolf.de/images/d/d6/Install-Enterprise-CA_038.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_038.png)
40. Weiter und Fertig stellen
41. Von der Root CA das .crt Zertifikat und die Sperrliste kopieren  
    [![Install-Enterprise-CA 039.png](https://wiki.eidolf.de/images/thumb/7/7c/Install-Enterprise-CA_039.png/700px-Install-Enterprise-CA_039.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_039.png)
42. Die kopierten Dateien an der Sub CA unter inetpub\\wwwroot\\CertData einfügen  
    [![Install-Enterprise-CA 040.png](https://wiki.eidolf.de/images/thumb/4/42/Install-Enterprise-CA_040.png/700px-Install-Enterprise-CA_040.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_040.png)<dl><dd>[![Install-Enterprise-CA 041.png](https://wiki.eidolf.de/images/thumb/0/08/Install-Enterprise-CA_041.png/700px-Install-Enterprise-CA_041.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_041.png)</dd></dl>
43. An der Sub-CA kann nun das von der Root CA ausgestellt .p7b Zertifikat installiert werden  
    [![Install-Enterprise-CA 042.png](https://wiki.eidolf.de/images/thumb/1/12/Install-Enterprise-CA_042.png/700px-Install-Enterprise-CA_042.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_042.png)
44. Falls es beim vorherigen Punkt zu Fehlern kommt kann man noch mit folgendem Befehl das Zertifikat installieren  
    [![Install-Enterprise-CA 042-01.png](https://wiki.eidolf.de/images/thumb/5/57/Install-Enterprise-CA_042-01.png/700px-Install-Enterprise-CA_042-01.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_042-01.png)
45. Weiter gehts es mit [Enterprise CA konfigurieren](https://wiki.eidolf.de/index.php/Enterprise_CA_konfigurieren "Enterprise CA konfigurieren")

</div></div></div>