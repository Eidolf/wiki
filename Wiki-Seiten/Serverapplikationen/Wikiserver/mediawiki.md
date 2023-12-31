# Mediawiki

# <span class="mw-headline" id="bkmrk-ldap-authentifizieru-1">LDAP Authentifizierung</span>

<div class="vector-body" id="bkmrk-download-des-ldap-pa"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Download des LDAP Paketes [Mediawiki Download](https://www.mediawiki.org/wiki/Special:ExtensionDistributor/LdapAuthentication)
2. Entpacken und am Webserver in den Mediawiki **Extensions** Ordner kopieren
3. apt-get install -y php-ldap
4. Root-CA.crt und Sub-CA.crt Base64 Zertifikate (Ich glaube es braucht beide) in `/usr/local/share/ca-certificates` kopieren.
5. Zertifikate automatisch updaten`update-ca-certificates`
6. LDAP Config bearbeiten `nano /etc/ldap/ldap.conf`
7. und folgendes einfügen.

</div></div></div>```
TLS_CACERTDIR   /etc/ssl/certs
TLS_CACERT      /etc/ssl/certs/ca-certificates.crt
```

<div class="vector-body" id="bkmrk-localsettings.php-be"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. LocalSettings.php bearbeiten

</div></div></div>```
require_once( "$IP/extensions/LdapAuthentication/LdapAuthentication.php" );
$wgAuth = new LdapAuthenticationPlugin();
$wgLDAPUseLocal = true;
$wgLDAPDomainNames = array( "DomainNamepreWin2000" );
$wgLDAPServerNames = array( "DomainNamepreWin2000" => "DC1.fqdn DC2.fqdn" );
$wgLDAPSearchStrings = array( "DomainNamepreWin2000" => "USER-NAME@DomainNamepreWin2000" );
$wgLDAPEncryptionType = array( "DomainNamepreWin2000" => "ssl" );
$wgLDAPRetrievePrefs = array( "DomainNamepreWin2000" => true);
$wgGroupPermissions['*']['autocreateaccount'] = true;
```

<div class="vector-body" id="bkmrk-webserver-neu-laden%C2%A0"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Webserver neu laden `service apache2 reload`

</div></div></div>## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-lokale-benutzer-hinz-1">Lokale Benutzer hinzufügen</span>

Wenn das LDAP Plugin aktiviert ist, kann man keine lokalen Benutzer über die GUI anlegen.  
Folgender Fehler wird ungefähr angezeigt: **Benutzername ist bereits vergeben, bitte wählen Sie einen anderen Namen**  
Für einzelne Benutzer kann man folgenden Befehl über die Konsole des Webserver ausführen, zuvor sollte man in den Stammpfad von Mediawiki wechseln.  
`php maintenance/createAndPromote.php <em>Benutzername Passwort</em>`

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://phabricator.wikimedia.org/T196453" rel="nofollow">https://phabricator.wikimedia.org/T196453</a>
```