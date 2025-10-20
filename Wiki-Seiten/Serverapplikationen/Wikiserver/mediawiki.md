---
title: mediawiki
description: 
published: true
date: 2025-10-14T10:06:56.322Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:37:04.273Z
---

# LDAP Authentifizierung

1. Download des LDAP Paketes [Mediawiki Download](https://www.mediawiki.org/wiki/Special:ExtensionDistributor/LdapAuthentication)
2. Entpacken und am Webserver in den Mediawiki **Extensions** Ordner kopieren
3. apt-get install -y php-ldap
4. Root-CA.crt und Sub-CA.crt Base64 Zertifikate (Ich glaube es braucht beide) in `/usr/local/share/ca-certificates` kopieren.
5. Zertifikate automatisch updaten`update-ca-certificates`
6. LDAP Config bearbeiten `nano /etc/ldap/ldap.conf`
7. und folgendes einfügen.

```
TLS_CACERTDIR   /etc/ssl/certs
TLS_CACERT      /etc/ssl/certs/ca-certificates.crt
```

8. LocalSettings.php bearbeiten

```
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

9. Webserver neu laden `service apache2 reload`

## Lokale Benutzer hinzufügen

Wenn das LDAP Plugin aktiviert ist, kann man keine lokalen Benutzer über die GUI anlegen.  
Folgender Fehler wird ungefähr angezeigt: **Benutzername ist bereits vergeben, bitte wählen Sie einen anderen Namen**  
Für einzelne Benutzer kann man folgenden Befehl über die Konsole des Webserver ausführen, zuvor sollte man in den Stammpfad von Mediawiki wechseln.  
`php maintenance/createAndPromote.php <em>Benutzername Passwort</em>`

### Quelle:
https://phabricator.wikimedia.org/T196453