# Office Online Server update

<div class="vector-body" id="bkmrk-da-mir-bei-normalen-"><div id="bkmrk-"></div><div id="bkmrk--1"></div><div id="bkmrk--2"></div><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"></div>Da mir bei normalen Windows Server Updates der Office Online Server und sein Vorgänger öfters abgeraucht sind, halte ich mich doch langsam an den Microsoft Hinweis vor den Updates die Farm zu löschen.  
Bedeutet folgenden Ablauf bei ALLEN Updatevorgängen an einem Office Online Server.  
Vorsicht! Alle speziellen Einstellungen gehen verloren, wenn also viel mehr eingestellt wurde als "EditingEnabled", dann sollte man ein Backup der Einstellungen ziehen.</div><div class="vector-body" id="bkmrk-powershell-%C3%B6ffnen-re"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-powershell-%C3%B6ffnen-re-1" lang="de"><div class="mw-parser-output">1. Powershell öffnen <dl><dd>`Remove-OfficeWebAppsMachine`</dd></dl>
2. Updates installieren
3. Neustart durchführen
4. Farm erneut erstellen (Nur ein Beispiel) <dl><dd>`New-OfficeWebAppsFarm -InternalURL "<a class="external free" href="https://oos.xn--domne-ira.de/" rel="nofollow">https://oos.domäne.de</a>" -ExternalURL "<a class="external free" href="https://oos.xn--domne-ira.de/" rel="nofollow">https://oos.domäne.de</a>" -CertificateName "oos.domäne.de" -EditingEnabled`</dd></dl>

</div></div></div>