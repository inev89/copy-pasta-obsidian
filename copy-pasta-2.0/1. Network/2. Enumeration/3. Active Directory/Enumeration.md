# Get Service Prinicipal Names  
GetUserSPNs.py  
  
# Enumerate domain users  
net user /domain  
  
# Enumerate johndoe user  
net user johndoe /domain  
  
# Enumerate domain groups  
net group /domain  
  
# Enumerate password policy  
net accounts  
  
# Enumerate users in sales group  
```
net group "Sales Department" /domain   
```
  
# Add user to domain group  
```
net group "Management Department" stephanie /add /domain  
```
  
# Get Password Policy  
```
net accounts  
```
  
# Enumerate SPNs  
```
setspn -L iis_service  
```
  
# Powershell  
  
## Get Current Domain Info
```
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()  
```

## Retrieve DN  
```
([adsi]'').distinguishedName
```

