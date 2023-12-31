# AD Änderungen in Ereignisanzeige Protokollieren

# <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-schritt-1%3A-aktiviere-1">Schritt 1: Aktivieren der Überwachungsrichtlinie.</span>

Dieser Schritt enthält Verfahren zum Aktivieren der Änderungsüberwachung mithilfe der Windows-Benutzeroberfläche oder einer Befehlszeile: Wenn Sie die Gruppenrichtlinienverwaltung verwenden, können Sie die globale Überwachungsrichtlinie Verzeichnisdienstzugriff überwachen aktivieren, die alle Unterkategorien der AD DS-Überwachung aktiviert. Wenn Sie die Gruppenrichtlinienverwaltung installieren müssen, klicken Sie im Server-Manager auf Features hinzufügen. Wählen Sie Gruppenrichtlinienverwaltung aus, und klicken Sie dann auf Installieren.

  
Mithilfe des Befehlszeilentools Auditpol können Sie einzelne Unterkategorien aktivieren.

So aktivieren Sie die globale Überwachungsrichtlinie mithilfe der Windows-Benutzeroberfläche

1\. Klicken Sie auf Start, zeigen Sie auf Verwaltung und anschließend auf Gruppenrichtlinienverwaltung.

2\. Doppelklicken Sie in der Konsolenstruktur auf den Namen der Gesamtstruktur, doppelklicken Sie auf Domänen, doppelklicken Sie auf den Namen der Domäne, doppelklicken Sie auf Domänencontroller, klicken Sie mit der rechten Maustaste auf Standard-Domänencontrollerrichtlinie, und klicken Sie dann auf Bearbeiten.

3\. Doppelklicken Sie unter Computerkonfiguration auf Richtlinien, doppelklicken Sie auf Windows-Einstellungen, doppelklicken Sie auf Sicherheitseinstellungen, doppelklicken Sie auf Lokale Richtlinien und auf Überwachungsrichtlinie.

4\. Klicken Sie in Überwachungsrichtlinie mit der rechten Maustaste auf Verzeichnisdienstzugriff überwachen, und klicken Sie dann auf Eigenschaften.

5\. Aktivieren Sie das Kontrollkästchen Diese Richtlinieneinstellungen definieren.

6\. Aktivieren Sie unter Diese Versuche überwachen das Kontrollkästchen Erfolgreich, und klicken Sie dann auf OK.

  
So aktivieren Sie die Änderungsüberwachungsrichtlinie mithilfe einer Befehlszeile

1\. Klicken Sie auf Start, klicken Sie mit der rechten Maustaste auf Eingabeaufforderung, und klicken Sie dann auf Als Administrator ausführen.

2\. Geben Sie den folgenden Befehl ein, und drücken Sie dann die EINGABETASTE:

auditpol /set /subcategory:"directory service changes" /success:enable

# <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-schritt-2%3A-einrichte-1">Schritt 2: Einrichten der Überwachung in Objekt-SACLs.</span>

Das folgende Verfahren ist nur ein Beispiel der vielen unterschiedlichen Typen von SACLs, die Sie für Vorgänge festlegen können, die Sie überwachen möchten.

So richten Sie die Überwachung in Objekt-SACLs ein

1\. Klicken Sie auf Start, zeigen Sie auf Verwaltung, und klicken Sie dann auf Active Directory-Benutzer und -Computer.

2\. Klicken Sie mit der rechten Maustaste auf die Organisationseinheit, für die die Überwachung aktiviert werden soll, und klicken Sie dann auf Eigenschaften.

3\. Klicken Sie auf der Registerkarte Sicherheit auf Erweitert und anschließend auf die Registerkarte Überwachung.

4\. Klicken Sie auf Hinzufügen, und geben Sie Authentifizierte Benutzer (oder einen anderen Sicherheitsprinzipal) unter Geben Sie die zu verwendenden Objektnamen ein ein. Klicken Sie dann auf OK.

5\. Klicken Sie in Übernehmen für auf Untergeordnete "Benutzer"-Objekte (oder beliebige andere Objekte).

6\. Aktivieren Sie unter Zugriff das Kontrollkästchen Erfolgreich für Alle Eigenschaften schreiben.

7\. Klicken Sie auf OK, bis Sie die Eigenschaftenseite für die Organisationseinheit bzw. ein anderes Objekt geschlossen haben.

## <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=AD_%C3%84nderungen_in_Ereignisanzeige_Protokollieren&action=edit&section=3 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://technet.microsoft.com/de-de/library/cc731607(v=ws.10).aspx" rel="nofollow">http://technet.microsoft.com/de-de/library/cc731607(v=ws.10).aspx</a>
```