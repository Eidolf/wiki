# Windows CA

## <span class="mw-headline" id="bkmrk-einrichten-1">Einrichten</span>

### <span class="mw-headline" id="bkmrk-server-core-1">Server Core</span>

<big>**Eine Warnung!** Nach längerem Arbeiten mit einer Server Core Root CA würde ich von so einer Installation abraten, die Root CA sollte mindestens die GUI enthalten, besser auch die Sub CA</big>

1. Grundinstallation und Einstellungen einer Server Core Installation  
    [Windows Server Core](https://wiki.eidolf.de/index.php/Windows_Server_Core "Windows Server Core")
2. Zertifizierungsstelle installieren und einrichten  
    [Zertifizierungsstelle installieren](https://wiki.eidolf.de/index.php/Zertifizierungsstelle_installieren "Zertifizierungsstelle installieren")
3. Weitere Konfigurationen und Einstellungen  
    [Zertifizierungsstelle Konfigurieren](https://wiki.eidolf.de/index.php/Zertifizierungsstelle_Konfigurieren "Zertifizierungsstelle Konfigurieren")

## <span class="mw-headline" id="bkmrk-standard-aufgaben-1">Standard Aufgaben</span>

### <span class="mw-headline" id="bkmrk-sub-ca-zertifikat-er-1">Sub CA Zertifikat erneuern</span>

#### <span class="mw-headline" id="bkmrk-im-server-core-umfel-1">Im Server Core Umfeld</span>

Eines der Punkte weshalb ich eine Warnung bei Server Core Installationen hinzugefügt habe ist der umständliche Weg das Sub CA Zertifikat zu erneuern.  
Es werden leider wichtige Punkte in der Remote MMC ausgeblendet und weiterhin ist es nicht einfach Berechtigungen zu übergeben.  
Berechtigung ist der nächste wichtige Punkt, um einfach mit certsrv.msc eine Remoteverbindung mit der Root CA herzustellen (wird für das Ausstellen benötigt), sollte das Passwort des lokalen Admins an der Root CA gleich sein wie mit dem lokalen Admin des Remoteclients.

1. Shell Verbindung zur Sub CA herstellen
2. Befehl `certutil.exe -renewcert` ausführen, hiermit wird auch der private Schlüssel getauscht. Falls hier ein Fehler erscheint kann es sein das schon ein Versuch gestartet wurde. Hier kann man mit `certutil.exe -renewcert -f` die bestehende Anforderung überschreiben. <dl><dt>[![SubCARenewCert001.png](https://wiki.eidolf.de/images/b/bb/SubCARenewCert001.png)](https://wiki.eidolf.de/index.php/Datei:SubCARenewCert001.png)</dt></dl>
3. Da keine Verbindung zur Root CA hergestellt werden kann wird man zum Schluss auf Abbrechen drücken müssen und die Requestdatei manuell an die Root CA einreichen. Die Request Datei liegt auf der Sub CA direkt unter C:\\
4. certsrv.msc Verbindung zur Root CA herstellen (Wie gesagt lokales Administrator Passwort beachten).
5. Rechtklick auf den Verbindungspunkt der Root CA &gt; Alle Aufgaben &gt; Neue Anforderung einreichen <dl><dt>[![SubCARenewCert002.png](https://wiki.eidolf.de/images/b/b0/SubCARenewCert002.png)](https://wiki.eidolf.de/index.php/Datei:SubCARenewCert002.png)</dt></dl>
6. Die Request Datei auswählen <dl><dt>[![SubCARenewCert003.png](https://wiki.eidolf.de/images/1/16/SubCARenewCert003.png)](https://wiki.eidolf.de/index.php/Datei:SubCARenewCert003.png)</dt></dl>
7. Unter den Ausstehenden Anforderungen den Request mit Rechtklick &gt; Alle Aufgaben &gt; Ausstellen freigeben <dl><dt>[![SubCARenewCert004.png](https://wiki.eidolf.de/images/0/09/SubCARenewCert004.png)](https://wiki.eidolf.de/index.php/Datei:SubCARenewCert004.png)</dt></dl>
8. Unter Ausgestellte Zertifikate das neue Zertifikat exportieren mit doppelklick auf das Zertifikat &gt; Details &gt; In Datei kopieren &gt; .p7b export inkl. aller Zertifikate <dl><dt>[![Install-Enterprise-CA 027.png](https://wiki.eidolf.de/images/1/14/Install-Enterprise-CA_027.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_027.png)</dt></dl>
9. Shell Verbindung zur Sub CA herstellen
10. Mit dem Befehl `certutil -installcert c:\CertDateiName.p7b` das Zertifikat installieren. <dl><dt>[![Install-Enterprise-CA 042-01.png](https://wiki.eidolf.de/images/5/57/Install-Enterprise-CA_042-01.png)](https://wiki.eidolf.de/index.php/Datei:Install-Enterprise-CA_042-01.png)</dt></dl>
11. Den Certsrv Dienst durchstarten

## <span class="mw-headline" id="bkmrk-erweiterte-aufgabe-1">Erweiterte Aufgabe</span>

### <span class="mw-headline" id="bkmrk-san-zertifikat-erste-1">SAN Zertifikat erstellen</span>

#### <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-iis-wizard-%28certsrv%29-1">IIS Wizard (certsrv)</span>

Unter dem Attribut Feld gibt man SAN:DNS=SAN.FQDN ein.  
Jeder weitere Eintrag wird durch ein kaufmännisches UND "&amp;" getrennt

Beispiel:

```
SAN:DNS=eins.domain.de & DNS=zwei.domain.de
```

## <span class="mw-headline" id="bkmrk-einstellungen-1">Einstellungen</span>

### <span class="mw-headline" id="bkmrk-bestehende-sha1-ca-a-1">Bestehende SHA1 CA auf SHA256 upgraden</span>

1. CMD öffnen
2. `certutil -setreg ca\csp\CNGHashAlgorithm SHA256`
3. CA Dienst neustarten
4. Eigenschaften der CA überprüfen
5. CA Zertifikat erneuern

#### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://www.frankysweb.de/migration-stammzertifizierungsstelle-sha1-zu-sha256-hashalgorithmus/" rel="nofollow">https://www.frankysweb.de/migration-stammzertifizierungsstelle-sha1-zu-sha256-hashalgorithmus/</a>
```

### <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-zertifikat-kann-nich-1">Zertifikat kann nicht unter "Ausstellende Zertifikatsvorlage" ausgewählt werden</span>

Lösung:

1. ADSI Edit öffnen
2. Auf `CN=Configuration | CN=Services | CN=Public Key Services | CN=Enrollment Services` wechseln
3. Eigenschaften der Zertifizierungsstelle auswählen, bei der die Vorlagen nicht ausgewählt werden können
4. Attribut **flag** von **2** auf **10** setzen
5. Dienst der betroffenen CA neu starten

#### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://serverfault.com/questions/320565/certificate-template-missing-from-certificate-template-to-issue" rel="nofollow">https://serverfault.com/questions/320565/certificate-template-missing-from-certificate-template-to-issue</a>
```