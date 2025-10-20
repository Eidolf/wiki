---
title: exchange-2003-keine-verbindung-uber-activesync
description: 
published: true
date: 2023-12-31T14:32:43.337Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:32:40.125Z
---

# Exchange 2003 keine Verbindung über ActiveSync

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-unterst%C3%BCtzungscode-0-1">Unterstützungscode 0x85010014</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Exchange_2003_keine_Verbindung_%C3%BCber_ActiveSync&action=edit&section=1 "Abschnitt bearbeiten: Unterstützungscode 0x85010014")<span class="mw-editsection-bracket">\]</span></span>

## <span class="mw-headline" id="bkmrk-wahrscheinliche-syst-1">Wahrscheinliche Systemkonfiguration</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Exchange_2003_keine_Verbindung_%C3%BCber_ActiveSync&action=edit&section=2 "Abschnitt bearbeiten: Wahrscheinliche Systemkonfiguration")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-server-2003-sp1-oder"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Server 2003 SP1 oder SP2 kein SBS
2. Exchange 2003 mit SP1 oder SP2
3. Domaincontroller, Zertifizierungsstelle und Exchange auf einem Server (nicht bestätigt)
4. Zertifikat richtig ausgestellt, da die Zertifizierungsstelle und OWA (Exchange) sich über 443 öffnen lassen
5. SSL ist auf der Standardwebsite erzwungen und evtl. ist die Formularbasierte Authentifizierung aktiviert

</div></div></div>## <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung">Lösung</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Exchange_2003_keine_Verbindung_%C3%BCber_ActiveSync&action=edit&section=3 "Abschnitt bearbeiten: Lösung")<span class="mw-editsection-bracket">\]</span></span>

**Deaktivieren der formularbasierten Authentifizierung für das virtuelle Exchange-Verzeichnis**  
Um ein sekundäres virtuelles Verzeichnis für Exchange zu erstellen, das auf Schritt 1 bis 7 in der folgenden Prozedur basiert, vergewissern Sie sich, dass die formularbasierte Authentifizierung für das virtuelle Exchange-Verzeichnis deaktiviert ist, bevor Sie die Kopie erstellen. Bevor Sie diese Schritte ausführen, deaktivieren Sie die formularbasierte Authentifizierung in Exchange-System-Manager, und starten Sie Internetinformationsdienste (IIS) anschließend neu. Gehen Sie hierzu folgendermaßen vor:

<div class="vector-body" id="bkmrk-%C3%96ffnen-sie-den-excha"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Öffnen Sie den Exchange-Manager.
2. Erweitern Sie Administrative Gruppen, erweitern Sie die erste administrative Gruppe, und erweitern Sie anschließend Server.
3. Erweitern Sie den Servercontainer für den Exchange Server 2003-Server, den Sie konfigurieren möchten, erweitern Sie Protokolle und dann HTTP.
4. Klicken Sie unter dem HTTP-Container mit der rechten Maustaste auf den Container Virtueller Exchange-Server, und klicken Sie anschließend auf Eigenschaften.
5. Klicken Sie auf die Registerkarte Einstellungen, deaktivieren Sie das Kontrollkästchen Formularbasierte Authentifizierung aktivieren, und klicken Sie auf OK.
6. Schließen Sie den Exchange-Manager.
7. Klicken Sie auf Start, klicken Sie auf Ausführen, geben Sie IISRESET/NOFORCE ein, und drücken Sie anschließend die EINGABETASTE, um Internetinformationsdienste (IIS) neu zu starten.

</div></div></div>**Erstellen eines sekundären virtuellen Verzeichnisses für Exchange-Server**  
Damit dieses virtuelle Verzeichnis für Exchange ActiveSync und Outlook Mobile Access funktioniert, müssen Sie es mit dem IIS-Manager erstellen. Wenn Sie mit Windows Server 2003 arbeiten, führen Sie die folgenden Schritte aus:

<div class="vector-body" id="bkmrk-starten-sie-den-iis-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Starten Sie den IIS-Manager.
2. Suchen Sie das virtuelle Exchange-Verzeichnis. Der Standardort hierfür lautet: <dl><dd>Web Sites\\Default Web Site\\Exchange</dd></dl>
3. Klicken Sie mit der rechten Maustaste auf das virtuelle Exchange-Verzeichnis, klicken Sie auf Alle Aufgaben, und anschließend auf Konfiguration in einer Datei speichern.
4. Geben Sie in das Feld Dateiname einen Namen ein. Geben Sie zum Beispiel ExchangeVDir ein. Klicken Sie auf OK.
5. Klicken Sie mit der rechten Maustaste auf das Stammverzeichnis der Website. In der Regel ist das "Standardwebsite". Klicken Sie auf Neu und dann auf Virtuelles Verzeichnis (aus Datei).
6. Klicken Sie im Dialogfeld Konfiguration importieren auf Durchsuchen, suchen Sie die Datei, die Sie in Schritt 4 erstellt haben, klicken Sie auf Öffnen, und dann auf Datei lesen.
7. Klicken Sie unter Wählen Sie eine Konfiguration zum Importieren auf Exchange, und dann auf OK.  
    Eine Meldung wird angezeigt, die besagt, dass das virtuelle Verzeichnis bereits vorhanden ist.
8. Wählen Sie die Option Neues virtuelles Verzeichnis erstellen aus. Geben Sie in das Feld Alias den Namen des neuen virtuellen Verzeichnisses ein, das von Exchange ActiveSync und Outlook Mobile Access verwendet werden soll. Geben Sie zum Beispiel exchange-oma ein. Klicken Sie auf OK.
9. Klicken Sie mit der rechten Maustaste auf das neue virtuelle Verzeichnis. Geben Sie im vorliegenden Beispiel exchange-oma ein. Klicken Sie auf Eigenschaften.
10. Klicken Sie auf die Registerkarte Verzeichnissicherheit.
11. Klicken Sie unter Authentifizierung und Zugriffssteuerung auf Bearbeiten.
12. Vergewissern Sie sich, dass nur die folgenden Authentifizierungsmethoden aktiviert sind, und klicken Sie dann auf OK: 
    - Integrierte Windows-Authentifizierung
    - Standardauthentifizierung
13. Klicken Sie auf der Registerkarte Verzeichnissicherheit unter Beschränkungen für IP-Adressen und Domänennamen auf Bearbeiten.
14. Klicken Sie auf die Option für Zugriff verweigert, dann auf Hinzufügen und danach auf Einzelner Computer. Geben Sie die IP-Adresse des Servers an, den Sie konfigurieren, und #klicken Sie dann auf OK.
15. Klicken Sie unter Sichere Kommunikation auf Bearbeiten. Stellen Sie sicher, dass Sicheren Kanal verlangen (SSL) nicht aktiviert ist, klicken Sie dann auf OK.
16. Klicken Sie auf OK, und schließen Sie den IIS-Manager.
17. Klicken Sie auf Start, dann auf Ausführen, geben Sie regedit ein, und klicken Sie auf OK.
18. Navigieren Sie zum folgenden Registrierungsunterschlüssel: <dl><dd>HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\MasSync\\Parameters</dd></dl>
19. Klicken Sie mit der rechten Maustaste auf den Schlüssel Parameter, dann auf Neu, und anschließend auf Zeichenfolgenwert.
20. Geben Sie ExchangeVDir ein, und drücken Sie die EINGABETASTE. Klicken Sie mit der rechten Maustaste auf ExchangeVDir, und dann auf Ändern.  
    **Hinweis**  
    Beachten Sie bei ExchangeVDir die Groß-/Kleinschreibung. Falls Sie ExchangeVDir nicht in der hier angegebenen Schreibweise eingeben, findet ActiveSync den Schlüssel nicht, wenn es den Ordner exchange-oma aufgefunden hat.
21. Geben Sie in das Feld Wert den Namen des neuen virtuellen Verzeichnisses ein, das Sie in Schritt 8 erstellt haben. Geben Sie zum Beispiel /exchange-oma ein. Klicken Sie auf OK.
22. Beenden Sie den Registrierungs-Editor.
23. Starten Sie den IIS-Admindienst neu. Gehen Sie hierzu folgendermaßen vor: 
    1. Klicken Sie auf Start, klicken Sie auf Ausführen, geben Sie services.msc ein, und klicken Sie auf OK.
    2. Klicken Sie in der Liste der Dienste mit der rechten Maustaste auf IIS-Administrationsdienst, dann auf Neu starten.
24. Wenn Sie die formularbasierte Authentifizierung auf dem Exchange-Server wiederverwenden möchten, gehen Sie folgendermaßen vor, um die formularbasierte Authentifizierung für das virtuelle Verzeichnis "/Exchange" im Exchange-System-Manager wieder zu aktivieren. 
    1. Öffnen Sie den Exchange-Manager.
    2. Erweitern Sie Administrative Gruppen, erweitern Sie die erste administrative Gruppe, und erweitern Sie anschließend Server.
    3. Erweitern Sie den Servercontainer für den Exchange Server 2003-Server, den Sie konfigurieren möchten, erweitern Sie Protokolle und dann HTTP.
    4. Klicken Sie unter dem HTTP-Container mit der rechten Maustaste auf den Container Virtueller Exchange-Server, und klicken Sie anschließend auf Eigenschaften.
    5. Klicken Sie auf die Registerkarte Einstellungen, aktivieren Sie das Kontrollkästchen Formularbasierte Authentifizierung aktivieren, und klicken Sie dann auf OK.
    6. Schließen Sie den Exchange-Manager.
    7. Klicken Sie auf Start, klicken Sie auf Ausführen, geben Sie IISRESET/NOFORCE ein, und drücken Sie anschließend die EINGABETASTE, um Internetinformationsdienste (IIS) neu zu starten.

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Exchange_2003_keine_Verbindung_%C3%BCber_ActiveSync&action=edit&section=4 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://www.administrator.de/index.php?content=c0caad190cc335ea6a4cd014475fc2f7" rel="nofollow">http://www.administrator.de/index.php?content=c0caad190cc335ea6a4cd014475fc2f7</a>
<a class="external free" href="http://support.microsoft.com/kb/817379" rel="nofollow">http://support.microsoft.com/kb/817379</a>
```