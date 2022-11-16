# General
This sections aims to give generals tricks regarding the investigation of any storage media. 
## Cloning a storage media
##### DDRescue tool
###### Clone a storage media locally
```bash
sudo apt install gddrescue # Debian and Ubuntu
#ddrescue <options> <source> <destination> <log_file>
ddrescue -d -r3 /dev/sda test.img test.logfile
```
###### Clone a storage media through netcat 
##### DD tool
###### Clone a storage media locally
```
dd if=path/to/disk of=path/to/output
```
###### Create a disk image from scratch for test purposes
```
sudo dd if=/dev/random of=/path/to/output/file.img count=512 #Count flag is the number of blocks we want in the output files (There is 512 block of 512 bytes.)
```
##### EWF tools
```
sudo apt install ewf-tools #Debian and Ubuntu
sudo pacman -S libewf #Arch and Manjaro
```
###### Clone a storage media locally
```
ewfacquire /dev/sdX
```
###### Get metadata from ewf file
```
ewfinfo /path/to/file
```
###### Mount EWF file
```
ewfmount /path/to/ewf/file /path/to/mount/folder
```
## Mounting a storage media
```bash
mount /path/to/storage/media /mnt/where/you/want
```
### Mount a hard drive image with multiple partitions
```bash
fdisk -l /path/to/file # Retrieve the first sector number of the desired partition (a) and the sector size (b)
to compute offset such as a * b = offset 
mount -o offset=65536 /path/to/file.img /mnt #-o flag allows you to mount the partition of your choice
```
### Unmount a storage media
```
umount /mnt/folder #Folder on which the hard drive is mounted
umount /dev/sdX #Unmount the disk directly
```
## Analyzing the storage media for informations 
### Get the file system
```
parted -m /path/to/file print | tail -n +3 | awk -F ":" '{print $(NF-2)}'
```
## Analyzing the storage media for files
This sections aims to give tricks regarding the analyze of storage media to find files. Don't hesitate to consult this page to get more informations on the different files format you might encouter during your investigation.
### Getting the current files
##### Testdisk
```
testdisk path/to/disk/image
```
### Searching deleted files
##### Photorec
```
photorec path/to/disk/image
```
## Fixing the storage media 
### Filesystem
##### Anything
```
sudo fsck -p /path/to/file
```
##### NTFS Only
```
ntfsfix /dev/sdX
```
## Sources
https://fight-flash-fraud.readthedocs.io/en/latest/introduction.html#testing-performance-with-f3read-f3write