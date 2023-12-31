# Kennwortalter abfragen

# <span class="mw-headline" id="bkmrk-additional-account-i-1">Additional Account Info</span>

Siehe Link [Additional Account Info](https://wiki.eidolf.de/index.php/Additional_Account_Info "Additional Account Info")

# <span class="mw-headline" id="bkmrk-dsquery-1">dsquery</span>

Folgenden Befehl eingeben um alle Benutzer in eine Textdatei zu schreiben die Ihr Passwort länger als 30 Tage nicht geändert haben. Das Limit kann man erhöhen sonst gibt er Standardmäßig nur 100 Einträge aus.

```
dsquery user -stalepwd 30 > Benutzer.txt -limit 200
```