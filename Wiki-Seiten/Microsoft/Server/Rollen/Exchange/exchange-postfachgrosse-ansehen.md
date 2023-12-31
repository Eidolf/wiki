# Exchange Postfachgröße ansehen

Befehl eingeben

```
Get-MailboxStatistics -server "servername" | sort $_.TotalItemSize | FT DisplayName,TotalItemSize
```