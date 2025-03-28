# Log  
```
dir 'C:\windows\System32\winevt\Logs\Microsoft-Windows-Windows Defender%4Operational.evtx'  
```
  
# check if in log  
```
Get-WinEvent -LogName "Microsoft-Windows-Windows Defender/Operational" -Oldest | Where-Object { $_.LevelDisplayName -ne "Information" }   
```
  
# more info for analysis  
```
Get-WinEvent -LogName "Microsoft-Windows-Windows Defender/Operational" -Oldest | Where-Object { $_.LevelDisplayName -ne "Information" } | Select-Object -ExpandProperty Message  
```
  
# Disable Defender  
```
Set-MpPreference -DisableRealtimeMonitoring $true  
```
  
# Enable Defender  
```
Set-MpPreference -DisableRealtimeMonitoring $false  
```
  
# Add defender exception  
```
Set-MpPreference -ExclusionPath 'C:\Windows\Temp'  
```
  
# Remove Defender exception  
```
Remove-MpPreference -ExclusionPath 'C:\Windows\Temp'  
```
  
# Get event log  
```
Get-EventLog -LogName System -Source 'Service Control Manager' -Newest 5  
```
  
# Registry Keys to Disable Defender that beats tamper protection.. requires restart  
```
reg add "HKLM\Software\Policies\Microsoft\Windows Defender" /v DisableAntiSpyware /t reg_dword /d 1 /f  
```
  
```
reg query "HKLM\Software\Policies\Microsoft\Windows Defender/DisableAntiSpyware"  
```
  
```
reg delete "HKLM\Software\Policies\Microsoft\Windows Defender" /v DisableAntiSpyware /f
```