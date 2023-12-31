# Sicherung von Domaincontrollern

Es wird nicht empfohlen bei multiplen Domänencontrollern ausschließlich die virtuelle Festplatte zu sichern, da der Status des Sicherungsvorgangs so nicht an das AD weitergegeben wird und somit bei einer Wiederherstellung USN (Update Sequence Number) Rollback Situation auftreten kann, bedeutet alte/falsche Daten werden synchronisiert.

# <span class="mw-headline" id="bkmrk-quellen%3A">Quellen:</span><span class="mw-editsection"><span class="mw-editsection-bracket">\[</span>[Bearbeiten](https://wiki.eidolf.de/index.php?title=Sicherung_von_Domaincontrollern&action=edit&section=1 "Abschnitt bearbeiten: Quellen:")<span class="mw-editsection-bracket">\]</span></span>

```
<a class="external free" href="http://www.altaro.com/hyper-v/how-to-back-up-and-restore-domain-controllers-virtualized-on-hyper-v/" rel="nofollow">http://www.altaro.com/hyper-v/how-to-back-up-and-restore-domain-controllers-virtualized-on-hyper-v/</a>
<a class="external free" href="http://social.technet.microsoft.com/Forums/en-US/dpmhypervbackup/thread/ae13f97d-4ffe-4210-b29c-7f2aa85cd022" rel="nofollow">http://social.technet.microsoft.com/Forums/en-US/dpmhypervbackup/thread/ae13f97d-4ffe-4210-b29c-7f2aa85cd022</a>
<a class="external free" href="http://blogs.technet.com/b/askds/archive/2011/04/21/disk-image-backups-and-multi-master-databases-or-how-to-avoid-early-retirement.aspx" rel="nofollow">http://blogs.technet.com/b/askds/archive/2011/04/21/disk-image-backups-and-multi-master-databases-or-how-to-avoid-early-retirement.aspx</a>
```