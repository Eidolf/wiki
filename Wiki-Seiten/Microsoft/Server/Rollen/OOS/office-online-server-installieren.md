# Office Online Server installieren

# <span class="mw-headline" id="bkmrk-installation-1">Installation</span>

<div class="vector-body" id="bkmrk-server-2016-rollen-i"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Server 2016 Rollen installieren <dl><dd></dd><dd>`Add-WindowsFeature Web-Server,Web-Mgmt-Tools,Web-Mgmt-Console,Web-WebServer,Web-Common-Http,Web-D`</dd><dd>`efault-Doc,Web-Static-Content,Web-Performance,Web-Stat-Compression,Web-Dyn-Compression,Web-Security,Web-Filtering,Web-Windows-Auth,Web-App-Dev,Web-Net-Ext45,Web-Asp-Net45,Web-ISAPI-Ext,Web-ISAPI-Filter,Web-Includes,NET-Framework-Features,NET-Framework-45-Features,NET-Framework-Core,NET-Framework-45-Core,NET-HTTP-Activation,NET-Non-HTTP-Activ,NET-WCF-HTTP-Activation45,Windows-Identity-Foundation,Server-Media-Foundation`</dd></dl>
2. Installation folgender weiteren Voraussetzungen 
    1. [MicrosoftIdentityExtensions-64](https://go.microsoft.com/fwlink/p/?LinkId=620072)
    2. [vc\_redist.x64-2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145)
    3. [vcredist\_x64-2013](https://www.microsoft.com/en-us/download/details.aspx?id=40784)
3. Office Online Server installer ausführen (Am Besten die englische Version, da in der deutschen {Stand 09/2017} gefühlt ein Bug implementiert ist)
4. Am IIS ein Zertifikat anfordern und bei der Zertifizierungsstelle einreichen
5. Am IIS die Zertifikatsanforderung abschließen und unbedingt eine Namen angeben, siehe bei nachfolgendem Power Shell Befehl den "CertificateName"
6. Power Shell öffnen und nachfolgenden Befehl eingeben um eine Farm zu erstellen <dl><dd>`New-OfficeWebAppsFarm -InternalURL "<a class="external free" href="https://oos.xn--domne-ira.de/" rel="nofollow">https://oos.domäne.de</a>" -ExternalURL "<a class="external free" href="https://oos.xn--domne-ira.de/" rel="nofollow">https://oos.domäne.de</a>" -CertificateName "oos.domäne.de"`</dd></dl>
    1. Mit folgendem Befehl kann online Editiert werden (Lizenz vorausgesetzt) <dl><dd>`New-OfficeWebAppsFarm -InternalURL "<a class="external free" href="https://oos.xn--domne-ira.de/" rel="nofollow">https://oos.domäne.de</a>" -ExternalURL "<a class="external free" href="https://oos.xn--domne-ira.de/" rel="nofollow">https://oos.domäne.de</a>" -CertificateName "oos.domäne.de" -EditingEnabled`</dd></dl>
7. Grober Test: Seite **https://oos.domäne.de/hosting/discovery** aufrufen, es sollte eine xml Datei angezeigt werden.
8. Ab jetzt müssen Exchange Einstellungen gesetzt werden

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://docs.microsoft.com/de-de/officeonlineserver/deploy-office-online-server" rel="nofollow">https://docs.microsoft.com/de-de/officeonlineserver/deploy-office-online-server</a>
<a class="external free" href="https://jaapwesselius.com/2016/12/08/install-office-online-server-2016/" rel="nofollow">https://jaapwesselius.com/2016/12/08/install-office-online-server-2016/</a>
<a class="external free" href="https://cloudmoran.wordpress.com/2016/08/22/deploy-office-online-server-owa-aka-wac-with-skype-for-business-server-2015/" rel="nofollow">https://cloudmoran.wordpress.com/2016/08/22/deploy-office-online-server-owa-aka-wac-with-skype-for-business-server-2015/</a>
```