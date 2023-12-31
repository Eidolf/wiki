# Druck Benachrichtigung in Taskleiste

Um die aufploppende Druckbenachrichtigung in der Taskleiste abzuschalten muss man folgenden Regeintrag setzen.

```
HKEY_CURRENT_USER\Printers\Settings "EnableBalloonNotificationsLocal" DWORD:"0"
```