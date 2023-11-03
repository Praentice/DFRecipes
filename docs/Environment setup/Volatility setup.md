# Volatility 2 and 3 setup
The aim of this pages is to give tips and tricks regarding the installation and setup of Volatility 2 and 3.
## Install Volatility
### Volatility 2
### Volatility 3
## Get custom existing volatility profile and symbols
### Precompiled memory profile and symbols
#### Volatility 2
Here's a list of repo on which you can find precompiled memory profile for volatility2

|Description|Link|
|-|-|
|Volatility2 symbols from Podalirius|[Link](https://github.com/p0dalirius/volatility2-profiles)|
|Volatlity2 Compilation of volatility2 profiles by volatility foundation|[Link](https://github.com/volatilityfoundation/profiles)|
#### Volatility 3
Here's a list of repo on which you can find precompiled symbols for volatility3

|Description|Link|
|-|-|
|Volatility3 symbols from Podalirius|[Link](https://github.com/p0dalirius/volatility3-symbols)|
|Isf server from techanarchy|[Link](https://isf-server.techanarchy.net/)|
|Volatility 3 symboles tables|[Link](https://volatility3.readthedocs.io/en/latest/symbol-tables.html#mac-linux-symbol-tables)|
### Creating you own memory profile and symbols
#### Windows
Volatility2 and 3 comes with a lot of prebuilts profiles and symbols for Windows

#### Mac OS X
##### Volatility2

You can find pre-builts volatility profile on this link : https://github.com/volatilityfoundation/profiles

##### Volatility3

#### Linux 
##### Volatility2

#### Create a virtual machine to build symbols or a memory profile
```
gunzip QEMU_EFI.img.gz
qemu-img create -f qcow2 debian.img 128G
qemu-img create -f qcow2 varstore.img 64M

qemu-system-aarch64 \
    -cpu cortex-a53 -M virt -m 4096 \
    -drive if=pflash,format=raw,file=QEMU_EFI.img \
    -drive if=pflash,file=varstore.img \
    -drive if=virtio,file=debian.img \
    -drive if=virtio,format=raw,file=ubuntu-18.04.6-server-arm64.iso
    
qemu-system-aarch64 \
    -cpu host -M virt,accel=kvm -m 4096 -nographic \
    -kernel arch/arm64/boot/Image -append root=/dev/vda2 \
    -drive if=virtio,file=debian.img


```
#### Android

https://github.com/SorCelien/CTF-WRITEUPS/blob/main/FCSC-2021/forensics/ordiphone-1.md

```bash
mkdir ~/android-kernel && cd ~/android-kernel
repo init -u https://android.googlesource.com/kernel/manifest - kernel_version
repo sync -c -j8
BUILD_CONFIG=common/build.config.gki.x86_64 build/build.sh
```
#### Troubleshooting
##### Stuck on "LTO vmlinux.o" during kernel compile
There is not enough memory allocated to the compiler during link of vmlinux.o file.
Create a swapfile to allocate more memory by using these commands
```
sudo -i #Become root
dd if=/dev/zero of=/path/swapfile bs=1G count=size_of_swapfile status=progress #Create swap file
chmod 600 /path/swapfile #Modify permissions so only root can use it
mkswap /path/swapfile #Make the file a swap file
swapon /path/swapfile #Enable the swap file
cat /proc/swaps # Check that the swapfile is indeed used by system
echo "/path/swapfile        none        swap        sw        0        0" >> /etc/fstab
```

## Configure volatility 2 and 3 to use them
### Adding a custom memory profile or symbols
#### Volatility 2
Once you found the appropriate memory profile for your investigation, you need to setup Volatility2 in order to use it. Here's the following steps to do this.


#### Volatility 3
Once you found the appropriate symbols for your memory dump, you need to setup Volatility3 in order to use it. Here's the following steps to do this.


## Resources

https://source.android.com/docs/setup/build/building-kernels

https://github.com/volatilityfoundation/volatility/wiki/Androiddir

https://unix.stackexchange.com/questions/238783/how-can-i-create-a-swap-file

