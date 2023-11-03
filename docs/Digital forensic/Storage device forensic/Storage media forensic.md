# Storage media forensic
This sections aims to give generals tricks regarding the investigation of any storage media. 
## Acquire a storage media image from device
### Cloning a storage media
??? info "DDRescue tool"

	=== "Install DDRescue tool"
		For Debian and Ubuntu 
		```
		sudo apt install gddrescue # Debian and Ubuntu
		```
	=== "Clone a storage media locally"
		```
		ddrescue -d -r3 /dev/sda test.img test.logfile
		```
	=== "Clone a storage media through network"
		```
		ddrescue -d -r3 /dev/sda test.img test.logfile
		```

??? info "DD tool"

	=== "Clone a storage media locally"
		```
		dd if=path/to/disk of=path/to/output
		```
	=== "Clone a storage media through network"
		Launch this command on the analyst machine : 
		```
		(analyst)$ nc -lvp 3615 | bzip2 -d | dd bs=16M of=linux_disk.dmp
		```
		And launch this command on the infected machine
		```
		sudo dd bs=16M if /dev/sda status=progress | bzip2 -c | nc -v @IP 3615
		```
		Wait for the transfer to end and make sure to check the file integrity after the transfer 
		```
		ls -lah | grep linux_disk
		
		```

??? info "EWF tools"

	=== "Install EWF tools"
		For Debian and Ubuntu
		```
		dd if=path/to/disk of=path/to/output
		```
		For Arch, EndeavourOS and Manjaro
		```
		sudo pacman -S libewf #Arch and Manjaro
		```
	=== "Clone a storage media locally"
		```
		ewfacquire /dev/sdX
		```
	=== "Get metadata from EWF file"
		```
		ewfinfo /path/to/file
		```

## Mount a storage media image / device
??? info "Mount a specific format of disk image"

	=== ".EWF Format"
	First, you need to open the container by typing this command : 
		```
		sudo ewfmount /path/to/ewf/file /mnt/where/you/want
		```
	This will open the evidence file and the disk itself will be accessible at the path `/mnt/where/you/want/ewf1`
	Once you're finished, you can type this command to unmount the evidence file : 
		```
		sudo umount /mnt/where/you/want
		```

??? info "Mount a storage media without encryption"

	=== "Storage media with single partition"
		```
		mount -o ro,loop,norecovery /path/to/storage/media /mnt/where/you/want
		```
	 The flag `ro` allow us to mount the disk in read-only mode, the flag `loop` allow us to mount the disk as a loop device and the flag `norecovery` allow us to mount the disk, even though the filesystem is in a dirty state (Which means that the filesystem was not recovered)
	=== "Storage media with multiple partition"
		First step : Retrieve the first sector number of the needed partition (a) and the sector size (b)
		```
		fdisk -l /path/to/file
		```
		Second step : Mount the needed partition by specifying the values found during the previous step 
		```
		mount -o ro,loop,norecoveryoffset=$((a*b)) /path/to/file.img /mnt
		```
	The flag `ro` allow us to mount the disk in read-only mode, the flag `loop` allow us to mount the disk as a loop device and the flag `norecovery` allow us to mount the disk, even though the filesystem is in a dirty state (Which means that the filesystem was not recovered). Finally, the flag offset allow us to mount the partition of our choice by specifying its start offset.
	=== "Unmount a storage media"
		First way : Unmount the folder on which the storage media is mounted
		```
		umount /mnt/folder
		```
		Second way : Unmount the disk itself
		```
		umount /dev/sdX
		```

??? info "Mount a storage media with encryption"

	=== "Bitlocker (Windows)"
		=== "Passphrase"
			```
			TO DO
			```
		=== "Key file"
			```
			TO DO
			```
	=== "LUKS (Linux)"
		=== "Passphrase"
			First step : Decrypt the partition. the command will ask you a password.
				```
				sudo cryptsetup luksOpen /path/to/encrypted/partition your_volume_name
				```
			Second step : Mount the decrypted volume on your filesystem
				```
				sudo mount /dev/mapper/your_volume_name /mnt/path/
				```
			Third step : Don't forget to close the volume after ! 
				```
				sudo cryptsetup luksClose your_volume_name
				```
		=== "Key file"
			First step : Decrypt the partition with the key file path. 
				```
				sudo cryptsetup luksOpen --key-file yourkeyfile.key /path/to/encrypted/partition your_volume_name
				```
			Second step : Mount the decrypted volume on your filesystem
				```
				sudo mount /dev/mapper/your_volume_name /mnt/path/
				```
			Third step : Don't forget to close the volume after ! 
				```
				sudo cryptsetup luksClose your_volume_name
				```
	=== "Filevault (Mac OS X)"
		=== "Tool installation"
			Clone the libfvde github repository. For further details regarding the installation and building of libfvde, go check this [link !](https://github.com/libyal/libfvde/wiki/Building)
				```
				git clone https://github.com/libyal/libfvde/ && cd libfvde && ./synclibs.sh
				```
			Install the required packages
				```
				sudo apt-get update && sudo apt-get upgrade && udo apt install git autoconf automake autopoint libtool pkg-config flex bison
				```
			In the root folder of the github repository, issue the following commands : 
				```
				./autogen.sh
				./configure
				make
				```
			If eveything went all, you will be able to mount filevault encrypted partition.
		=== "Encryption key in hex format"
			First step : Retrieve the first sector number of the needed partition (a) and the sector size (b)
				```
				fdisk -l /path/to/file
				```
			Second step : Decrypt the volume by specifying the offset computed during the previous step 
				```
				sudo ./libfvde/fvdetools/fvdemount -k the_hex_encoded_encryption_key -o $((a*b)) your_encrypted_volume /tmp/volume
				```
			Third step : Mount the decrypted volume on your filesystem
				```
				sudo mount /tmp/volume /mnt/path/
				```
## Analyze a storage media image / device
??? info "Analyze the storage media to gather informations"

	=== "Get the file system"
		```
		parted -m /path/to/file print | tail -n +3 | awk -F ":" '{print $(NF-2)}'
		```

??? info "Analyze the storage media to gather files"

	=== "Getting the current files"
		```
		testdisk path/to/disk/image
		```
		Press Enter -> None -> Advanced -> List
	=== "Retrieve the deleted files"
			=== "extundelete"
				```
				extundelete path/to/disk/image
				```
			=== "binwalk"
				```
				binwalk
				```
			=== "foremost"
				```
				foremost
				```
			=== "bulkextractor"
				```
				bulkextractor
				```
			=== "photorec"
				```
				photorec
				```
			=== "scalpel"
				```
				scalpel
				```


## Fixing a storage media image / device 

??? info "Fix the filesystem"

	=== "NTFS"
		```
		ntfsfix /dev/sdX
		```
	=== "Others"
		```
		sudo fsck -p /path/to/file
		```

## Resources
https://wiki.evolix.org/HowtoLUKS