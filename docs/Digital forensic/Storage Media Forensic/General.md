# General
This sections aims to give generals tricks regarding the investigation of any storage media. 
## Cloning a storage media
### DDRescue tool
#### Clone a storage media locally
```bash
sudo apt install gddrescue # Debian and Ubuntu
#ddrescue <options> <source> <destination> <log_file>
ddrescue -d -r3 /dev/sda test.img test.logfile
```
#### Clone a storage media through netcat 
### DD tool
#### Clone a storage media locally
```
dd if=path/to/disk of=path/to/output
```
#### Create a disk image from scratch for test purposes
```
sudo dd if=/dev/random of=/path/to/output/file.img count=512 #Count flag is the number of blocks we want in the output files (There is 512 block of 512 bytes.)
```
### EWF tools
```
sudo apt install ewf-tools #Debian and Ubuntu
sudo pacman -S libewf #Arch and Manjaro
```
#### Clone a storage media locally
```
ewfacquire /dev/sdX
```
#### Get metadata from ewf file
```
ewfinfo /path/to/file
```
#### Mount EWF file
```
ewfmount /path/to/ewf/file /path/to/mount/folder
```
## Mounting a storage media without encryption
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
## Mounting a storage media with encryption
### Bitlocker
### LUKS
#### Passphrase
```
sudo cryptsetup luksOpen /path/to/encrypted/partition your_volume_name #Prompt for a passphrase
sudo mount /dev/mapper/your_volume_name /mnt/path/ #Mount the decrypted volume
sudo cryptsetup luksClose your_volume_name #Close the decrypted volume
```
#### Key file
```
sudo cryptsetup luksOpen --key-file yourkeyfile.key /path/to/encrypted/partition your_volume_name 
sudo mount /dev/mapper/your_volume_name /mnt/path/ #Mount the decrypted volume
sudo cryptsetup luksClose your_volume_name #Close the decrypted volume
```
### Filevault
Filevault is a software created by apple to encrypt disks and partitions. Follow these steps to be able to mount a filevault encrypted partition on Linux : 
#### Install libfvde
For further details regarding the installation and building of libfvde, go check this [link !](https://github.com/libyal/libfvde/wiki/Building)
```
git clone https://github.com/libyal/libfvde/
cd libfvde
./synclibs.sh
sudo apt-get update && sudo apt-get upgrade
sudo apt install git autoconf automake autopoint libtool pkg-config flex bison #For debian and Ubuntu 
./autogen.sh
./configure
make
fdisk -l your_encrypted_volume #For the encrypted volume, retrieve the value in the 'Start' column and the number of bytes in a sector.
sudo ./libfvde/fvdetools/fvdemount -k the_hex_encoded_key -o $((start_offset_of_volume*number_of_bytes_in_a_block_sector)) your_encrypted_volume /tmp/volume #Replace 'start_offset_of_volume' and 'number_of_bytes_in_a_block_sector' with the value you found earlier
sudo mount /tmp/volume /mnt/path/ #Mount the decrypted volume
cd /mnt/path #Access the decrypted volume
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
https://wiki.evolix.org/HowtoLUKS