---
title: OldArchTemplate
filename: OldArchTemplate.md
---

### 1. Download the ISO from the Arch Downloads
#### 1.1. Pick any of the US versions - verify the hash value
#### 1.2. You do NOT need to alter anything on your computer except within your VM - do not change anything else.  

### 2. Before entering the VM, go into your settings and update it from loading the BIOS to loading UEFI. If done correctly, the command cat /sys/firmware/efi/fw_platform_size will return a value of 64 instead of "file does not exist".  

### 3. Also before entering the VM, make sure you have at least a hard drive of 37 GB allocated for this VM. Arch recommends 1GB for the boot, 4GB for the SWAP, and between 23 and 32 GB for the root.  

### 4. When the VM opens, press enter on "Arch Linux install medium"  

### 5. Run the command cat /sys/firmware/efi/fw_platform_size. If it returns a value of 64, congrats! You have properly booted into UEFI mode.  

### 6. Verify your connection to the internet by using ip link and ping archlinux.org. If the ping command works, you are successfully connected to the internet!  

### 7. To set up the suggested UEFI table:
#### 7.1. The boot should be 1GB. The measurements are off, so you will want the boot partition to be from 2048 to +1GB
#### 7.2. The SWAP should be the next default to +4GB
#### 7.3. The root should be the remainder of the space that you have - both defaults will work for this.  

### 8. After this, you will need to go back in and change the type of the boot - it's default type will be Linux and you want EFI. Use t and the boot partition (1) with the hex code ef to change the partition type.  

### 9. Once again, use t and the swap partition with the hex code 82 to change the partition type to Swap for the SWAP partition.  

### 10. Formatting the partitions:
#### 10.1. For the root partition (if you followed these instructions, it will be sda3), run mkfs.ext4 /dev/sda3
#### 10.2. For the swap partition (if you followed these instructions, it will be sda2), run mkswap /dev/sda2
#### 10.3. For the efi partition that you created (sda1), run mkfs.fat -F 32 /dev/sda1  

### 11. Mounting:  

### 12. Selecting the Mirrors: there's nothing you need to do for this step, but I highly recommend checking out the mirrors that you will have. You can do this by running cat /etc/pacman.d/mirrorlist  

### 13. Installing essential packages. When you run this command, it will take a long time and that is okay and normal! Do not freak out...and you will 100% want to plug in your computer to do this.
#### 13.1. If you only want the necessary items, run pacstrap -K /mnt base linux linux-firmware
#### 13.2. In the documentation, it recommends you installing more packages than this to be of assistance. 
##### 13.2.1. I have an AMD processor on my computer, so I used amd-ucdoe. If yours has Intel, use intel-ucode.
##### 13.2.2. I also thought the sound open firmware would be cool, so I threw that in there as well with sof-firmware
##### 13.2.3. You will eventually want a network manager, so I used the networkmanager package. I chose the NetworkManager since it was the most compatible Network Manager. There are other options, but this is the one that I would recommend using due to its compatibility and its ease.
##### 13.2.4. Won't be caught dead without nano. Added onto the command by typing nano
##### 13.2.5. Need Vim too - vim
##### 13.2.6. All of the man pages... use man-db, man-pages, and texinfo addons to the command.  

### 14. Run genfstab -U /mnt >> /mnt/etc/fstab to establish an fstab file. To ensure it was done correctly, run cat /mnt/etc/fstab  

### 15. Change root into the arch system by running: arch-chroot /mnt  

### 16. To set the timezone, run ln -sf /usr/share/zoneinfo/_Region_/_City_ /etc/localtime. Then, you will run hwclock --systohc to generate the /etc/adjtime file.
#### 16.1. I ran it as America/Chicago, as I am located in that timezone, but it is customizable.  

### 17. Editing the files:
#### 17.1. nano /etc/locale.gen
##### 17.1.1. Uncomment en_US.UTF-8 UTF-8 and whatever other UTF-8s you need. I only needed the first one.
##### 17.2. nano /etc/locale.conf (This will create the file. You will then set the language to the value above: LANG=en_US.UTF-8)
##### 17.3. nano /etc/hostname (This creates the file. You are free to name your hostname as you please. I named mine archibald.)  

### 18. Network Manager
#### 18.1. Enable and start NetworkManager by using the commands: systemctl start NetworkManager
##### 18.1.1. If this doesn't work, run systemctl enable NetworkManager first
##### 18.1.2. There is a chance that if you installed NetworkManager before that it did not actually install. To fix any error saying networkmanager.service doesn't exit, run pacman -Sy networkmanager to install NetworkManager.  

### 19. Setting the passwd: run passwd and then choose a password. I used Archy for my password (idgaf)  

### 20. Boot Loader:
#### 20.1. I chose to use the GRUB boot loader since it was the most compatible boot loader. There are other options, but the instructions will follow this one.
#### 20.2. Install grub and the efi manager by running: pacman -S grub efibootmanager
#### 20.3. You have already mounted the efi partition, so don't worry about these commands not running
#### 20.4. To install GRUB-EFI, run grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
#### 20.5. ~~After this, you will be given a choice to use CA keys or Shimlock to configure Secure Boot. I chose to use CA keys, so I reran the previous command with another variable added on: grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB --modules="tpm" --disable-shim-lock~~
#### 20.6. ~~After this, you will want to install sbctl to create and automate the CA keys. To do this, run pacman -Sy sbctl~~
##### 20.6.1. ~~To check that this has been run properly, run sbctl status. It should show that sbctl is NOT installed and Secure boot is disabled.~~
##### 20.6.2. ~~I shall be skipping secure boot bc I do not care~~
#### 20.7. Make the GRUB configuration file by running: grub-mkconfig -o /boot/grub/grub.cfg  

### 21. Just in case something happened, rerun systemctl enable NetworkManager.service and systemctl start NetworkManager.service  

### 22. After this, exit the chroot by running chroot and then run reboot. Boot into Arch Linux normally. Congrats! You have installed Arch Linux!  


## Part 2: Setting up the Desktop, Users, and Other Stuff
### 1. Now we move onto the DE:  

### 2. Install Xorg by running: pacman -Sy xorg-server  

### 3. Check your video drivers by running: lscpi -v -d ::03xx and then running: pacman -Ss xf86-video  

### 4. Can Xorg :0 -configure to configure Xorg ==(NOT SURE THIS WORKED)==  

### 5. Installing LXQT: pacman -Sy lxqt  

### 6. Installing Breeze Icons: pacman -Sy breeze-icons  

### 7. Installing SDDM (recommended display manager): pacman -Sy sddm  

### 8. Enable and start the sddm service by running systemctl enable sddm.service and systemctl start sddm.service
#### 8.1. Note: By running systemctl start sddm.service, you are starting the desktop interface.  

### 9. In the top left corner of the desktop interface, double-check that it is set to LXQt Desktop
### 10. 
