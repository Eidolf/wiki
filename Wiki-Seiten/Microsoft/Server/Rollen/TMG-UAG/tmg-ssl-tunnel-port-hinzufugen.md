# TMG SSL Tunnel Port hinzufügen

<div class="vector-body" id="bkmrk-go-to-www.isatools.o"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. Go to www.isatools.org and download the isa\_tpr.js file ([http://www.isatools.org/tools/isa\_tpr.js](http://www.isatools.org/tools/isa_tpr.js)) and copy that file to your ISA firewall. Do not use the browser on the firewall. Download the file to a management workstation, scan the file, and then copy the file to removable media and then take it to the ISA firewall. Remember, never use client applications, such as browsers, e-mail clients, etc. on the firewall itself.
2. Double click the isa\_tpr.js file. The first dialog box you see states This is your current Tunnel Port Range list. Click OK.
3. The NNTP port is displayed. Click OK.
4. The SSL port is displayed. Click OK.
5. Now copy the isa\_tpr.js file to the root of the C: drive. Open a command prompt(als Administrator and enter the following: **cscript isa\_tpr.js /?**
6. Beispiel für Port 8443 **cscript isa\_tpr.js /add Name8443 8443**
7. Neustart des TMG Firewalldienstes

</div></div></div># <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://www.isaserver.org/articles/2004tunnelportrange.html" rel="nofollow">http://www.isaserver.org/articles/2004tunnelportrange.html</a>
<a class="external free" href="http://social.technet.microsoft.com/Forums/en-US/Forefrontedgegeneral/thread/66f2a836-5185-4975-8c9d-5a0c78e10030/" rel="nofollow">http://social.technet.microsoft.com/Forums/en-US/Forefrontedgegeneral/thread/66f2a836-5185-4975-8c9d-5a0c78e10030/</a>
```