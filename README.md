# CYB-3353-01-22-FA-Arch-Linux-Installation-Guide
A short Arch Linux installation guide with documentation to recreate my custom Arch Linux virtual machine.

## Arch Linux Setup
1. Utilizing the VMWare's student subscription, I requested for VMware Workstation Pro 16
2. I then installed the Arch Linux iso from the mit.edu on the direct downloads page on the Arch Linux wiki
3. During the installation process of VMware I used the memory allocation specified on the slides given to us in class.
4. In this setup process I also edited the .vmx file within the VMware folder to add `firmware="efi"` into the second line to enable UEFI
5. After booting the environment, I verfied my network connection `ping archlinux.org`
6. I then used `timedatectl status` and checked if the system clock lined up with my laptop's time
7. `fdisk -l` was used to identify the disks, where I created two partitions for sda1 and sda2 as instructed in the slides.
8. I used `cfdisk` and partitioned the disks are dictated by the slides. 
9. I mounted sda1 to `mkfs.ext4 /dev/root_partition` because it is the EFI partition and I used `mkfs.fat -F 32 /dev/efi_system_partition` for the other partition
10. sda1 was mounted to /mnt using `mount --mkdir /dev/efi_system_partition /mnt/boot`

### Arch Linux Installation

