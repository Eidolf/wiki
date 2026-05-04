---
title: nginx
description: Konfiguration für nginx Reverse Proxy
published: true
date: 2026-05-04T15:37:13.516Z
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