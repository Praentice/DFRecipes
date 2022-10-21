# Workstation setup
This page aims to give instructions to help you setup a forensic workstation.
Regarding the configuration of your forensic workstation, you have two choices. The first one is using an existant distro tuned from Vanilla Debian, Ubuntu OS or create your own distro from scratch. Please consult the right section based on your needs. 
## Using an existant distro
Using an existant distro can be very useful if you want to setup a forensic workstation as a VM. You can find on the following table, the existing distro for the forensic world.

| Name    | Description | OS  | Link |
|---------|-------------|-----|------|
| Tsuguri |Forensic distro which includes popular forensic and OSINT tools. Best run as an installed OS     | Based on Ubuntu    |https://tsurugi-linux.org/      |
| PALADIN |Forensic distro which aims to provide tools for disk imaging and data viewer. Best run as a live session.   | Based on Ubuntu    |https://sumuri.com      |
| CAINE   |Forensic distro which aims to provide basic forensic tools like Autopsy, testdisk, phororec... Both ways are fine with this OS.  | Based on Ubuntu    |https://www.caine-live.net/      |
|GRML     |Forensic distro which aims to provide basic forensic tools for disk imaging. Best run as a live session    | Based on Grml Linux live which is itself based on Debian|https://grml-forensic.org/|
|Renmux|Linux distro dedicated to reverse-engineering. Best run as an installed OS.| Base on Ubuntu|https://docs.remnux.org/|
|Challenger OS|Data recovery distro which aims to provide rescue tools to save data from disk. Best run as an installed OS.|Based on slackware|https://challengeros.com/|
|DEFT Zero|Forensic distro which aims to provide basic forensic tools like Autopsy, testdisk, photorec... Both ways are fine with this OS   | REDACTED          |Discontinued|

Paladin, GRML are distro which are best run as a live session whereas Renmux, Tsuguri is best run as an installed OS. CAINE and DEFT Zero are fine in either way. Please note that all of these OS have built-in write blocking protection for any storage media plugged on the computer. 

Each distro have their pros and cons, personally, I setup a forensic workstation on a Raspberry PI and virtual forensic workstation, using the Tsuguri distro on my own personnal laptop for Forensic CTF.

## Setting your workstation from scratch
### Step N° 1 : Choose the right distro
The first step is to choose the right linux distro. The key here is to choose a Linux OS you're comfortable with. If you have no idea of which one to use, you can try Debian, Ubuntu or Slackware
### Step N° 2 : Install the distro
### Step N° 3 : Configuring your workstation
To avoid unwanted actions on digital evidence such as storage media, we want to make sure that any storage media plugged to our workstation will be marked as "Read-only". Even though a storage media can have the flag "Read-only", it will be still possible for the linux kernel to do write I/O on the storage media. To avoid this issue, you have two ways : 
#### Buy/Install a hardware write blocker 
These device are placed between the workstation and the evidence you want to analyze. Their primary goal is to block any attempt from the workstation to realize write operation on the evidence. The massive cons of this technology is that it can be very expensive to buy one of this devices for the benefits.
#### Buy/Install a software write blocker 
A software write blocker works in the same fashion as a hardware write blocker except that it is a software which is running on your workstation. It can be a third parties software or a even a patch on the Linux kernel. As long as the software configuration is correctly done, its reliability will be the same as an hardware write blocker.
##### How to patch and recompile our own linux kernel
In this section, I'm going to explain how to patch your own kernel to "truly" mark a storage-media as "Read-only" and avoid unwanted write I/O on digital evidences.
The first step you need to do is to open a terminal and type this command :
```
uname -r
```
Based on the output, please refer to the tailored section to your needs to configure your kernel.
###### Version superior to 3.15.1, 4.14, 5.4
First, you need to install the following packages : 
###### Debian, Ubuntu
```
sudo apt-get update && sudo apt-get upgrade && sudo apt-get install 
```
Once it's done, go to /usr/src to find the kernel source. You will need to find the kernel version you want to install. Consult this link to download the right kernel [https://kernel.org/pub/linux/kernel/](https://kernel.org/pub/linux/kernel/)
Then, download the kernel file on your PC : 
```
wget https://link/to/kernel/file.com #Download the kernel
wget https://link/to/kernel/signature.com #Download the kernel signature
```
Uncompress the archive and check the signature 
```
tar -xvzf archive.tar.gz | gpg --verify linux-5.10.62.tar.sign - #If archive is a tar.gz file
unxz -c linux-5.10.62.tar.xz | gpg --verify linux-5.10.62.tar.sign - #If archive is a tar.xz file
```
Once you have the archive, launch the following command
```
cp /boot/config-5.10.0-8-amd64 ~/kernel/linux-source-5.10/.config #Keep the new configuration as close as possible to the previous kernel version
```
Before we go compile the kernel, we need to several files to activate the write blocking ability of the kernel.
```
any_text_editor_software ~/kernel/config-version/block/blk-core.c
```
Go to the line N° 708 or to the function bio_check_ro(struct bio \*bio, struct hd_struct \*part) and change this line : 
```
return false
```
to
```
return true
```
Once it's done, you can save and close the file.
Now that we have patched the source code of the downloaded kernel, we can begin to compile the kernel. 

Please consult the "Credits" section if you want to have additionnal informations regarding this patch from Maxim Suhanov.
### Step N° 4 : Install the needed packages
Now that we have the right foundation to create our forensic workstation, we need to downloads the software which will be needed for forensic purposes.
### Step N° 5 : Post-installation
If you have successfully accomplished all the previous steps, then congralutations ! Enjoy your tuned workstation and don't forget to create a snapshot if it is a virtual machine. This will allow you to go back to a functionnal version of your VM in case you have broken it. 
## Credits
Original linux kernel patch for software write blocker : https://github.com/msuhanov/Linux-write-blocker
Forked linux kernel patch for software write blocker : https://github.com/vitaly-kamluk/Linux-write-blocker