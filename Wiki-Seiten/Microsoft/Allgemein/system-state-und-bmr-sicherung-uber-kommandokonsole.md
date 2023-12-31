# System State und BMR Sicherung über Kommandokonsole

Zum überprüfen ob eine System State Sicherung oder eine Bare Metal Recovery Sicherung funktionieren muss man folgendes in die Kommandokonsole eingeben.

System State Backup:

```
wbadmin start systemstatebackup -backupTarget:<VolumeName>
```

Bare Metal Recovery:

```
wbadmin start backup -allcritical -backuptarget:\\<Servername>\<Freigabenname>
```