# Maria DB

# <span class="mw-headline" id="bkmrk-upgrade-1">Upgrade</span>

## <span class="mw-headline" id="bkmrk-linux-1">Linux</span>

Eine ausführliche Version inkl. Backup (welches bei mir nicht funktioniert hat) ist in der Quelle zu finden.  
Hier stehen nur Befehle für ein schnelles Upgrade bei dem bestehende Datenbanken aller Voraussicht vorhanden bleiben.

<div class="vector-body" id="bkmrk-mari-db-stoppen%C2%A0sudo"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Mari DB stoppen `sudo systemctl stop mariadb`
2. Mari DB entfernen `sudo apt remove "mariadb-*"`
3. Galera entfernen `sudo apt remove galera-4` (bei mir war es galera-3 auf 18.04)
4. Prüfen ob noch eine Version vorhanden ist `apt list --installed | grep -i -E "mariadb|galera"`
5. Upgrade des Repository 
    1. `sudo apt-get install software-properties-common`
    2. `sudo apt-key adv --fetch-keys '<a class="external free" href="https://mariadb.org/mariadb_release_signing_key.asc'" rel="nofollow">https://mariadb.org/mariadb_release_signing_key.asc'</a>`
    3. `sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] <a class="external free" href="https://mirror.dogado.de/mariadb/repo/10.5/ubuntu" rel="nofollow">https://mirror.dogado.de/mariadb/repo/10.5/ubuntu</a> bionic main'`
6. Apt neu einlesen `sudo apt update`
7. Installieren `sudo apt install mariadb-server mariadb-backup`
8. Bestehende Datenbanken upgraden `sudo mariadb-upgrade --force`
9. Versionscheck `sudo mariadb`

</div></div></div>### <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="https://itblog.webdigg.org/471-upgrade-mariadb-10-1-to-10-5-on-debian-stretch/" rel="nofollow">https://itblog.webdigg.org/471-upgrade-mariadb-10-1-to-10-5-on-debian-stretch/</a>
<a class="external free" href="https://downloads.mariadb.org/mariadb/repositories/#distro=Ubuntu&distro_release=bionic--ubuntu_bionic&mirror=timo&version=10.5" rel="nofollow">https://downloads.mariadb.org/mariadb/repositories/#distro=Ubuntu&distro_release=bionic--ubuntu_bionic&mirror=timo&version=10.5</a>
```