# Hard drive forensic
This section aims to give specifics tricks regarding the analysis of hard drive.

??? info "Checking hard drive health"

	=== "smartctl command"
	Install package smartmontools before issuing the command 
		```
		smartctl -i /dev/sdX  
		```
??? warning "Get informations on the hard drive"

	=== "hdparm command"
	This command can compromise the evidence integrity ! Please proceed with caution.
		```
		sudo hdparm -I /dev/sdX
		```

??? info "Convert hard drive image between format"

	=== " VMDK -> RAW"
		```
		qemu-img convert path/to/file.vmdk path/to/new/file.raw 
		```
	=== "RAM -> QCOW2"
	
		```
		qemu-img convert path/to/file.raw path/to/new/file.qcow2
		```
	=== "VMDK -> QCOW2"
	 
		```
		qemu-img convert -f vmdk path/to/file.vmdk path/to/new/file.qcow2 
		```
	=== "OVA -> QCOW2"
	First step : Get the vmdk file
		```
		tar xf path/to/file.ova -C .
		```
	Second step : Convert the VMDK file to QCOW2 file
		```
		qemu-img convert -f vmdk path/to/file.vmdk path/to/new/file.qcow2
		```
	Third step (optionnal) : Reduce the size of the generated file
		```
		qemu-img convert -O qcow2 path/to/new/file.qcow2 path/to/new/file-shrunk.qcow2
		```

## Resources