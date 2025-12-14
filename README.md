This repository has been tested with Intel® Optane™ Persistent Memory 100 Series 128GB modules on a HPE DL360GEN10 using 2x Xeon Platinum 8259CL

![image](https://img.fish/d5c736.png)

## build instructions

install deps: `apt install build-essential debhelper-compat libtest-mockmodule-perl lintian pve-doc-generator`

clone repo -> cd repo -> make dinstall

## proxmox install instructions
```
Open Proxmox Installer as Debug
First shell -> exit

nano /usr/share/perl5/Proxmox/Sys/Block.pm

CTRL + F -> nvme - then either add:

sector mode 
} elsif ($dev =~ m|^/dev/pmem\d+s$|) {
    return "${dev}$partnum";
}

fsdax mode
} elsif ($dev =~ m|^/dev/pmem\d+$|) {
    return "${dev}p$partnum";
}

*save*
exit

*Do installer as normal*

mkdir /target

*mounts will be different based on lvm/names/such*
mount /dev/mapper/pve-root /target
mount /dev/pmem0s2 /target/boot/efi
*                                                *

mount --bind /dev /target/dev
mount --bind /proc /target/proc
mount --bind /sys /target/sys
mount --bind /run /target/run
chroot /target /bin/bash

nano /etc/initramfs-tools/modules

nfit
libnvdimm
pmem
nd_pmem
dax_pmem
nd_btt

*save*

update-initramfs -k all -u 
exit
umount -R /target/boot/efi
umount -R /target/
exit
```
