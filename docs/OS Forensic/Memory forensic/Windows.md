
# Windows memory analysis
This page aims to give tricks regarding the analysis of Windows memory by using Volatility
## Identify the OS system
```
volatility -f file.dmp imageinfo 
volatility -f file.dmp kdbgscan 
```
## General informations on the memory dump
### Get hostname of computer
#### First way
```
volatility -f file.dmp printkey -K "Software\Microsoft\Windows Media\WMSDK\General"
```
#### Second way
```
volatility -f file.dmp printkey -K "ControlSet001\Control\ComputerName\ActiveComputerName"
```
#### Third way
```
volatility -f file.dmp hivelist # Retrieve virtual address of \REGISTRY\MACHINE\SYSTEM
volatility -f file.dmp `printkey -o virtual_address -K "ControlSet001\services\Tcpip\Parameters"`

```
### Get the environment variables
```
volatility --profile=PROFILE -f file.dmp envars
```
### Get the file cached in the memory dump
```
photorec path/to/mem/dump #Get All Files with photorec
volatility -f file.dmp filesystem
```
## Informations on processes 
```
volatility --profile=PROFILE -f path/to/mem/dump memdump -p your_pid --dump-dir .
strings your_pid.dmp > your_pid.txt #Retrieve only strings char from process dump
```
### Get command list
```
volatility --profile=PROFILE -f path/to/mem/dump cmdlist
```