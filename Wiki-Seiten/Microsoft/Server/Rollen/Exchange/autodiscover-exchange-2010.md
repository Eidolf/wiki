# Autodiscover Exchange 2010

# <span class="mw-headline" id="bkmrk-zertifikat-auf-exter-1">Zertifikat auf externe Adresse ausstellen</span>

# <span class="mw-headline" id="bkmrk-webservices-virtual--1">Webservices Virtual Directory</span>

Um das "Webservices Virtual Directory" zu überprüfen

```
get-webservicesvirtualdirectory |fl
```

Um das "Webservices Virtual Directory" für eine externe Adresse einzurichten

```
Set-WebServicesVirtualDirectory -Identity "EWS*" -ExternalUrl "https://extern.adresse.de/EWS/Exchange.asmx"
```

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-iis-authentifizierun-1">IIS Authentifizierung ändern</span>

Bei den Ordner "EWS" und "OAB" die Standardauthentifizierung hinzufügen. **Nur im Fall das es keinen speziellen Listener an der Firewall gibt und der vorhandene auf Standard hört.**

# <span class="mw-headline" id="bkmrk-firewallregel-erstel-1">Firewallregel erstellen</span>