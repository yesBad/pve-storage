![image](https://img.fish/d5c736.png)

## build instructions

install deps: `apt install build-essential debhelper-compat libtest-mockmodule-perl lintian pve-doc-generator`

clone repo -> cd repo -> make dinstall

## proxmox install instructions
```
Open Proxmox Installer as Debug
First shell -> exit

nano /usr/share/perl5/Proxmox/Sys/Block.pm

CTRL + F -> nvme - then add:
} elsif ($dev =~ m|^/dev/pmem\d+s$|) {
    return "${dev}$partnum";
}

*save*
exit

*Do installer as normal*

mkdir /target
mount /dev/mapper/pve-root /target
mount /dev/pmem0s2 /target/boot/efi
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
reboot
```
