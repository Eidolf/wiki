---
title: nginx
description: Konfiguration für nginx Reverse Proxy
published: true
date: 2026-05-06T14:54:52.009Z
tags: linux, proxy, cli, reverse proxy, firewall
editor: markdown
dateCreated: 2026-05-04T15:37:13.516Z
---

# HTTPS zu bestimmten Port einrichten
Folgende Punkte muss man durchführen um über den Port 443 auf eine bestimmte Domäne (z.B. nginxconf.domain.de) auf einen anderen Port über nginx umzuleiten. Weiterhin wird das optionale certbot Zertfikat auf der Seite ausgerollte.

Voraussetzung ist das alle nötigen Tools installiert sind, das ist im Moment nicht Teil meiner Anleitung und wird bei viel Motivation nachgereicht.

## Voraussetzungen

- Ubuntu/Debian-Server
- nginx installiert
- certbot installiert (inkl. nginx-Plugin)
- DNS-Eintrag für `beispielanwendung.domain.de` zeigt auf den Server
- Anwendung läuft lokal auf `127.0.0.1:13060`

---

## 1. nginx Server-Block anlegen

`nano /etc/nginx/sites-available/beispielanwendung`

### Beispielkonfiguration

```
server {
    server_name beispielanwendung.domain.de;

    location / {
        proxy_pass http://localhost:13060;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Datei speichern und schließen.

## 2. Konfiguration testen

`nginx -t`

## 3. Site aktivieren

`ln -s /etc/nginx/sites-available/beispielanwendung /etc/nginx/sites-enabled`

`ls /etc/nginx/sites-enabled`

## 4. nginx neu starten

`systemctl restart nginx`

## 5. TLS-Zertifikat mit certbot erstellen (HTTPS / Port 443)

`certbot --nginx -d beispielanwendung.domain.de`

certbot passt die nginx-Konfiguration automatisch an und aktiviert HTTPS auf Port 443.

## 6. Finale Überprüfung

`nginx -t`

`systemctl reload nginx`

## Ergebnis

- Anwendung läuft intern auf `http://localhost:13060`
- Extern erreichbar über `https://beispielanwendung.domain.de`
- TLS-Zertifikat wird automatisch von certbot verwaltet


# SMTP (Port 25) umleiten oder separat konfigurieren

> **Hinweis:** SMTP (Port 25) ist **kein HTTP/HTTPS** und kann **nicht** über einen normalen `server {}`‑Block verarbeitet werden.  
> Dafür wird der **nginx stream‑Block** benötigt (TCP‑Proxy).
{.is-info}


## nginx **stream‑Modul** – prüfen und installieren

### Prüfen, ob das stream‑Modul verfügbar ist

Alternativ gezielt nach `--with-stream` suchen:

`nginx -V 2>&1 | grep -- --with-stream`

✅ Ausgabe enthält `--with-stream` oder `--with-stream=dynamic`
Hier ist aber wichtig zu beachten das
- with-stream schon vollständig ist
- with-stream=dynamic immer noch voraussetzt das Modul sauber zu installieren und zu laden, siehe [nginx#alternative-streammodul-als-dynamisches-modul](/de/Wiki-Seiten/Linux/nginx#alternative-streammodul-als-dynamisches-modul)

❌ Keine Ausgabe → stream‑Modul ist nicht vorhanden

### Installation des stream‑Moduls (Debian / Ubuntu)

#### Empfohlene Variante: nginx‑full installieren

```
apt update
apt install nginx-full
```
Diese Variante enthält das stream‑Modul standardmäßig.

#### Alternative: stream‑Modul als dynamisches Modul

`apt install libnginx-mod-stream`

Danach Modul aktivieren (falls nicht automatisch geladen):

> Beim automatisch laden wird das Modul in
> `/etc/nginx/modules-enabled/50-mod-stream.conf`
> geschrieben
> Aber erst ausprobieren ob es direkt nach der Installation funktioniert
{.is-info}


> Dies hier wirklich nur Optional durchführen!
> In `/etc/nginx/nginx.conf` **ganz oben**:
> 
> `load_module modules/ngx_stream_module.so;`
{.is-warning}


### Konfiguration testen

`nginx -t`

## Variante A: Port 25 auf einen internen SMTP‑Port umleiten

Beispiel: Weiterleitung von **Port 25 → Port 2525** auf dem gleichen Server.

### Voraussetzung

- nginx mit **stream‑Modul** (`nginx-full` oder entsprechendes Paket)
- Ziel‑SMTP‑Dienst läuft auf `127.0.0.1:2525`


### Konfiguration (separat, z. B. `/etc/nginx/stream.d/smtp.conf`)

```
stream {
    upstream smtp_backend {
        server 127.0.0.1:2525;
    }

    server {
        listen 25;
        proxy_pass smtp_backend;
        proxy_timeout 1h;
        proxy_connect_timeout 30s;
    }
}
```

### stream‑Konfiguration einbinden (falls noch nicht vorhanden)

In `/etc/nginx/nginx.conf`:

`include /etc/nginx/stream.d/*.conf;`

### nginx testen und neu laden

```
nginx -t
systemctl reload nginx
```

## Variante B: Zusätzlichen SMTP‑Port offenlegen (ohne Umleitung)

Beispiel: SMTP‑Dienst direkt auf Port 25 verfügbar machen:

```
stream {
    server {
        listen 25;
        proxy_pass 127.0.0.1:25;
    }
}
```

## Ergebnis

- SMTP‑Traffic läuft **unabhängig von HTTPS/nginx HTTP‑Server‑Blöcken**
- Port 25 kann **umgeleitet** oder **separat exponiert** werden
- Geeignet für Postfix, Exim, Sendmail, Mailcow, etc.