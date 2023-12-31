# AD Additional Account Info

Wenn diese .dll Datei, an einem Client, registriert wird der die AD Benutzer und Computer Verwaltung installiert hat kann man zusätzliche Informationen einzelner Benutzer im AD abrufen.  
**Intallation:**

<div class="vector-body" id="bkmrk-die-acctinfo2.dll-mu"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Die Acctinfo2.dll muss zuerst ins Verzeichnis %windir% verschoben werden.Im nächsten Schritt ist diese DLL, entweder unter Start - Ausführen oder in einer Kommandozeile mit dem Befehl regsvr32 acctinfo2.dll zu registrieren.
2. Als nächstes ruft man ADSIEdit (aus den Windows Support Tools) auf und navigiert in der Konfigurationspartition zu folgendem Pfad: CN=user-Display,CN=407,CN=DisplaySpecifiers,CN=Configuration,DC=Domäne,DC=TLD  
    Hinweis: Läuft der Windows Server mit einer englischen Betriebssystem Version, so lautet die Länderkennung nicht CN=407 sondern CN=409.
3. Dort angelangt, sind die Eigenschaften von CN=user-Display aufzurufen.Anschließend gilt es im Attribut adminPropertyPages den folgenden Eintrag hinzuzufügen:  
    2,{5969F63F-CACF-40bf-8891-CA580EB589E9}
4. Am Anfang des Eintrags (2,..) ist ein freier oder der nächst höhere Wert zu wählen, gefolgt von einem Komma und der GUID der Acctinfo2.dll (in geschweiften Klammern).
5. Danach steht ab sofort der neue Reiter in den Benutzereigenschaften zur Verfügung. Weiterhin ist es möglich, dass beide Versionen (1.x sowie 2.x) nebeneinander existieren.

</div></div></div>  
  
64Bit Version Download [ACCTINFO2 64bit](https://wiki.eidolf.de/index.php?title=Spezial:Hochladen&wpDestFile=ACCTINFO2_64bit.zip.rar "ACCTINFO2 64bit.zip.rar") ( in ACCTINFO2\_64bit.zip umbenennen )

  
Quellen:

```
<a class="external free" href="http://blog.dikmenoglu.de/PermaLink,guid,203db19c-bd2e-4e66-8510-986432a34cad.aspx" rel="nofollow">http://blog.dikmenoglu.de/PermaLink,guid,203db19c-bd2e-4e66-8510-986432a34cad.aspx</a>
```