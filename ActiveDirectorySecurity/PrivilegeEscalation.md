# Windows Privilege Escalation

__Privilege Escalation Notes for Active Directory__

## Unquoted Service Paths

It is possible to escalate privileges if there's a running service, of which the service binary which is running is not quoted. Not wrapping binary paths can allow us to place a malicious binary in place of the path which later can get executed at service restart.

__Scenario__

There's a program running in windows system:

C:\Applications\Web Server\webserver.exe

In this case, we can place a malicious binary at C:\Applications\Web.exe which will get executed as the previous path isn't enclosed in quotes, windows will execute the binary before the whitespace " ".


__Looking for unquoted service paths__

```powershell
wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows\\" |findstr /i /v """
```
PowerSploit module:

```powershell
Get-ServiceUnquoted
```
