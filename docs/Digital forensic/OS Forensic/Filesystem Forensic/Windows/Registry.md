# The windows registry
The aim of this page is to conduct investigation within the windows registry.
## What is the windows registry ?
The Windows Registry is a collection of databases that contains the system's configuration data. This configuration data can be about the hardware, the software, or the user's information. It also includes data about the recently used files, programs used, or devices connected to the system. As you can understand, this data is beneficial from a forensics standpoint. Throughout this room, we will learn ways to read this data to identify the required information about the system. You can view the registry using regedit.exe, a built-in Windows utility to view and edit the registry. We'll explore other tools to learn about the registry in the upcoming tasks.

The Windows registry consists of Keys and Values. When you open the regedit.exe utility to view the registry, the folders you see are Registry Keys. Registry Values are the data stored in these Registry Keys. A [Registry Hive](https://docs.microsoft.com/en-us/windows/win32/sysinfo/registry-hives#:~:text=Registry%20Hives.%20A%20hive%20is%20a%20logical%20group,with%20a%20separate%20file%20for%20the%20user%20profile.) is a group of Keys, subkeys, and values stored in a single file on the disk.

The registry on any Windows system contains the following five root keys:

???+ faq "Five root keys"

    === "HKEY_CURRENT_USER"
        Contains the root of the configuration information for the user who is currently logged on. The user's folders, screen colors, and Control Panel settings are stored here. This information is associated with the user's profile. This key is sometimes abbreviated as HKCU.

    === "HKEY_USERS"
        Contains all the actively loaded user profiles on the computer. HKEY_CURRENT_USER is a subkey of HKEY_USERS. HKEY_USERS is sometimes abbreviated as HKU.

    === "HKEY_LOCAL_MACHINE"
        Contains configuration information particular to the computer (for any user). This key is sometimes abbreviated as HKLM.
    === "HKEY_CLASSES_ROOT"
        This is a subkey of `HKEY_LOCAL_MACHINE\Software`. The information that is stored here makes sure that the correct program opens when you open a file by using Windows Explorer. This key is sometimes abbreviated as HKCR. Starting with Windows 2000, this information is stored under both the HKEY_LOCAL_MACHINE and HKEY_CURRENT_USER keys. The `HKEY_LOCAL_MACHINE\Software\Classes` key contains default settings that can apply to all users on the local computer. The `HKEY_CURRENT_USER\Software\Classes` key has settings that override the default settings and apply only to the interactive user. The HKEY_CLASSES_ROOT key provides a view of the registry that merges the information from these two sources. HKEY_CLASSES_ROOT also provides this merged view for programs that are designed for earlier versions of Windows. To change the settings for the interactive user, changes must be made under `HKEY_CURRENT_USER\Software\Classes` instead of under HKEY_CLASSES_ROOT. To change the default settings, changes must be made under `HKEY_LOCAL_MACHINE\Software\Classes`. If you write keys to a key under HKEY_CLASSES_ROOT, the system stores the information under `HKEY_LOCAL_MACHINE\Software\Classes`. If you write values to a key under HKEY_CLASSES_ROOT, and the key already exists under `HKEY_CURRENT_USER\Software\Classes`, the system will store the information there instead of under `HKEY_LOCAL_MACHINE\Software\Classes`.
    === "HKEY_CURRENT_CONFIG"
        Contains information about the hardware profile that is used by the local computer at system startup.

## Tools for the windows registry
### Acquire the windows registry

|Name|Description|Link|
|-|-|-|
|KAPE|Tool to acquire the windows registry|[KAPE](https://www.kroll.com/en/services/cyber-risk/incident-response-litigation-support/kroll-artifact-parser-extractor-kape)|
|Autopsy|Tool to acquire the windows registry, hard drive and conduct investigations on these elements|[Autopsy](https://www.autopsy.com/)|
|FTK Imager|Tool to acquire, extract files from hard drive|[FTK Imager](https://www.exterro.com/ftk-imager)|



### Explore the windows registry
|Name|Description|Link|
|-|-|-|
|Registry viewer|Tool to acquire the windows registry|[Registry Viewer](https://accessdata.com/product-download/registry-viewer-2-0-0)|
|Zimmerman's registry explorer|Tool to explore the windows registry|[Zimmerman's blog](https://ericzimmerman.github.io/#!index.md)|
|RegRipper|Tool to explore the windows registry|[RegRipper](https://github.com/keydet89/RegRipper3.0)|

## System informations and system accounts
### OS version

`SOFTWARE\Microsoft\Windows NT\CurrentVersion`
### Current control set

The last good known configuration is stored here : `SYSTEM\ControlSet001`
- `SYSTEM\ControlSet002`
- `SYSTEM\Select\Current`
- `SYSTEM\Select\LastKnownGood`

### Computer Name

- `SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName`
### Time zone information

- `SYSTEM\CurrentControlSet\Control\TimeZoneInformation`

### Network interfaces and past networks 

- `SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces`
- `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged`
- `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Managed`

### Autostart programs

`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run`
`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce`
`SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
`SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run`
`SOFTWARE\Microsoft\Windows\CurrentVersion\Run`

### SAM Hive and user information

`SAM\Domains\Account\Users`

## Usage or knowledge of files/folders
### Recent files

`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`
`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.ext`

### Office recent files

`NTUSER.DAT\Software\Microsoft\Office\VERSION`
`NTUSER.DAT\Software\Microsoft\Office\15.0\Word`
`NTUSER.DAT\Software\Microsoft\Office\VERSION\UserMRU\LiveID_####\FileMRU`

### Shellbags
`USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags`
`USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU`
`NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU`
`NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags`

### Open/Save and LastVisited dialog MRUs

`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU`
`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU`

### Windows explorer address/search bars

`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths`
`NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery`

## Executed programs
### UserAssist

`NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count`

### ShimCache

`SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache`
`AppCompatCacheParser.exe --csv <path to save output> -f <path to SYSTEM hive for data parsing> -c <control set to parse>`

### AmCache

`C:\Windows\appcompat\Programs\Amcache.hve`
`Amcache.hve\Root\File\{Volume GUID}\`

### BAM/DAM

`SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}`
`SYSTEM\CurrentControlSet\Services\dam\UserSettings\{SID}`

## External devices 
### Device identification

`SYSTEM\CurrentControlSet\Enum\USBSTOR`
`SYSTEM\CurrentControlSet\Enum\USB`

### First/Last Times

`SYSTEM\CurrentControlSet\Enum\USBSTOR\Ven_Prod_Version\USBSerial#\Properties\{83da6326-97a6-4088-9453-a19231573b29}\####`

### USB devices volume name
`SOFTWARE\Microsoft\Windows Portable Devices\Devices`

## Ressources
https://tryhackme.com/room/windowsforensics1