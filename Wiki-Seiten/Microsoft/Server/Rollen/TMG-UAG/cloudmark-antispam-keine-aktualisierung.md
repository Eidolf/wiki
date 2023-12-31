# Cloudmark Antispam keine Aktualisierung

Alle Module lassen sich einwandfrei aktualisieren, nur das Antispammodul ( Cloudmark ) nicht.

<div class="vector-body" id="bkmrk-%C3%9Cberpr%C3%BCfen-ob-ein-pr"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Überprüfen ob ein Proxyserver eingestellt ist und dieser keine Authentifizierung benötigt.
2. Wenn dies der Fall ist --&gt; Richtilinienverwaltung --&gt; Globale Einstellungen --&gt; Moduloptionen --&gt; Proxyserver deaktivieren
3. Testweise auch mit Authentifizierung deaktivieren

</div></div></div>Da das Cloudmarkmodul scheinbar eine Authentifizierung benötigt kann ein Proxy einen anonymen Benutzer nicht an die Updateseite übergeben  
Eventuell noch diese Dienste durchstarten:

<div class="vector-body" id="bkmrk-%22-forefront-protecti"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. " Forefront Protection Controller " sollte auch nächste Dienste durchstarten, sonst manuell
2. " Exchange Transport "
3. " Exchange Information Store "

</div></div></div>Link:

```
<a class="external free" href="http://social.technet.microsoft.com/Forums/en/forefrontexchange/thread/b35414f4-e80b-42ff-92d6-b996af6913a0" rel="nofollow">http://social.technet.microsoft.com/Forums/en/forefrontexchange/thread/b35414f4-e80b-42ff-92d6-b996af6913a0</a>
```