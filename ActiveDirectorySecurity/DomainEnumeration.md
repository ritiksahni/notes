# Domain Enumeration

- To get current domain information

```powershell
Get-NetDomain
Get-NetDomain -Domain "DOMAINNAME.local"
```

- To get domain SID

```powershell
Get-DomainSID
```

- To get domain policy of current domain

```powershell
(Get-DomainPolicy)."system access"
```

- To get information about the domain controller.

```powershell
Get-NetDomainController
```

- To see the last password set time of all users in current domain.

```powershell
Get-UserProperty –Properties pwdlastset
```

- To get user descriptions of all users.

```powershell
Find-UserField -SearchField Description
```

- To get the list of all computers in the current domain

```powershell
Get-NetComputers
```

- To get all the live hosts in the current domain

```powershell
Get-NetComputer -Ping
```

- To get the members in "Domain Admins" group

```powershell
Get-NetGroupMember -GroupName "Domain Admins"
```

- To get local administrators of a machine

```powershell
Get-NetLocalGroup
Get-NetLocalGroup –ComputerName COMPUTER-NAME
```

- To get actively logged on users on a machine

```powershell
Get-NetLoggedon –ComputerName COMPUTER-NAME
```

- To get all the network shares in the current domain

```powershell
Invoke-ShareFinder
```

 – To display running processes with verbose mode

```powershell
tasklist /V
```

# Enumeration using Active Directory module

> Active Directory powershell module is signed by Microsoft itself so the detection rate by AVs is very less. By default, it needs admin privileges and  RSAT tools installed in order to work in system.

Steps to use AD-Modules:

1. Download and place http://github.com/winsaafman/Scriptdotsh-ActiveDirectory/raw/master/Microsoft.ActiveDirectory.Management.dll in the system.
2. Import the DLL file using PowerShell using the following command

```powershell
Import-Module .\Microsoft.ActiveDirectory.Management.dll
```

> AD-Modules can also work in PowerShell Constrained Language mode (https://devblogs.microsoft.com/powershell/powershell-constrained-language-mode/)

-----------------

- To get information about the current domain

```powershell
Get-ADDomain
```

- To get information about groups in current domain

```powershell
Get-ADGroup -Filter *
```

- To get information about domain forests

```powershell
Get-ADForest
Get-ADForest -Filter *
```


# Enumeration using PowerView

> It can get loaded into memory and has many features 

Steps to use PowerView:

1. Download https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1 in the system.
2. Import the module using Powershell using the following command:

```powershell
Import-Module .\PowerView.ps1
```

-----------------

- To get information about domain controller

```powershell
Get-NetDomainController
````

- To get group information of another domain (only if there is a domain trust relationship in the current domain)

```powershell
Get-NetGroup -DomainController DC-HOSTNAME
```

- To get current session details of a specified host

```powershell
Get-NetSession -ComputerName HOSTNAME
```

- To get access control list information

```powershell
Get-ObjectAcl -SamAccountName NAME
```

- To find if the current user is local administrator in any other host of the current domain

```powershell
Find-LocalAdminAccess -Verbose
```

- To find members of local administrators group across all machines in the domain

```powershell
Invoke-EnumerateLocalAdmin -Verbose
```

- To find computers where domain administrators are logged in

```powershell
Invoke-UserHunter
Invoke-UserHunter -Stealth
```

#### GPO is "Group Policy Objects" and are used for efficient management of systems across domains for tasks like logoff, shutdown, software installation. From an attackers' point of view, GPOs can be elevated to perform tasks like PrivEsc, backdooring etc.

- Get list of GPOs in current domain

```powershell
Get-NetGPO
Get-NetGPO -ComputerName DCORP-DC1.CORPORATE.local
```

- Get list of GPOs which uses "Restricted Groups" or groups.xml for interesting users

```powershell
Get-NetGPOGroup
```

- Get machines where specified users are member of a particular group

```powershell
Find-GPOLocation -UserName USERNAME -Verbose
```
