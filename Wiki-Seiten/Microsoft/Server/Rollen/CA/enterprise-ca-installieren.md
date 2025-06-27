---
title: enterprise-ca-installieren
description: 
published: true
date: 2025-06-27T14:24:17.386Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:31:26.726Z
---

# Enterprise CA installieren

1. Server Manager öffnen
2. Rollen und Funktionen hinzufügen
![install-enterprise-ca_001.png](/media/install-enterprise-ca_001.png)
3. Rolle hinzufügen 
![install-enterprise-ca_002.png](/media/install-enterprise-ca_002.png)
4. Server auswählen für Enterprise CA  
![install-enterprise-ca_003.png](/media/install-enterprise-ca_003.png)
5. Active Directory-Zertifikatsdienste auswählen  
![install-enterprise-ca_004.png](/media/install-enterprise-ca_004.png)
6. Zusätzlich als Rollendienst die Zertifizierungsstellen-Webregistrierung auswählen 
![install-enterprise-ca_005.png](/media/install-enterprise-ca_005.png)
7. Übesicht der ausgewählten Rollen  
![install-enterprise-ca_006.png](/media/install-enterprise-ca_006.png)
8. Die Webregistrierung installiert den IIS  
![install-enterprise-ca_007.png](/media/install-enterprise-ca_007.png)
9. IIS Rollendienste sollte schon alles benötigte ausgewählt sein 
![install-enterprise-ca_008.png](/media/install-enterprise-ca_008.png)
10. Wie immer hake ich "Zielserver bei Bedarf automatisch neu starten" an (wird aber nicht benötigt) 
![install-enterprise-ca_009.png](/media/install-enterprise-ca_009.png)
11. Wenn die Installation durchgelaufen ist, konfiguriert man die AD-Zertifikatsdienste  
![install-enterprise-ca_010.png](/media/install-enterprise-ca_010.png)
12. Für die Installation wird ein Domänen Administrator verwendet oder ein Benutzer mit den speziell benötigten Rechten 
![install-enterprise-ca_011.png](/media/install-enterprise-ca_011.png)
13. Konfigurieren von beiden Rollen auswählen  
![install-enterprise-ca_012.png](/media/install-enterprise-ca_012.png)
14. Unternehmenszertifizierunsstelle auswählen
![install-enterprise-ca_013.png](/media/install-enterprise-ca_013.png)
15. Untergeordnete Zertifizierungsstelle (Sub-CA) auswählen  
![install-enterprise-ca_014.png](/media/install-enterprise-ca_014.png)
16. Neuen privaten Schlüssel generieren  
![install-enterprise-ca_015.png](/media/install-enterprise-ca_015.png)
17. Verschlüsselungsstärke auswählen 4096 / SHA256  
![install-enterprise-ca_016.png](/media/install-enterprise-ca_016.png)
18. Benamung der Zertifizierungsstelle, andernfalls wird der Rechnername mitverwendet  
![install-enterprise-ca_017.png](/media/install-enterprise-ca_017.png)
19. Da die Root CA in keiner Domäne ist muss ein Request erstellt werden der danach eingereicht werden kann 
![install-enterprise-ca_018.png](/media/install-enterprise-ca_018.png)
20. Standardeinstellung bei den Datenbanken lassen 
![install-enterprise-ca_019.png](/media/install-enterprise-ca_019.png)
21. Ergebnis der Konfiguration  
![install-enterprise-ca_020.png](/media/install-enterprise-ca_020.png)
22. Root CA öffnen  
![install-enterprise-ca_021.png](/media/install-enterprise-ca_021.png)
23. Neue Anforderung einreichen  
![install-enterprise-ca_022.png](/media/install-enterprise-ca_022.png)
24. Den gerade erstellten Request einreichen
![install-enterprise-ca_023.png](/media/install-enterprise-ca_023.png)
25. Ausstehende Anforderung, ausstellen  
![install-enterprise-ca_024.png](/media/install-enterprise-ca_024.png)
26. Das ausgestellte Zertifikat als Datei abspeichern 
![install-enterprise-ca_025.png](/media/install-enterprise-ca_025.png)
27. Der Assistent öffnet sich  
![install-enterprise-ca_026.png](/media/install-enterprise-ca_026.png)
28. Als .p7b Datei abspeichern inkl. aller benötigten Zertifikate 
![install-enterprise-ca_027.png](/media/install-enterprise-ca_027.png)
29. Speicherpfad auswählen  
![install-enterprise-ca_028.png](/media/install-enterprise-ca_028.png)
30. Fertig stellen  
![install-enterprise-ca_029.png](/media/install-enterprise-ca_029.png)
31. MMC am Verwaltungsrechner öffnen 
![install-enterprise-ca_030.png](/media/install-enterprise-ca_030.png)
32. Snap-In hinzufügen  
![install-enterprise-ca_031.png](/media/install-enterprise-ca_031.png)
33. Zertifikate Snap-In auswählen 
![install-enterprise-ca_032.png](/media/install-enterprise-ca_032.png)
34. Computerkonto auswählen  
![install-enterprise-ca_033.png](/media/install-enterprise-ca_033.png)
35. Den Enterprise CA Server eintragen
![install-enterprise-ca_034.png](/media/install-enterprise-ca_034.png)
36. Aufklappen und unter den Vertrauenswürdigen Stammzertifizierungsstellen,Importieren auswählen 
![install-enterprise-ca_035.png](/media/install-enterprise-ca_035.png)
37. Der Assistent öffnet sich  
![install-enterprise-ca_036.png](/media/install-enterprise-ca_036.png)
38. Das Root CA Zertifikat auswählen  
![install-enterprise-ca_037.png](/media/install-enterprise-ca_037.png)
39. Ziel Zertifikatsspeicher überprüfen 
![install-enterprise-ca_038.png](/media/install-enterprise-ca_038.png)
40. Weiter und Fertig stellen
41. Von der Root CA das .crt Zertifikat und die Sperrliste kopieren 
![install-enterprise-ca_039.png](/media/install-enterprise-ca_039.png)
42. Die kopierten Dateien an der Sub CA unter inetpub\\wwwroot\\CertData einfügen 
![install-enterprise-ca_040.png](/media/install-enterprise-ca_040.png)
![install-enterprise-ca_041.png](/media/install-enterprise-ca_041.png)
43. An der Sub-CA kann nun das von der Root CA ausgestellt .p7b Zertifikat installiert werden 
![install-enterprise-ca_042.png](/media/install-enterprise-ca_042.png)
44. Falls es beim vorherigen Punkt zu Fehlern kommt kann man noch mit folgendem Befehl das Zertifikat installieren 
![install-enterprise-ca_042-01.png](/media/install-enterprise-ca_042-01.png)
45. Weiter gehts es mit [enterprise-ca-konfigurieren](/de/Wiki-Seiten/Microsoft/Server/Rollen/CA/enterprise-ca-konfigurieren)