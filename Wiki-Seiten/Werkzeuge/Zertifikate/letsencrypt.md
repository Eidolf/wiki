# Letsencrypt

## <span class="mw-headline" id="bkmrk-certbot-windows-inst-1">Certbot Windows Installation</span>

Die modernste und einfachste Art ist in aktuellen Windows Versionen eine Installation mit winget

1. PowerShell öffnen
2. `winget install certbot`

## <span class="mw-headline" id="bkmrk-zertifikat-mit-csr-e-1">Zertifikat mit CSR erstellen</span>

Eigentlich ist es nicht vorgesehen einen CSR an Let's Encrypt einzureichen, doch für Testzwecke manchmal ein gangbarer Weg.

1. CSR erstellen und speichern
2. PowerShell öffnen
3. Bei Windows in den Ordner von Certbot wechseln, Standardpfad: `C:\Program Files\Certbot\bin`
4. Anfrage einreichen mit folgendem Befehl `./certbot certonly --manual --csr /path/to/file.csr --preferred-challenges dns`
5. Im Wizard wird durch das Flag **"--preferred-challenges dns"** am Ende gefordert einen DNS TXT Eintrag auf **"\_acme-challenge.&lt;YOUR\_DOMAIN&gt;"** zur Autorisierung zu setzen, somit sollte man Zugriff auf einen öffentlichen DNS haben.
6. Falls alles korrekt durchgelaufen ist, werden die Zertifikate im "Certbot\\bin" Ordner abgelegt. Beschreibung zu den einzelnen Dateien ist am Ende im Wizard sichtbar.

## <span class="mw-headline" id="bkmrk-zertifikate-erneuern-1">Zertifikate erneuern</span>

Der Vorgang ist relativ einfach, da eigentlich nur der Befehl `certbot` verwendet wird.  
Doch gibt es folgendes zu beachten falls ein Reverse Proxy verwendet wird und **certbot** / **letsencrypt** somit auf Port 443 das falsche Zertifikat angezeigt bekommt.  
Der Befehl um eine Abfrage über Port 80 zu erzwingen ist folgender:  
`certbot --preferred-challenges http`

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://certbot.eff.org/docs/using.html" rel="nofollow">https://certbot.eff.org/docs/using.html</a>
```

## <span class="mw-headline" id="bkmrk-san-zertifikate-erst-1">SAN Zertifikate erstellen</span>

`certbot -d wiki.eidolf.de -d www.eidolf.de --cert-name Zertifikatsnamen`

### <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://community.letsencrypt.org/t/adding-san-to-a-certificate/103954/2" rel="nofollow">https://community.letsencrypt.org/t/adding-san-to-a-certificate/103954/2</a>
```

## <span class="mw-headline" id="bkmrk-powershell-1">PowerShell</span>

### <span class="mw-headline" id="bkmrk-quelle%3A-5">Quelle:</span>

```
<a class="external free" href="https://mssec.wordpress.com/2019/10/25/using-powershell-to-get-wildcard-certificate-from-lets-encrypt/" rel="nofollow">https://mssec.wordpress.com/2019/10/25/using-powershell-to-get-wildcard-certificate-from-lets-encrypt/</a>
```