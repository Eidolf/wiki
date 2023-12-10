::: {#mw-page-base .noprint}
:::

::: {#mw-head-base .noprint}
:::

::: {#content .mw-body role="main"}
[]{#top}

::: {#siteNotice}
:::

::: mw-indicators
:::

# Internet Explorer {#firstHeading .firstHeading}

::: {#bodyContent .vector-body}
::: {#siteSub .noprint}
Aus Eidolf Wiki
:::

::: {#contentSub}
:::

::: {#contentSub2}
:::

::: {#jump-to-nav}
:::

[Zur Navigation springen](#mw-head){.mw-jump-link} [Zur Suche
springen](#searchInput){.mw-jump-link}

::: {#mw-content-text .mw-body-content .mw-content-ltr lang="de" dir="ltr"}
::: mw-parser-output
::: {#toc .toc role="navigation" aria-labelledby="mw-toc-heading"}
::: {.toctitle lang="de" dir="ltr"}
## Inhaltsverzeichnis {#mw-toc-heading}

[]{.toctogglespan}
:::

-   [[1]{.tocnumber} [Internet Explorer ( Browser / Internet )
    Beschreibung]{.toctext}](#Internet_Explorer_(_Browser_/_Internet_)_Beschreibung)
-   [[2]{.tocnumber} [Tipps und Tricks]{.toctext}](#Tipps_und_Tricks)
    -   [[2.1]{.tocnumber} [Passive FTP Verbindung
        abschalten]{.toctext}](#Passive_FTP_Verbindung_abschalten)
-   [[3]{.tocnumber} [Quellen:]{.toctext}](#Quellen:)
:::

## []{#Internet_Explorer_.28_Browser_.2F_Internet_.29_Beschreibung}[Internet Explorer ( Browser / Internet ) Beschreibung]{#Internet_Explorer_(_Browser_/_Internet_)_Beschreibung .mw-headline}

Der Windows Internet Explorer (früher Microsoft Internet Explorer;
Abkürzung: IE oder MSIE) ist ein Webbrowser vom Softwarehersteller
Microsoft für das Betriebssystem Microsoft Windows. Seit Windows 95 A
ist der Internet Explorer fester Bestandteil von
Windows-Betriebssystemen. Bei älteren Windows-Versionen kann er
nachinstalliert werden. Die aktuelle Version ist Windows Internet
Explorer 8.\
Seit dem Erscheinen der siebten Generation des Webbrowsers, erschienen
im Jahre 2006, lautet der Produktname Windows Internet Explorer.\
Den Microsoft Internet Explorer gab es für einige Zeit auch in Versionen
für Mac OS und Unix-Derivate wie Solaris und HP-UX.

## [Tipps und Tricks]{#Tipps_und_Tricks .mw-headline}

### [Passive FTP Verbindung abschalten]{#Passive_FTP_Verbindung_abschalten .mw-headline}

-   Entweder den Haken unter \" Extras → Internetoptionen → Erweitert →
    Abschnitt: Browsen → Passives FTP verwenden \" herausnehmen
-   Oder als VB Script Speichern für den automatischen Einsatz z.B. in
    den Gruppenrichtlinien.

```{=html}
<!-- -->
```
    Const HKEY_CURRENT_USER = &H80000001

    strComputer = "."

    Set objRegistry=GetObject("winmgmts:\\" & _ 
    strComputer & "\root\default:StdRegProv")

    strKeyPath = "Software\Microsoft\Ftp"
    strValueName = "Use PASV"
    strValue = "no"
    objRegistry.SetStringValue HKEY_CURRENT_USER,strKeyPath, _
    strValueName,strValue

\

## [Quellen:]{#Quellen: .mw-headline}

    http://de.wikipedia.org/wiki/Internet_Explorer
    http://gallery.technet.microsoft.com/ScriptCenter/de-DE/628be64c-ec61-4d0e-9d62-dccd5fe0a096?persist=True

\
\--[Eidolf](/index.php?title=Benutzer:Eidolf&action=edit&redlink=1 "Benutzer:Eidolf (Seite nicht vorhanden)"){.new}
21:40, 16. Nov. 2009 (UTC)
:::

::: printfooter
Abgerufen von
„[http://wiki.eidolf.de/index.php?title=Internet_Explorer&oldid=57](http://wiki.eidolf.de/index.php?title=Internet_Explorer&oldid=57){dir="ltr"}"
:::
:::

::: {#catlinks .catlinks .catlinks-allhidden mw="interface"}
:::
:::
:::

::: {#mw-navigation}
## Navigationsmenü

::: {#mw-head}
### Meine Werkzeuge {#p-personal-label .vector-menu-heading}

::: vector-menu-content
-   [[Anmelden](/index.php?title=Spezial:Anmelden&returnto=Internet+Explorer "Sich anzumelden wird gerne gesehen, ist jedoch nicht zwingend erforderlich. [o]"){accesskey="o"}]{#pt-login}
:::

::: {#left-navigation}
### Namensräume {#p-namespaces-label .vector-menu-heading}

::: vector-menu-content
-   [[Seite](/index.php/Internet_Explorer "Seiteninhalt anzeigen [c]"){accesskey="c"}]{#ca-nstab-main}
-   [[Diskussion](/index.php?title=Diskussion:Internet_Explorer&action=edit&redlink=1 "Diskussion zum Seiteninhalt (Seite nicht vorhanden) [t]"){rel="discussion"
    accesskey="t"}]{#ca-talk}
:::

### Varianten [ausgeklappt]{.vector-menu-checkbox-expanded} [eingeklappt]{.vector-menu-checkbox-collapsed} {#p-variants-label .vector-menu-heading}

::: vector-menu-content
:::
:::

::: {#right-navigation}
### Ansichten {#p-views-label .vector-menu-heading}

::: vector-menu-content
-   [[Lesen](/index.php/Internet_Explorer)]{#ca-view}
-   [[Quelltext
    anzeigen](/index.php?title=Internet_Explorer&action=edit "Diese Seite ist geschützt. Ihr Quelltext kann dennoch angesehen und kopiert werden. [e]"){accesskey="e"}]{#ca-viewsource}
-   [[Versionsgeschichte](/index.php?title=Internet_Explorer&action=history "Frühere Versionen dieser Seite [h]"){accesskey="h"}]{#ca-history}
:::

### Weitere [ausgeklappt]{.vector-menu-checkbox-expanded} [eingeklappt]{.vector-menu-checkbox-collapsed} {#p-cactions-label .vector-menu-heading}

::: vector-menu-content
:::

::: {#p-search .vector-search-box role="search"}
<div>

### Suche

::: {#simpleSearch search-loc="header-navigation"}
:::

</div>
:::
:::
:::

::: {#mw-panel}
::: {#p-logo role="banner"}
[](/index.php/Hauptseite "Hauptseite"){.mw-wiki-logo}
:::

### Navigation {#p-navigation-label .vector-menu-heading}

::: vector-menu-content
-   [[Hauptseite](/index.php/Hauptseite "Hauptseite besuchen [z]"){accesskey="z"}]{#n-mainpage-description}
-   [[Letzte
    Änderungen](/index.php/Spezial:Letzte_%C3%84nderungen "Liste der letzten Änderungen in diesem Wiki [r]"){accesskey="r"}]{#n-recentchanges}
-   [[Zufällige
    Seite](/index.php/Spezial:Zuf%C3%A4llige_Seite "Zufällige Seite aufrufen [x]"){accesskey="x"}]{#n-randompage}
-   [[Hilfe zu
    MediaWiki](https://www.mediawiki.org/wiki/Special:MyLanguage/Help:Contents)]{#n-help-mediawiki}
:::

### Werkzeuge {#p-tb-label .vector-menu-heading}

::: vector-menu-content
-   [[Links auf diese
    Seite](/index.php/Spezial:Linkliste/Internet_Explorer "Liste aller Seiten, die hierher verlinken [j]"){accesskey="j"}]{#t-whatlinkshere}
-   [[Änderungen an verlinkten
    Seiten](/index.php/Spezial:%C3%84nderungen_an_verlinkten_Seiten/Internet_Explorer "Letzte Änderungen an Seiten, die von hier verlinkt sind [k]"){rel="nofollow"
    accesskey="k"}]{#t-recentchangeslinked}
-   [[Spezialseiten](/index.php/Spezial:Spezialseiten "Liste aller Spezialseiten [q]"){accesskey="q"}]{#t-specialpages}
-   [[Druckversion](javascript:print(); "Druckansicht dieser Seite [p]"){rel="alternate"
    accesskey="p"}]{#t-print}
-   [[Permanenter
    Link](/index.php?title=Internet_Explorer&oldid=57 "Dauerhafter Link zu dieser Seitenversion")]{#t-permalink}
-   [[Seiten­informationen](/index.php?title=Internet_Explorer&action=info "Weitere Informationen über diese Seite")]{#t-info}
-   [[Seite
    zitieren](/index.php?title=Spezial:Zitierhilfe&page=Internet_Explorer&id=57&wpFormIdentifier=titleform "Hinweise, wie diese Seite zitiert werden kann")]{#t-cite}
:::
:::
:::

-   [Diese Seite wurde zuletzt am 31. März 2018 um 00:28 Uhr
    bearbeitet.]{#footer-info-lastmod}
-   [Der Inhalt ist verfügbar unter der Lizenz [\'\'Creative Commons\'\'
    „Namensnennung -- Weitergabe unter gleichen
    Bedingungen"](https://creativecommons.org/licenses/by-sa/4.0/){.external
    rel="nofollow"}, sofern nicht anders
    angegeben.]{#footer-info-copyright}

```{=html}
<!-- -->
```
-   [[Datenschutz](/index.php/Eidolf_Wiki:Datenschutz "Eidolf Wiki:Datenschutz")]{#footer-places-privacy}
-   [[Über Eidolf
    Wiki](/index.php/Eidolf_Wiki:%C3%9Cber_Eidolf_Wiki "Eidolf Wiki:Über Eidolf Wiki")]{#footer-places-about}
-   [[Haftungsausschluss](/index.php/Eidolf_Wiki:Impressum "Eidolf Wiki:Impressum")]{#footer-places-disclaimer}

```{=html}
<!-- -->
```
-   [[![\'\'Creative Commons\'\' „Namensnennung -- Weitergabe unter
    gleichen
    Bedingungen"](/resources/assets/licenses/cc-by-sa.png){width="88"
    height="31"
    loading="lazy"}](https://creativecommons.org/licenses/by-sa/4.0/)]{#footer-copyrightico}
-   [[![Powered by
    MediaWiki](/resources/assets/poweredby_mediawiki_88x31.png){srcset="/resources/assets/poweredby_mediawiki_132x47.png 1.5x, /resources/assets/poweredby_mediawiki_176x62.png 2x"
    width="88" height="31"
    loading="lazy"}](https://www.mediawiki.org/)]{#footer-poweredbyico}
