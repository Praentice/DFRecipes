The aim of this page is to give you the list of interesting ID to query through the Windows GUI tool Event Viewer per log life. 
## Automated analysis
If you want to automate the analysis of windows event logs, you should export the log from the compromised machine : 

??? faq "Windows event logs path location"
	You can find the windows event logs file in the following path : 
	```
	C:\Windows\System32\winevt\Logs
	```

Then, you can use the following tools to assist you : 

??? faq "Tools to analyze evtx logs"

	=== "DeepBlueCLI"
	    This tool allows you to easily detect suspicious events within the windows event log. Click [here](https://github.com/sans-blue-team/DeepBlueCLI) to access the repo


## Manual analysis
### Windows Defender
The aim of this file is log any action on WIndows Defender. 

1116 : Threat detected
1117 : Threat quarantined
5001 : Real time scan desactivated

### Security
This file contains events related to the host security in general. 

4624 : Successful login (User Logon)
4647 : User logoff
4688 : Process Creation
4697 : A service was installed on the machine

### Powershell
This file contains events related to Powershell in general. 

800 : Pipeline execution details

### System
This file contains events logged by applications. 

7045 : Service was installed
6006 : Log record was disabled
1102 : Log record was erased

### TerminalServices-LocalSessionManager

21 : Remote desktop session for user opened
23 : Remote desktop session for user closed

### Sysmon
You can find these logs in the following path on Event Viewer : 
"Applications and Services Logs" -> "Microsoft" -> "Windows" -> "Sysmon" -> "Operational"

8 : Create Remote Thread
11 : File creation
17 : Pipeline creation

### Resources
https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/
https://github.com/sans-blue-team/DeepBlueCLI