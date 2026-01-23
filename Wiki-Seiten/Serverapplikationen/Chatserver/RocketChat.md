---
title: RocketChat
description: Chatserver
published: true
date: 2026-01-23T11:28:09.563Z
tags: docker, docker compose
editor: markdown
dateCreated: 2026-01-23T11:27:39.887Z
---


# Übersicht - MongoDB Update

Beim Update von MongoDB in Docker – beispielsweise von Version **6.0** auf **7.0** – sind folgende Schritte erforderlich:

1. Docker‑Konfiguration anpassen (Image‑Version erhöhen)  
2. Container neu starten  
3. In den Container eintreten  
4. `mongosh` starten  
5. Aktuelle FCV auslesen  
6. FCV setzen  
7. Erneut prüfen

# Schritte im Detail

## 1. Docker-Konfiguration anpassen

Erhöhe die verwendete MongoDB‑Version in deiner `docker-compose.yml` oder deinem Docker‑Setup, z. B.:

```yaml
image: mongo:7.0
```

## 2. Container neu starten

```bash
docker-compose down
docker-compose up -d
```

## 3. In den laufenden Container verbinden

```bash
docker exec -it <containername> bash
```

## 4. `mongosh` starten

```bash
mongosh
```

## 5. Aktuelle Feature Compatibility Version auslesen

```javascript
db.adminCommand({ getParameter: 1, featureCompatibilityVersion: 1 })
```

## 6. Feature Compatibility Version setzen

> Ersetze `7.0` mit der Ziel‑Version, die deinem MongoDB‑Update entspricht.

```javascript
db.adminCommand({ setFeatureCompatibilityVersion: "7.0" })
```

## 7. Erneut prüfen

```javascript
db.adminCommand({ getParameter: 1, featureCompatibilityVersion: 1 })
```

# Referenz

Offizielle Dokumentation zu `setFeatureCompatibilityVersion`:  
https://www.mongodb.com/docs/manual/reference/command/setFeatureCompatibilityVersion/
