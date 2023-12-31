# Sharepoint Standard Port ändern

<div class="vector-body" id="bkmrk-from-iis-manager%2Crig"><div class="mw-body-content mw-content-ltr" dir="ltr" lang="de"><div class="mw-parser-output">1. from IIS Manager,right click on the site,and change the port from the old to the new one.
2. From Sharepoint Central Administration,click on operation tab,and click on alternate access mapping (AAM) to map the new url to the created web application url. <dl><dd>For example : you changed the created site from port 9999 to 90 :</dd><dd>[http://MyServer:90/](http://myserver:90/) Map To [http://myserver:9999/](http://myserver:9999/)</dd><dd>This tells that if the sharepoint get a request on the port 90 from the iis,map it to the created url web application.</dd></dl>

</div></div></div>Quelle:

```
<a class="external free" href="http://moustafa-arafa.blogspot.com/2008/01/change-port-of-created-sharepoint-site.html" rel="nofollow">http://moustafa-arafa.blogspot.com/2008/01/change-port-of-created-sharepoint-site.html</a>
```