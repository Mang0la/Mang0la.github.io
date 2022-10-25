# CYB-3353-01-22-FA-Arch-Linux-Installation-Guide
A short Arch Linux installation guide with documentation to recreate my custom Arch Linux virtual machine.

## Arch Linux Setup
1. Utilizing the VMWare's student subscription, I requested for VMware Workstation Pro 16
2. I then installed the Arch Linux iso from the mit.edu on the direct downloads page on the Arch Linux wiki
3. During the installation process of VMware I used the memory allocation specified on the slides given to us in class.
4. In this setup process I also edited the .vmx file within the VMware folder to add `firmware="efi"` into the second line to enable UEFI
5. After booting the environment, I verfied my network connection `ping archlinux.org`
6. I then used `timedatectl status` and checked if the system clock lined up with my laptop's time
7. `fdisk -l` was used to identify the disks, where I used `cfdisk` and partitioned the disks where sda1 is allocated 1M and set to boot, sda2 is allocated 4G and its type is Linux swap, and sda3 is allocated the rest of the space.
8. I formatted using `mkfs.ext4 /dev/sda3` because it is the EFI partition and I used `mkswap /dev/sda2` and `swapon -a` for the other partition. I also mounted sda3 using `mount /dev/sda3 /mnt`.
> I originally ran into the issue of either the partition not having enough space or an error in formatting when attempting to follow the hints left on the slides. So I decided diverging from the slides and following a YouTube guide that used Steps 7 & 8.

## Arch Linux Installation & Configuration
1. I installed the base, linux, and linux-firmware packages using `pacstrap -K /mnt base linux linux-firmwar`
2. I generated an fstab file using `genfstab -U /mnt >> /mnt/etc/fstab`
3. I changed root with `arch-chroot /mnt`, used `ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime` to set the local time to Central Standard Time, and `hwclock --systohc` to create /etc/adjtime
4. The next few steps required nano so I installed it using `pacman --sync vim nano` where I created my localization files, console keyboard layout, and network configuration according to steps 3.3-3.5 on the Arch Linux Wiki.
5. `mkinitcpio -P` was used to generate a new initramfs image
6. A root password was set using `passwd`
7. Finally I installed the amd microcode due to my system's CPU using `pacman -S amd-unicode` which will be automatically detected with grub. I installed grub as my bootloader using `pacman -S grub`, `grub-install /dev/sda`, and `grub-mkconfig -o /boot/grub/grub.cfg`.
> When booting back in, I had the largest issues from my keyboard being in the wrong language to being unable to connect to the internet. I fixed the language issue by finding the Latin American keyboard layout and navigating into the keyboard configs and changed KEYMAP to US. I then fixed the network connection issue by searching online and finding a solution. I had to create a file `/etc/systemd/network/20-wired.network` which I inserted:

[Match]
Name=*

[Network]
DHCP=yes

> Then I ran the commands `systemctl start systemd-networkd` and `systemctl start systemd-resolved`. I quickled installed dhcpcd using `pacman -S dhcpcd` and had it enabled using `systemctl enable dhcpcd`.

- I switched from dhcpcd to Network Manager as my research online as it seemed better for configurations and wireless connections.
- `sudo pacman -Syu`, `sudo pacman -S wpa_supplicant wireless_tools networkmanager`, `sudo pacman -S nm-connection-editor network-manager-applet` updates packages and apps to use Network Manager 
- `sudo systemctl enable NetworkManager.service` & `sudo systemctl enable wpa_supplicant.service` to enable to service and `sudo systemctl disable dhcpcd.service` as it is incompatible with Network Manager.

# ONCE LOGGED BACK IN PLEASE TAKE A SNAPSHOT TO SAVE PROGRESS

## User Account
1. `sudo useradd -m thomas` - Creates a user 'thomas' with a home dir
2. `sudo passwd thomas` - changes the password for the newly created user
3. The same above commands are done but for user `codi` instead with a password of GraceHopper1906
4. `sudo passwd -e codi` - Sets password of user `codi` into an expired status
5. `sudo groupadd sudo`, `sudo usermod -a -G sudo thomas`, and `sudo usermod -a -G sudo codi`, grants the users sudo permissions

## Desktop Environment
I chose KDE as a GUI-based desktop environment to make it easier to navigate through the VM. 
1. 	`lspci | grep -e VGA` - checkes the latest graphics drivers
2. 	`sudo pacman -Syyu` - updates system
3. 	`pacman -S xorg` & `pacman -S xorg-server` - installs display server packages
4. 	`sudo pacman -S nvidia nvidia-utils nvidia-settings` - installs nvidia drivers and settings since I have an nvidia graphics card
5. 	`pacman -S plasma plasma-meta`, `pacman-S plasma-wayland-session kde-applications`, `sudo pacman -S plasma kdeplasma-addons` - installs KDE Plasma Desktop Environment and its addons
6. 	`systemctl enable sddm.service` - enables the display manager needed 

> I had to redo the entire process because I did not set up a snapshot nor did I setup users to log into for the desktop environment

