---
title: TrueNAS Scale
description: Informationen zu TrueNAS Scale, einem auf Debian basierenden alles in einem Lösung mit Apps, Containern, VM's
published: true
date: 2025-10-13T21:28:22.531Z
tags: speicher, storage, iscsi, docker
editor: markdown
dateCreated: 2025-10-13T21:28:19.281Z
---

# 📁 SFTPGo mit SMB-Share als Speicherziel einrichten

## 📝 Ziel

Diese Anleitung beschreibt, wie man in **SFTPGo** einen **SMB-Netzwerkpfad** als Speicherziel einbindet. Dadurch können Dateien, die per SFTP hochgeladen werden, direkt auf einem entfernten Windows- oder Samba-Server gespeichert werden – **ohne lokale Kopie** auf dem Hostsystem (z. B. TrueNAS SCALE).

---

## 🔧 Voraussetzungen

- Eine laufende Instanz von **SFTPGo** (z. B. als App in TrueNAS SCALE oder als Container).
- Ein erreichbarer **SMB-Server** (z. B. Windows-Server oder Samba).
- Ein freigegebener **SMB-Share** mit gültigen Anmeldedaten.
- **Netzwerkverbindung** zwischen SFTPGo und dem SMB-Server.

---

## ⚙️ Konfiguration in SFTPGo

### 1. Benutzer anlegen oder bearbeiten

- Öffne die SFTPGo-Weboberfläche.
- Navigiere zu **Users**.
- Erstelle einen neuen Benutzer oder bearbeite einen bestehenden.

### 2. Dateisystemtyp auf „SMB Share“ setzen

Im Abschnitt **File System** folgende Felder ausfüllen:

<table>
  <thead>
    <tr>
      <th>Feld</th>
      <th>Wert</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Storage</strong></td>
      <td>SMB Share</td>
    </tr>
    <tr>
      <td><strong>Mount Path</strong></td>
      <td>z. B. <code>/mnt/smb/scans</code></td>
    </tr>
    <tr>
      <td><strong>Server</strong></td>
      <td>IP oder Hostname des SMB-Servers (z. B. <code>192.168.1.100</code>)</td>
    </tr>
    <tr>
      <td><strong>Share</strong></td>
      <td>Pfad zum SMB-Share (z. B. <code>Freigaben/Scans</code>)</td>
    </tr>
    <tr>
      <td><strong>Username</strong></td>
      <td>Benutzername mit Zugriff auf den Share</td>
    </tr>
    <tr>
      <td><strong>Password</strong></td>
      <td>Passwort des SMB-Benutzers</td>
    </tr>
    <tr>
      <td><strong>Domain (optional)</strong></td>
      <td>z. B. <code>WORKGROUP</code> oder Domänenname</td>
    </tr>
  </tbody>
</table>

💡 *Der Mount Path ist der lokale Pfad, unter dem der SMB-Share im Container oder Hostsystem eingebunden wird. Er muss eindeutig sein.*

### 3. (Optional) Virtuelle Ordner definieren

Im Abschnitt **Virtual Folders** kannst du zusätzliche Pfade innerhalb des SMB-Shares definieren.

**Beispiel:**

- **Mount Path**: `/`
- **Folder**: `/mnt/smb/scans`
- **Quota**: *(optional)*

---

## ✅ Ergebnis

Nach dem Speichern der Konfiguration:

- Alle Dateien, die per SFTP an diesen Benutzer gesendet werden, landen **direkt im SMB-Share**.
- Der SMB-Share wird beim Start von SFTPGo **automatisch eingebunden**.
- Es ist **keine lokale Speicherung** auf dem Hostsystem notwendig.

## 🛠️ Fehlerbehebung

<table>
  <thead>
    <tr>
      <th>Problem</th>
      <th>Mögliche Ursache</th>
      <th>Lösung</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Upload schlägt fehl</td>
      <td>Falsche SMB-Zugangsdaten</td>
      <td>Zugangsdaten prüfen</td>
    </tr>
    <tr>
      <td>Keine Verbindung zum Share</td>
      <td>Netzwerkproblem oder falscher Servername</td>
      <td>IP-Adresse testen, Firewall prüfen</td>
    </tr>
    <tr>
      <td>Dateien erscheinen nicht</td>
      <td>Falscher Mount Path oder Share</td>
      <td>Pfade und Rechte prüfen</td>
    </tr>
    <tr>
      <td>Keine Schreibrechte</td>
      <td>Benutzer hat keine Schreibrechte im Share</td>
      <td>Berechtigungen auf dem SMB-Server anpassen</td>
    </tr>
  </tbody>
</table>

---

## 🔐 Sicherheitshinweise

- Verwende nach Möglichkeit **dedizierte Benutzerkonten** mit minimalen Rechten.
- Stelle sicher, dass die Verbindung zum SMB-Server **verschlüsselt** ist (z. B. über VPN oder internes Netzwerk).
- Vermeide es, sensible Daten **unverschlüsselt** über das Netzwerk zu übertragen.

---

## 📚 Weiterführende Links

