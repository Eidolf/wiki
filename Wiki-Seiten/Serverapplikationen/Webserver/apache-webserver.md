# Apache Webserver

# <span class="mw-headline" id="bkmrk-sicherheit-1">Sicherheit</span>

## <span class="mw-headline" id="bkmrk-pfs-1">PFS</span>

SSLProtocol all -SSLv2 -SSLv3  
SSLHonorCipherOrder On  
SSLCipherSuite EECDH+AES:EDH+AES:-SHA1:EECDH+RC4:EDH+RC4:RC4-SHA:EECDH+AES256:EDH+AES256:AES256-SHA:!aNULL:!eNULL:!EXP:!LOW:!MD5

### <span class="mw-headline" id="bkmrk-quellen-1">Quellen</span>

```
<a class="external free" href="http://stackoverflow.com/questions/17308690/how-do-i-enable-perfect-forward-secrecy-by-default-on-apache" rel="nofollow">http://stackoverflow.com/questions/17308690/how-do-i-enable-perfect-forward-secrecy-by-default-on-apache</a>
```