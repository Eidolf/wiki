# Lync / S4B Hardware Telefone

## <span class="mw-headline" id="bkmrk-dhcp-voraussetzungen-1">DHCP Voraussetzungen</span>

1. Am DHCP Server müssen Optionen die genutzt werden vorbereitet werden. 
    - [Option 119](https://wiki.eidolf.de/index.php/DHCP#DHCP_Option_119 "DHCP")
    - [Option 120](https://wiki.eidolf.de/index.php/DHCP#DHCP_Option_120 "DHCP")
    - [Option MSUCClient](https://wiki.eidolf.de/index.php/DHCP#DHCP_Option_MSUCClient "DHCP")
2. Anschließend die werden die Optionen konfiguriert 
    1. Administrative CMD auf dem Skype for Business Server öffnen
    2. In den Pfad **C:\\Program Files\\Common Files\\Skype for Business Server 2019** wechseln (kommt natürlich auf die Serverversion an)
    3. `DHCPUtil.exe -SipServer PoolFQDN(meist Frontend) -WebServer FrontendFQDN(meist Frontend)`
    4. Mit den Angezeigten Werten kann man schlussendlich die DHCP Options erstellen. 
        1. Option 119 konfigurieren **!!! Eintrag stimmt nicht, es funktioniert kein String Wert ab Server 2012 !!!**
            1. Rechtsklick auf **Scope Options**
            2. Klick auf **Configure Options**
            3. Unter **General** die Option **119** suchen und anhaken
            4. Im Feld die Domains mit Anführungszeichen schreiben und jeweils mit Komma trennen <dl><dt></dt></dl>
        2. Option 120 konfigurieren 
            1. Rechtsklick auf **Scope Options**
            2. Klick auf **Configure Options**
            3. Unter **General** die Option **120** suchen und anhaken
            4. Im Binary Feld den zuvor ausgelesenen Wert einfügen <dl><dt>[![Option120Config.png](https://wiki.eidolf.de/images/thumb/1/17/Option120Config.png/300px-Option120Config.png)](https://wiki.eidolf.de/index.php/Datei:Option120Config.png)</dt></dl>
        3. Option MSUCClient konfigurieren 
            1. Rechtsklick auf **Scope Options**
            2. Klick auf **Configure Options**
            3. Unter **Advanced** die Vendor Class **MSUCClient** auswählen
            4. Alle einzelnen Optionen mit den jeweiligen ausgelesenen Binary Werten befüllen <dl><dt>[![OptionMSUConfig.png](https://wiki.eidolf.de/images/thumb/2/2c/OptionMSUConfig.png/300px-OptionMSUConfig.png)](https://wiki.eidolf.de/index.php/Datei:OptionMSUConfig.png)</dt></dl>

### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://blog.valeconsulting.co.uk/2015/03/06/configuring-dhcp-options-for-lync-phone-edition-manually/" rel="nofollow">https://blog.valeconsulting.co.uk/2015/03/06/configuring-dhcp-options-for-lync-phone-edition-manually/</a>
```