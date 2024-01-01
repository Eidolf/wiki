---
title: raspberry-pi
description: 
published: true
date: 2024-01-01T15:49:27.725Z
tags: 
editor: markdown
dateCreated: 2023-12-31T13:27:35.311Z
---

# Raspberry Pi

# <span class="mw-headline" id="bkmrk-fehler-1">Fehler</span>

## <span id="bkmrk-"></span><span class="mw-headline" id="bkmrk-fqdn-aufl%C3%B6sen-1">FQDN auflösen</span>

Dieses Problem tritt wie ich gelesen habe auch bei anderen Ubuntu Distributionen auf.  
Es kann keine **xxx.local** über FQDN erreicht werde.

### <span id="bkmrk--1"></span><span class="mw-headline" id="bkmrk-l%C3%B6sung-1">Lösung</span>

In Datei **/etc/nsswitch.conf** folgenden Eintrag von  
`hosts: files mdns4_minimal [NOTFOUND=return]`  
zu  
`hosts: files mdns4_minimal dns mdns4`  
ändern.

### <span class="mw-headline" id="bkmrk-quelle-1">Quelle</span>
https://andrewgdotcom.wordpress.com/2007/09/28/avahi-and-dot-local-addresses-on-ubuntu-gutsy/