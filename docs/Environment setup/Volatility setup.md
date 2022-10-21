# Volatility
## Install Volatility
## Creation of volatility profile
### Windows
### Mac OS X
You can find pre-builts volatility profile on this link : https://github.com/volatilityfoundation/profiles
If the needed
### Linux 
https://github.com/p0dalirius/volatility3-symbols
https://github.com/p0dalirius/volatility2-profiles
https://isf-server.techanarchy.net/
https://volatility3.readthedocs.io/en/latest/symbol-tables.html#mac-linux-symbol-tables
https://github.com/volatilityfoundation/profiles

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
### Android
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

## Sources
https://source.android.com/docs/setup/build/building-kernels
https://github.com/volatilityfoundation/volatility/wiki/Androiddir

https://unix.stackexchange.com/questions/238783/how-can-i-create-a-swap-file
