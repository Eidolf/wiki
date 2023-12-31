# Test des WMI Repository

# <span class="mw-headline" id="bkmrk-powershell-skript%3A-1">Powershell Skript:</span>

```
function test-wmirepository {            
 param(            
  [string]$path            
 )            
             
 if ($path) {            
   if (-not(Test-Path $path)){            
    Throw "$path not found"            
   }            
   else {            
    $path            
    $exp = "winmgmt /verifyrepository $path"            
   }            
 }            
 else {            
  $exp = "winmgmt /verifyrepository"            
 }            
 Invoke-Expression -Command $exp            
            
}
```

## <span class="mw-headline" id="bkmrk-quelle%3A-1">Quelle:</span>

```
<a class="external free" href="http://itknowledgeexchange.techtarget.com/powershell/testing-the-wmi-repository/" rel="nofollow">http://itknowledgeexchange.techtarget.com/powershell/testing-the-wmi-repository/</a>
```