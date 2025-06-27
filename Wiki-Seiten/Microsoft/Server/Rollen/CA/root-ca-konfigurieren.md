---
title: root-ca-konfigurieren
description: 
published: true
date: 2025-06-27T15:03:57.323Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:31:44.810Z
---

# Root CA konfigurieren

1. Zertifizierungsstelle öffnen.
2. Eigenschaften der Zertifizierungsstelle öffnen  
![settings-root-ca_001.png](/media/settings-root-ca_001.png)
3. Unter Erweiterungen einen Sperrlisten-Verteilungspunkt hinzufügen  
![settings-root-ca_002.png](/media/settings-root-ca_002.png)
4. http Pfad zum Server angeben (DNS Eintrag setzen)  
![settings-root-ca_003.png](/media/settings-root-ca_003.png)
5. **CaName** einfügen  
![settings-root-ca_004.png](/media/settings-root-ca_004.png)
6. **CRLNameSuffix** einfügen  
![settings-root-ca_005.png](/media/settings-root-ca_005.png)
7. **DeltaCRLAllowed** einfügen 
![settings-root-ca_006.png](/media/settings-root-ca_006.png)
8. **.crl** anhängen 
![settings-root-ca_007.png](/media/settings-root-ca_007.png)
9. Den neu hinzugefügten Pfad in die Sperrliste und CDP-Erweiterung einbeziehen 
![settings-root-ca_008.png](/media/settings-root-ca_008.png)
10. Den geforderten Neustart des Dienstes nicht durchführen
11. Jetzt Zugriff auf Stelleninformationen hinzufügen 
![settings-root-ca_009.png](/media/settings-root-ca_009.png)
12. http Pfad zum Server angeben 
![settings-root-ca_010.png](/media/settings-root-ca_010.png)
13. **ServerDNSName** einfügen  
![settings-root-ca_011.png](/media/settings-root-ca_011.png)
14. Unterstrich einfügen  
![settings-root-ca_012.png](/media/settings-root-ca_012.png)
15. **CaName** einfügen 
![settings-root-ca_013.png](/media/settings-root-ca_013.png)
16. **CertificateName** einfügen  
![settings-root-ca_014.png](/media/settings-root-ca_014.png)
17. **.crt** anhängen 
![settings-root-ca_015.png](/media/settings-root-ca_015.png)
18. Den neu hinzugefügten Pfad in AIA-Erweiterung einbeziehen  
![settings-root-ca_016.png](/media/settings-root-ca_016.png)
19. Jetzt geforderten Neustart des Dienstes durchführen
20. Eigenschaften der Gesperrten Zertifikate öffnen  
![settings-root-ca_017.png](/media/settings-root-ca_017.png)
21. Veröffentlichungsintervall auf ein Jahr erhöhen  
![settings-root-ca_018.png](/media/settings-root-ca_018.png)
22. Sperrliste veröffentlichen  
![settings-root-ca_019.png](/media/settings-root-ca_019.png)
![settings-root-ca_020.png](/media/settings-root-ca_020.png)
23. Nochmal die Einstellungen der Zertifizierungsstelle öffnen
![settings-root-ca_001.png](/media/settings-root-ca_001.png)
24. Unter Allgemein das Zertifikat anzeigen 
![settings-root-ca_021.png](/media/settings-root-ca_021.png)
25. Bei Reiter Details das Zertifikat in eine Datei kopieren
![settings-root-ca_022.png](/media/settings-root-ca_022.png)
26. Willkommenseite überpringen
![settings-root-ca_023.png](/media/settings-root-ca_023.png)
27. DER-codiert auswählen und auf Weiter klicken
![settings-root-ca_024.png](/media/settings-root-ca_024.png)
28. **UNC** Pfad eingeben zum Speichern des Zertifikates und den Anhang **.cer** nicht vergessen
![settings-root-ca_025.png](/media/settings-root-ca_025.png)
29. Auf der RootCA unter `C:\Windows\System32\CertSrv\CertEnroll` die zwei Dateien kopieren und wegsichern, dieses werden für die Zwischenzertifizierungsstelle benötigt
![settings-root-ca_026.png](/media/settings-root-ca_026.png)
30. Die Root CA ist eingerichtet und nun geht es an die Zwischenzertifizierungsstelle.