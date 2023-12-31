# Outlook ersetzt automatisch die RPC over HTTP Einstellungen

Mit folgendem Befehl überprüfen ob der Zertifikatsname eingetragen ist unter **CertPrincipalName**

```
Get-OutlookProvider |FL
```

Wenn nichts eingetragen ist wird der Interne Name verwendet z.B. bei intern.domamin.local → msstd:intern.domain.local  
Da man aber den Externen namen haben will muss man folgenden Befehl eingeben um hier den Standard zu setzen.

```
Set-OutlookProvider EXPR -CertPrincipalName msstd:mail.company.org
```

# <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.experts-exchange.com/Software/Office_Productivity/Groupware/Outlook/Q_24815951.html" rel="nofollow">http://www.experts-exchange.com/Software/Office_Productivity/Groupware/Outlook/Q_24815951.html</a>
<a class="external free" href="http://blogs.technet.com/b/exchange/archive/2008/09/29/3406352.aspx" rel="nofollow">http://blogs.technet.com/b/exchange/archive/2008/09/29/3406352.aspx</a>
```