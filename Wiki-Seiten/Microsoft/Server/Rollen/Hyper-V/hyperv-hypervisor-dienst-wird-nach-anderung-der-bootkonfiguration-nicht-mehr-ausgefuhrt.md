# HyperV Hypervisor Dienst wird nach Änderung der Bootkonfiguration nicht mehr ausgeführt

Dieses Problem tritt auf, weil es kein Sysprep-Anbieter für Hyper-V ist. Daher ist der HypervisorLaunchType BCD-Eintrag aus der Datei Startkonfigurationsdatenbank (Boot Configuration Data, BCD) entfernt, wenn Sie Sysprep ausführen.

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span>

<div class="vector-body" id="bkmrk-%C3%96ffnen-sie-auf-dem-c"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Öffnen Sie auf dem Computer, auf dem das Abbild bereitgestellt, wird, eine Eingabeaufforderung.
2. Verschieben Sie an der Eingabeaufforderung in das folgende Verzeichnis:%Windir%\\System32
3. Geben Sie den folgenden Befehl ein, und drücken Sie die \[EINGABETASTE\]:  
    ```
    Bcdedit /set {current} hypervisorlaunchtype auto
    ```
4. Starten Sie den Computer neu.

</div></div></div># <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://support.microsoft.com/kb/954356" rel="nofollow">http://support.microsoft.com/kb/954356</a>
```