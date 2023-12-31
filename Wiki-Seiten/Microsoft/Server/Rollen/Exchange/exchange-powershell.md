# Exchange Powershell

## <span class="mw-headline" id="bkmrk-skripte-1">Skripte</span>

### <span class="mw-headline" id="bkmrk-kontaktobjekte-anleg-1">Kontaktobjekte anlegen mit CSV import</span>

Überschriften in der CSV sind folgende:

<table class="wikitable" id="bkmrk-name-vorname-nachnam"><tbody><tr><th>Name</th><th>Vorname</th><th>Nachname</th><th>Alias</th><th>Anrede</th><th>Firma</th><th>Ort</th><th>Festnetz</th><th>Mobil</th><th>Mail</th><th>OU</th></tr></tbody></table>

```
$Kontakte = Import-Csv -Delimiter ";" C:\Tests\import.csv
$Kontakte | foreach {
New-MailContact -Name $_.Name -FirstName $_.Vorname -LastName $_.Nachname -Alias $_.Alias -ExternalEmailAddress $_.Mail -OrganizationalUnit $_.OU
}
$Kontakte | foreach {
Set-Contact -Identity $_.Name -Company $_.Firma -City $_.Ort -Phone $_.Festnetz -MobilePhone $_.Mobil
}
```