# Flash memory forensic
This page aims to give specifics tricks regarding the analysis of flash storage media (USB keys, any memory cards..)
## Running performance test
??? warning "Testing I/O performance"

	These commands will compromise the evidence integrity ! Please proceed with caution. 
	=== "f3write and f3read commands"
	Write large files to the card
		```
		./f3write /media/username/AAAA-HHHH/  
		```
	Check if the card contains the written files
		```
		./f3read /media/username/AAAA-HHHH/ 
		```

??? warning "Testing capacity size"

	This command will compromise the evidence integrity ! Please proceed with caution. 
	=== "f3probe command"
		```
		sudo f3probe --destructive --time-ops /dev/mmcblkX 
		```
## Repair flash memory card

??? warning "Fixing the flash memory card"

	This command will compromise the evidence integrity ! Please proceed with caution. 
	=== "f3fix command"
		```
		./f3fix --last-sec=any_value /dev/mmcblkX
		```
## Resources
https://fight-flash-fraud.readthedocs.io/en/latest/introduction.html#testing-performance-with-f3read-f3write