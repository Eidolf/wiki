# PS gesperrte Benutzer anzeigen

Aber die Suche nach den tatsächlich gesperrten und vor allem das Entsperren dieser Benutzerkonten, lässt sich ganz einfach realisieren. Die Lösung ist wieder einmal die AD-PowerShell! Mit diesem State of the Art Werkzeug lässt sich das schnell und einfach mit einem Einzeiler erledigen.  
  
Mit diesem Befehl werden alle tatsächlich gesperrten Benutzer angezeigt:

```
Search-ADAccount –LockedOut | FT Name
```

  
Mit dem folgenden Befehl werden alle tatsächlich gesperrten Benutzer entsperrt:

```
Search-ADAccount -LockedOut | Unlock-ADAccount
```

  
  
Quelle:

```
<a class="external free" href="http://blog.dikmenoglu.de/CategoryView,category,Active%2BDirectory,AD-Powershell.aspx" rel="nofollow">http://blog.dikmenoglu.de/CategoryView,category,Active%2BDirectory,AD-Powershell.aspx</a>
```