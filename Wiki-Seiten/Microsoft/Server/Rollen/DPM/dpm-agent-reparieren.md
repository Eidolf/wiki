# DPM Agent reparieren

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-1-m%C3%B6glichkeit-1">1 Möglichkeit</span>

<div class="vector-body" id="bkmrk-den-dpm-server-dem-l"><div class="mw-body-content mw-content-ltr" dir="ltr" id="bkmrk-den-dpm-server-dem-l-1" lang="de"><div class="mw-parser-output">1. Den DPM Server dem lokalen Agent wieder bekannt machen, hier werden einfach die letzten Punkte der manuellen Installation eingehalten.
2. Auf dem Client folgenden Befehl ausführen 
    - &lt;Laufwerksbuchstabe&gt;:\\Programme\\Microsoft Data Protection Manager\\DPM\\bin\\SetDpmServer.exe -dpmServerName &lt;DPM Server Name&gt;
    
      
    Beispiel: ```
    SetDpmServer.exe –dpmServerName DPM01
    ```
3. Falls nach einer Aktulisierung am Server immer noch ein Fehler auftritt, am Server folgenden Befehl in die DPM Shell eingeben 
    - Attach-ProductionServer.ps1 &lt;DPM server name&gt; &lt;production server name&gt; &lt;user name&gt; &lt;password&gt; &lt;domain&gt;
    
    Das Passwort muss nicht unbedingt angegeben werden, da er eine Abfrage startet

</div></div></div>