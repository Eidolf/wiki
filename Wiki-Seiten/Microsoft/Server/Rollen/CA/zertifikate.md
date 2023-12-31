# Zertifikate

# <span class="mw-headline" id="bkmrk-erweiterte-vorg%C3%A4nge-1">Erweiterte Vorgänge</span>

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-privaten-schl%C3%BCssel-w-1">Privaten Schlüssel wiederherstellen</span>

Problem:

CSR (Certificate Signing Request) wurde mit einem Exchange oder dem IIS erstellt. Es wurde vergessen anzuhaken das der Private Schlüssel exportierbar ist und somit gibt es keine ".key" Datei.

Die CSR wird an eine öffentliche Zertifizierungsstelle eingereicht, die einem das Zertifikat anschließend zusendet.

Um nun ein Zertifikat inkl. privatem Schlüssel zu erstellen benötigt man das zugesendete Zertifikat, eine Schlüsseldatei und evtl. eine Zwischenzertifizierungsstelle (siehe oben OpenSSL).

Das Problem ist, es gibt keine Schlüsseldatei zum erstellen.

Auch wenn man das Zertifikat am original Server der den CSR erstellt hat importiert, wird der private Schlüssel nicht erkannt/verknüpft.

  
Lösung:

1. Das Zertifikat am originalen Server importieren mit dem der CSR erstellt wurde.
2. Das Zertifikat öffnen und die Seriennummer kopieren.
3. Eine CMD öffnen und diesen Befehl eingeben ```
    certutil -repairstore my "SerialNumber"
    ```