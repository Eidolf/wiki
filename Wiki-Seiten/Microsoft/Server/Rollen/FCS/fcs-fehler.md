---
title: fcs-fehler
description: 
published: true
date: 2023-12-31T14:34:20.240Z
tags: 
editor: markdown
dateCreated: 2023-12-31T14:34:17.036Z
---

# FCS Fehler

# <span class="mw-headline" id="bkmrk-fcs-agent-deinstalli-1">FCS Agent deinstalliert sich selbst</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=FCS_Fehler&action=edit&section=1 "Abschnitt bearbeiten: FCS Agent deinstalliert sich selbst")<span class="mw-editsection-bracket">\]</span></span>

Dies tritt evtl. nach einem Agent Update auf. Es wird als erstes versucht FCS zu deinstallieren, wenn dies bei der hälfte fehlschlägt kann er nicht mehr das Update installieren und somit ist es wie wenn FCS sich selbst gelöscht hat.

## <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=FCS_Fehler&action=edit&section=2 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

Eine direkte und schnell Lösung gibt es nicht, man muss nur alles richtig deinstallieren und danach das Update installieren.

# <span class="mw-headline" id="bkmrk-fcs-mmc-absturz">FCS MMC absturz</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=FCS_Fehler&action=edit&section=3 "Abschnitt bearbeiten: FCS MMC absturz")<span class="mw-editsection-bracket">\]</span></span>

Die Managemant Konsole stürzt ab und als Fehler wird angezeigt **"Stapelüberwachung: System.Reflection.TargetInvocationExcepiton"**

## <span id="bkmrk--2"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-1">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=FCS_Fehler&action=edit&section=4 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

Hotfix installieren --&gt; siehe Quelle

## <span class="mw-headline" id="bkmrk-quelle%3A">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=FCS_Fehler&action=edit&section=5 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://support.microsoft.com/kb/942581/en-us" rel="nofollow">http://support.microsoft.com/kb/942581/en-us</a>
```

# <span id="bkmrk--4"></span><span class="mw-headline" id="bkmrk-fcs-verwaltungskonso-1">FCS Verwaltungskonsole für Reports kann sich nicht mehr Verbinden</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=FCS_Fehler&action=edit&section=6 "Abschnitt bearbeiten: FCS Verwaltungskonsole für Reports kann sich nicht mehr Verbinden")<span class="mw-editsection-bracket">\]</span></span>

Nach dem Verbindungsversuch folgende Meldung angezeigt. **" Es konnte keine Verbindung zum eingegebenen Server hergestellt werden, oder der Server ist kein Host für die MOM-Dateizugriffsdienste. Geben Sie einen anderen Servernamen ein. "**  
In der Ereignisanzeige werden folgende Meldungen angezeigt:

```
Event ID 9000

MOM Engine of management Group ForefronClient Security failed to initialize
```

```
Event ID 9029

The Microsoft Operation Manager Service (MOMService.exe) was unable to run under the supplied credentials,or the Password has expired.
```

## <span id="bkmrk--5"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung%3A-2">Lösung:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=FCS_Fehler&action=edit&section=7 "Abschnitt bearbeiten: Lösung:")<span class="mw-editsection-bracket">\]</span></span>

<div class="vector-body" id="bkmrk-%C3%9Cber-cmd-in-den-mom-"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Über CMD in den MOM Ordner Verbinden &gt; c:\\Programme\\Forefront Client Security\\Server\\MOM (gerade nicht sicher beim Pfad, sonst ändern)
2. SetActionAccount.exe ForefrontClientSecurity -query eingeben
3. Wenn der Action Account richtig ist mit Punkt 4 weitermachen 
    1. Neues Action Account Konto angeben
    2. SetActionAccount.exe ForefrontClientSecurity -set &lt;Domänenname&gt; &lt;Names-des-Action-Accounts&gt;
    3. Passwort eingeben
4. Start Component Services. To do this, click Start, click Run, type dcomcnfg in the Open box, and then click OK.
5. In Component Services, double-click Component Services, double-click Computers, double-click My Computer, and then double-click COM+ Applications.
6. Right-click Microsoft Operations Manager Data Access Server, and then click Properties.
7. Click the Identity tab.
8. Type the correct password for the DAS account in the Password box and in the Confirm password box, and then click OK.
9. Exit Component Services.

</div></div></div>## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=FCS_Fehler&action=edit&section=8 "Abschnitt bearbeiten: Quelle:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://social.technet.microsoft.com/Forums/en-US/Forefrontclientgeneral/thread/ae636674-9041-4d77-b250-c32c7ee73bbe/" rel="nofollow">http://social.technet.microsoft.com/Forums/en-US/Forefrontclientgeneral/thread/ae636674-9041-4d77-b250-c32c7ee73bbe/</a>
```