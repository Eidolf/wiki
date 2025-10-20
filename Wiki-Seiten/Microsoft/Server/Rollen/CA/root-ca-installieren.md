---
title: root-ca-installieren
description: 
published: true
date: 2025-06-27T14:56:14.281Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:31:40.319Z
---

# Root CA installieren

1. Server Manager öffnen
2. Rollen und Funktionen hinzufügen  
![install-root-ca_001.png](/media/install-root-ca_001.png)
3. Rolle hinzufügen 
![install-root-ca_002.png](/media/install-root-ca_002.png)
4. Server auswählen  
![install-root-ca_003.png](/media/install-root-ca_003.png)
5. Active Directory-Zertifikatsdienste als einziges auswählen 
![install-root-ca_004.png](/media/install-root-ca_004.png)
6. Features überspringen
7. Information überspringen
8. Nur die Zertifizierungsstelle markieren 
![install-root-ca_005.png](/media/install-root-ca_005.png)
9. Ich hake immer den automatischen neustart an (optional)  
![install-root-ca_006.png](/media/install-root-ca_006.png)
10. Die Fahne anklicken und weiterhin auf "Configure AD Certificate Service..."  
![install-root-ca_007.png](/media/install-root-ca_007.png)
11. Lokale Administrator Anmeldedaten angeben, da wir keine Enterprise CA erstellen 
![install-root-ca_008.png](/media/install-root-ca_008.png)
12. Zertifizierungsstelle auswählen  
![install-root-ca_009.png](/media/install-root-ca_009.png)
13. Eigenständige Zertifizierungsstelle auswählen  
![install-root-ca_010.png](/media/install-root-ca_010.png)
14. Root CA auswählen  
![install-root-ca_011.png](/media/install-root-ca_011.png)
15. Einen neuen privaten Schlüssel erstellen 
![install-root-ca_012.png](/media/install-root-ca_012.png)
16. SHA256 mit RSA4096 auswählen (RSA2048 ist auch noch ausreichend)
![install-root-ca_013.png](/media/install-root-ca_013.png)
17. CA Namen vergeben  
    \*CN=Beliebig  
    \*O=Firmenname  
    \*C=Land  
    ![install-root-ca_014.png](/media/install-root-ca_014.png)
18. Gültigkeit eingeben, 20 Jahre ist ein guter Standard 
![install-root-ca_015.png](/media/install-root-ca_015.png)
19. Datenbankpfade können auf Standard belassen werden 
![install-root-ca_016.png](/media/install-root-ca_016.png)
20. Übersicht der Einstellungen  
![install-root-ca_017.png](/media/install-root-ca_017.png)
21. Mit "Close" Beenden  
![install-root-ca_018.png](/media/install-root-ca_018.png)
22. Weiter geht es mit [root-ca-konfigurieren](/de/Wiki-Seiten/Microsoft/Server/Rollen/CA/root-ca-konfigurieren)