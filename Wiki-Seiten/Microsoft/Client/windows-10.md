# Windows 10

# <span class="mw-headline" id="bkmrk-benutzer-automatisch-1">Benutzer automatisch anmelden</span>

So wie ich es bisher gesehen habe funktioniert es nur bei Rechnern die nicht in einer Domäne sind, für Domänen verbundene Rechner ist im Quellenlink etwas zu finden.

<div class="vector-body" id="bkmrk-netplwiz%C2%A0unter-ausf%C3%BC"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. `netplwiz` unter Ausführen eingeben
2. Haken entfernen bei "Benutzer müssen Benutzernamen und Kennwort eingeben"
3. Benutzername und Passwort eingeben und speichern
4. Nach einem Neustart wird die Anmeldung automatisch durchgeführt.

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://www.tenforums.com/tutorials/3539-sign-user-account-automatically-windows-10-startup.html" rel="nofollow">https://www.tenforums.com/tutorials/3539-sign-user-account-automatically-windows-10-startup.html</a>
```

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-tempor%C3%A4res-profil-wi-1">Temporäres Profil wird geladen</span>

<div class="vector-body" id="bkmrk-melden-sie-sich-mit-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Melden Sie sich mit einem Administratorkonto beim System an, verwenden Sie aber nicht das Benutzerkonto, bei dem das Problem auftritt.
2. Sichern Sie alle Daten im Profilordner des aktuellen Benutzers, wenn dieser Profilordner noch vorhanden ist, und löschen Sie den Profilordner anschließend. Das Profil befindet sich standardmäßig an folgendem Speicherort: %SystemDrive%\\Users\\Benutzername
3. Klicken Sie auf Start, geben Sie in das Feld Suche starten die Zeichenfolge regedit ein, und drücken Sie anschließend die EINGABETASTE. <dl><dd>Wenn Sie aufgefordert werden, ein Administratorkennwort oder eine Bestätigung einzugeben, geben Sie Ihr Kennwort ein, oder klicken Sie auf Weiter.</dd></dl>
4. Suchen Sie nach dem folgenden Registrierungsunterschlüssel: <dl><dd>`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList`</dd></dl>
5. Löschen Sie unter dem Unterschlüssel „ProfileList“ den Unterschlüssel mit dem Namen „SID.bak“. <dl><dd>Hinweis SID ist ein Platzhalter für die Sicherheits-ID (SID) des Benutzerkontos, bei dem das Problem auftritt. Der Unterschlüssel „SID.bak“ sollte einen Registrierungseintrag „ProfileImagePath“ enthalten, der auf den ursprünglichen Profilordner verweist, bei dem das Problem auftritt.</dd></dl>
6. Beenden Sie den Registrierungs-Editor.
7. Melden Sie sich vom System ab.
8. Melden Sie sich erneut beim System an.

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-3">Quelle:</span>

```
<a class="external free" href="https://support.microsoft.com/de-de/help/947242/a-temporary-profile-is-loaded-after-you-log-on-to-a-windows-vista-base" rel="nofollow">https://support.microsoft.com/de-de/help/947242/a-temporary-profile-is-loaded-after-you-log-on-to-a-windows-vista-base</a>
```