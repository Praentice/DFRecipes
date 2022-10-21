# Flash memory
This page aims to give specifics tricks regarding the analysis of flash storage media (USB keys, any memory cards..)
## Running performance test
### Testing I/O performance 
```
./f3write /media/michel/5EBD-5C80/ #Write large files to the card
./f3read /media/michel/5EBD-5C80/ #Check if the card contains the written files
```
### Testing capacity size
```
sudo f3probe --destructive --time-ops /dev/mmcblkX 
# /!\ Destroy any data stored on the card /!\
```
### Fixing the flash memory
```
./f3fix --last-sec=any_value /dev/mmcblkX
```
## Sources
https://fight-flash-fraud.readthedocs.io/en/latest/introduction.html#testing-performance-with-f3read-f3write