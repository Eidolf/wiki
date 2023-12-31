# Root CA konfigurieren

# <span class="mw-headline" id="bkmrk-root-ca-1">Root CA</span>

<div class="vector-body" id="bkmrk-zertifizierungsstell"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-zertifizierungsstell-1" lang="de"><div class="mw-parser-output">1. Zertifizierungsstelle öffnen.
2. Eigenschaften der Zertifizierungsstelle öffnen  
    [![Settings-Root-CA 001.png](https://wiki.eidolf.de/images/thumb/7/78/Settings-Root-CA_001.png/700px-Settings-Root-CA_001.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_001.png)
3. Unter Erweiterungen einen Sperrlisten-Verteilungspunkt hinzufügen  
    [![Settings-Root-CA 002.png](https://wiki.eidolf.de/images/thumb/1/16/Settings-Root-CA_002.png/350px-Settings-Root-CA_002.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_002.png)
4. http Pfad zum Server angeben (DNS Eintrag setzen)  
    [![Settings-Root-CA 003.png](https://wiki.eidolf.de/images/thumb/f/fb/Settings-Root-CA_003.png/350px-Settings-Root-CA_003.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_003.png)
5. **CaName** einfügen  
    [![Settings-Root-CA 004.png](https://wiki.eidolf.de/images/thumb/f/f4/Settings-Root-CA_004.png/350px-Settings-Root-CA_004.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_004.png)
6. **CRLNameSuffix** einfügen  
    [![Settings-Root-CA 005.png](https://wiki.eidolf.de/images/thumb/1/1d/Settings-Root-CA_005.png/350px-Settings-Root-CA_005.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_005.png)
7. **DeltaCRLAllowed** einfügen  
    [![Settings-Root-CA 006.png](https://wiki.eidolf.de/images/thumb/0/05/Settings-Root-CA_006.png/350px-Settings-Root-CA_006.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_006.png)
8. **.crl** anhängen  
    [![Settings-Root-CA 007.png](https://wiki.eidolf.de/images/thumb/3/35/Settings-Root-CA_007.png/350px-Settings-Root-CA_007.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_007.png)
9. Den neu hinzugefügten Pfad in die Sperrliste und CDP-Erweiterung einbeziehen  
    [![Settings-Root-CA 008.png](https://wiki.eidolf.de/images/thumb/8/87/Settings-Root-CA_008.png/350px-Settings-Root-CA_008.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_008.png)
10. Den geforderten Neustart des Dienstes nicht durchführen
11. Jetzt Zugriff auf Stelleninformationen hinzufügen  
    [![Settings-Root-CA 009.png](https://wiki.eidolf.de/images/thumb/e/e1/Settings-Root-CA_009.png/350px-Settings-Root-CA_009.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_009.png)
12. http Pfad zum Server angeben  
    [![Settings-Root-CA 010.png](https://wiki.eidolf.de/images/thumb/c/cc/Settings-Root-CA_010.png/350px-Settings-Root-CA_010.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_010.png)
13. **ServerDNSName** einfügen  
    [![Settings-Root-CA 011.png](https://wiki.eidolf.de/images/thumb/f/f8/Settings-Root-CA_011.png/350px-Settings-Root-CA_011.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_011.png)
14. Unterstrich einfügen  
    [![Settings-Root-CA 012.png](https://wiki.eidolf.de/images/thumb/e/e4/Settings-Root-CA_012.png/350px-Settings-Root-CA_012.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_012.png)
15. **CaName** einfügen  
    [![Settings-Root-CA 013.png](https://wiki.eidolf.de/images/thumb/f/f5/Settings-Root-CA_013.png/350px-Settings-Root-CA_013.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_013.png)
16. **CertificateName** einfügen  
    [![Settings-Root-CA 014.png](https://wiki.eidolf.de/images/thumb/c/c5/Settings-Root-CA_014.png/350px-Settings-Root-CA_014.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_014.png)
17. **.crt** anhängen  
    [![Settings-Root-CA 015.png](https://wiki.eidolf.de/images/thumb/4/47/Settings-Root-CA_015.png/350px-Settings-Root-CA_015.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_015.png)
18. Den neu hinzugefügten Pfad in AIA-Erweiterung einbeziehen  
    [![Settings-Root-CA 016.png](https://wiki.eidolf.de/images/thumb/a/a1/Settings-Root-CA_016.png/350px-Settings-Root-CA_016.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_016.png)
19. Jetzt geforderten Neustart des Dienstes durchführen
20. Eigenschaften der Gesperrten Zertifikate öffnen  
    [![Settings-Root-CA 017.png](https://wiki.eidolf.de/images/thumb/3/3f/Settings-Root-CA_017.png/700px-Settings-Root-CA_017.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_017.png)
21. Veröffentlichungsintervall auf ein Jahr erhöhen  
    [![Settings-Root-CA 018.png](https://wiki.eidolf.de/images/thumb/3/3c/Settings-Root-CA_018.png/350px-Settings-Root-CA_018.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_018.png)
22. Sperrliste veröffentlichen  
    [![Settings-Root-CA 019.png](https://wiki.eidolf.de/images/thumb/1/11/Settings-Root-CA_019.png/700px-Settings-Root-CA_019.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_019.png)  
      
    [![Settings-Root-CA 020.png](https://wiki.eidolf.de/images/thumb/d/dc/Settings-Root-CA_020.png/350px-Settings-Root-CA_020.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_020.png)
23. Nochmal die Einstellungen der Zertifizierungsstelle öffnen&gt;  
    [![Settings-Root-CA 001.png](https://wiki.eidolf.de/images/thumb/7/78/Settings-Root-CA_001.png/700px-Settings-Root-CA_001.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_001.png)
24. Unter Allgemein das Zertifikat anzeigen&gt;  
    [![Settings-Root-CA 021.png](https://wiki.eidolf.de/images/thumb/e/ec/Settings-Root-CA_021.png/350px-Settings-Root-CA_021.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_021.png)
25. Bei Reiter Details das Zertifikat in eine Datei kopieren&gt;  
    [![Settings-Root-CA 022.png](https://wiki.eidolf.de/images/thumb/8/88/Settings-Root-CA_022.png/350px-Settings-Root-CA_022.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_022.png)
26. Willkommenseite überpringen&gt;  
    [![Settings-Root-CA 023.png](https://wiki.eidolf.de/images/thumb/c/c8/Settings-Root-CA_023.png/350px-Settings-Root-CA_023.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_023.png)
27. DER-codiert auswählen und auf Weiter klicken&gt;  
    [![Settings-Root-CA 024.png](https://wiki.eidolf.de/images/thumb/c/c8/Settings-Root-CA_024.png/350px-Settings-Root-CA_024.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_024.png)
28. **UNC** Pfad eingeben zum Speichern des Zertifikates und den Anhang **.cer** nicht vergessen&gt;  
    [![Settings-Root-CA 025.png](https://wiki.eidolf.de/images/thumb/4/48/Settings-Root-CA_025.png/350px-Settings-Root-CA_025.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_025.png)
29. Auf der RootCA unter `C:\Windows\System32\CertSrv\CertEnroll` die zwei Dateien kopieren und wegsichern, dieses werden für die Zwischenzertifizierungsstelle benötigt&gt;  
    [![Settings-Root-CA 026.png](https://wiki.eidolf.de/images/thumb/7/7a/Settings-Root-CA_026.png/700px-Settings-Root-CA_026.png)](https://wiki.eidolf.de/index.php/Datei:Settings-Root-CA_026.png)
30. Die Root CA ist eingerichtet und nun geht es an die Zwischenzertifizierungsstelle.

</div></div></div>