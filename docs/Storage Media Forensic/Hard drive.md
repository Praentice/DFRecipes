# Hard drive
This section aims to give specifics tricks regarding the analysis of hard drive.
## Checking hard drive health
```
smartctl -i /dev/sdX #Install package smartmontools before issuing the command 
```
## Get informations on the hard drive
```
sudo hdparm -I /dev/sdX #Get info on the disk itself
```
## Convert hard drive image between format
###### VMDK --> RAW
```
qemu-img convert path/to/file.vmdk path/to/new/file.raw
```
###### RAW --> QCOW2
```
qemu-img convert path/to/file.raw path/to/new/file.qcow2
```
###### VMDK --> QCOW2
```
qemu-img convert -f vmdk path/to/file.vmdk path/to/new/file.qcow2
```
###### OVA --> QCOW2
```
tar xf path/to/file.ova -C . #Get the VMDK file
qemu-img convert -f vmdk path/to/file.vmdk path/to/new/file.qcow2 #Convert the VMDK file to QCOW2 file
qemu-img convert -O qcow2 path/to/new/file.qcow2 path/to/new/file-shrunk.qcow2 # Reduce the size of the generated file
```
## Sources
