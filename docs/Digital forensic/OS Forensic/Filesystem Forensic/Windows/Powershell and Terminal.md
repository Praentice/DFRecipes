# Powershell and Terminal
The aim of this page is to give tips and tricks regarding windows investigation by using the powershell or terminal console of the compromised machine. 
## User accounts
### Get last login information for all local accounts
```Powershell
Get-LocalUser | Select Name, Lastlogon
```
or
```Powershell
$([ADSI]"WinNT://$env:COMPUTERNAME").Children | where {$_.SchemaClassName -eq 'user'} | Select Name, Lastlogin
```
or
```Powershell
net user USERNAME
```
### Processes and scheduled tasks
#### Get list of scheduled tasks
```Powershell
Get-ScheduledTask
```
or
```MS-DOS
schtasks /query
schtasks /query /v /fo LIST
schtasks /query /v /fo LIST /TN "name of the task"
```
#### Get list of C2 servers
```Powershell
Get-Content C:\Windows\System32\drivers\etc\hosts #Useful to check for DNS Poisoning
```
