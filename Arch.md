---
title: Arch
filename: Arch.md
---
## Section 1: Preparing the VM  

### 1. Download the ISO from the Arch Downloads
#### 1.1. Pick any of the US versions - verify the hash value
#### 1.2. You do NOT need to alter anything on your computer except within your VM - do not change anything else.  

### 2. Before entering the VM, go into your settings and update it from loading the BIOS to loading UEFI. If done correctly, the command cat /sys/firmware/efi/fw_platform_size will return a value of 64 instead of "file does not exist".  

### 3. Also before entering the VM, make sure you have at least a hard drive of 37 GB allocated for this VM. Arch recommends 1GB for the boot, 4GB for the SWAP, and between 23 and 32 GB for the root.  


## Section 2: The Installation of Arch  

### 4. Run the command **cat /sys/firmware/efi/fw_platform_size**. If it returns a value of 64, congrats! You have properly booted into UEFI mode.  

### 5. Verify your connection to the internet by using ip link and ping archlinux.org. If the ping command works, you are successfully connected to the internet!
![Verified](https://github.com/user-attachments/assets/1a4b6235-761b-40bb-887e-68b2cf178797)  

### 6. Use the fdisk command to partition the disk (format **fdisk /dev/insert_to_be_partitioned_disk**. To set up the suggested UEFI table:
#### 6.1. The boot should be 1GB. The measurements are off, so you will want the boot partition to be from 2048 to +1GB
#### 6.2. The SWAP should be the next default to +4GB
#### 6.3. The root should be the remainder of the space that you have - both defaults will work for this.  
![Full_Partition](https://github.com/user-attachments/assets/18677b36-6d59-41e9-b777-4d42f9f240bf)  

### 7. After this, you will need to go back in and change the type of the boot - it's default type will be Linux and you want EFI. Use t and the boot partition (1) with the hex code ef to change the partition type.  
![Change_to_EFI](https://github.com/user-attachments/assets/923aea40-fae4-453a-84da-123a7871f3db)  

### 8. Formatting the partitions:
#### 8.1. For the root partition (if you followed these instructions, it will be sda3), run mkfs.ext4 /dev/sda3
#### 8.2. For the swap partition (if you followed these instructions, it will be sda2), run mkswap /dev/sda2
#### 8.3. For the efi partition that you created (sda1), run mkfs.fat -F 32 /dev/sda1  

### 9. Mounting:  
![Mounting](https://github.com/user-attachments/assets/f2917393-ac52-49ef-b32d-4ea7a57b1a62)  

### 10. Selecting the Mirrors: there's nothing you need to do for this step, but I highly recommend checking out the mirrors that you will have. You can do this by running **cat /etc/pacman.d/mirrorlist**  

### 11. Installing essential packages. When you run this command, it will take a long time and that is okay and normal! Do not freak out...and you will 100% want to plug in your computer to do this.
#### 11.1. If you only want the necessary items, run pacstrap -K /mnt base linux linux-firmware
#### 11.2. In the documentation, it recommends you installing more packages than this to be of assistance. 
##### 11.2.1. I have an AMD processor on my computer, so I used amd-ucdoe. If yours has Intel, use intel-ucode.
##### 11.2.2. I also thought the sound open firmware would be cool, so I threw that in there as well with sof-firmware
##### 11.2.3. You will eventually want a network manager, so I used the networkmanager package. I chose the NetworkManager since it was the most compatible Network Manager. There are other options, but this is the one that I would recommend using due to its compatibility and its ease.
##### 11.2.4. Won't be caught dead without nano. Added onto the command by typing nano
##### 11.2.5. Need Vim too - vim
##### 11.2.6. All of the man pages... use man-db, man-pages, and texinfo addons to the command.  

### 12. Run **genfstab -U /mnt >> /mnt/etc/fstab** to establish an fstab file. To ensure it was done correctly, run **cat /mnt/etc/fstab**  

### 13. Change root into the arch system by running: **arch-chroot /mnt**  
![arch_chroot](https://github.com/user-attachments/assets/6cedfd0f-0fa5-4900-ac69-dff19c4e1384)  

### 14. To set the timezone, run **ln -sf /usr/share/zoneinfo/_Region_/_City_ /etc/localtime**. Then, you will run **hwclock --systohc** to generate the /etc/adjtime file.
#### 14.1. I ran it as America/Chicago, as I am located in that timezone, but it is customizable.  

### 15. Editing the files:
#### 15.1. nano /etc/locale.gen
##### 15.1.1. Uncomment en_US.UTF-8 UTF-8 and whatever other UTF-8s you need. I only needed the first one.
##### 15.2. nano /etc/locale.conf (This will create the file. You will then set the language to the value above: LANG=en_US.UTF-8)
##### 15.3. nano /etc/hostname (This creates the file. You are free to name your hostname as you please. I named mine archibald.)  

### 16. Network Manager
#### 16.1. Enable and start NetworkManager by using the commands: systemctl start NetworkManager
##### 16.1.1. If this doesn't work, run systemctl enable NetworkManager first
##### 16.1.2. There is a chance that if you installed NetworkManager before that it did not actually install. To fix any error saying networkmanager.service doesn't exit, run pacman -Sy networkmanager to install NetworkManager.  

### 17. Setting the passwd: run **passwd** and create a password.   

### 18. Boot Loader:
#### 18.1. I chose to use the GRUB boot loader since it was the most compatible boot loader. There are other options, but the instructions will follow this one.
#### 18.2. Install grub and the efi manager by running: pacman -S grub efibootmanager
#### 18.3. You have already mounted the efi partition, so don't worry about these commands not running
#### 18.4. To install GRUB-EFI, run grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
#### 18.5. Make the GRUB configuration file by running: grub-mkconfig -o /boot/grub/grub.cfg  
![Grub_Config_File](https://github.com/user-attachments/assets/879bfd79-cd0a-42fa-882a-e4e8a8e398e5)  

### 19. Just in case something happened, rerun systemctl enable NetworkManager.service and systemctl start NetworkManager.service  

### 20. After this, exit the chroot by running chroot and then run reboot. Boot into Arch Linux normally. Congrats! You have installed Arch Linux!  

### 21. Before you move on, shut down the VM and change the CD setting to Use Physical Device instead of using the ISO.  


## Part 2: Setting up the Desktop, Users, and Other Stuff  
### 24. Install Xorg by running: pacman -Sy xorg-server  
### 25. Figure out what video drivers are compatible (run **lscpi -v -d ::03xx**) then see what video drivers are available (**pacman -Ss xf86-video**). Pick one by installing it normally with pacman.  
![XVideo_Driver](https://github.com/user-attachments/assets/5b87030a-37de-4387-ad1a-67906f52487d)  
### 26. // Can Xorg :0 -configure to configure Xorg ==(NOT SURE THIS WORKED)==  
### 27. Install LXQt, breeze-icons, SDDM, and sudo with pacman (all package names in lowercase)  
### 28. Before you move to enable and start sddm, you will have to add a user. You will NOT be able to login to access ANYTHING if you do not do this first.
#### 28.1. Add the user using **useradd yourname -m**.  
##### 28.1.1. It's really important you do NOT use the -p option to set the password. Whatever you put after -p is what will be saved to /etc/shadow as the full password **hash**, thereby making your password something you don't actually know.  
##### 28.1.2. It is also critical to add the -m. This creates the user's home directory. If you do not add this, you will not be able to access the desktop when you try to login!  
##### 28.1.3. I created all of my users back to back to save time, but as long as you have one added, you can navigate the desktop environment.  
![AllUsers](https://github.com/user-attachments/assets/2f93d035-9313-4aff-b465-3e4083024571)  
#### 28.2. To properly set the password for your first account, run passwd yourname and follow the instructions in the terminal. Do this for any other user that you know what password to set up for them. You cannot login as this user until you set up a password for them.  
#### 28.3. You can add the users as sudoers any time, but it's easier to do it now than later. For arch, this is the wheel group. The command to add a user to the wheel group is **usermod -a -G wheel yourname**. You still have to nano into the /etc/sudoers file and uncomment the wheel group's access to root before you can run commands.
### 29. Enable and start the sddm service (booting up the desktop interface) by running systemctl enable sddm.service and systemctl start sddm.service. If you need to return to your terminal, use Control+Alt+Any F Key to move back to your root terminal.


## Part 3: Configuring the Desktop Environment  

### Yay! You now have a desktop environment to work with!  

### 30. In the top left corner of the desktop interface, double-check that it is set to LXQt Desktop.  
### 31. Sign in with the user you created before. Now you run into a problem - your Computer and Network links on the desktop throw an error "Operation not permitted". Aiden K and DietPi recommended installing gvfs, which allows apps to access remote resources. In this case, it gives the desktop environment permission to access the terminal.
### 32. Once in the environment, I need to install a terminal other than bash, so I installed fish using pacman -Sy fish
### 33. I also needed to install ssh, so I ran pacman -Sy ssh
### 34. Adding aliases - I went to my go-to aliases: **c="clear"** and **update="sudo pacman -Syu"**. Making aliases in fish will NOT save them automatically and export does not do the trick either! What you have to do is include --save after alias (alias --save c="clear. This saves it to a config file in fish to be reused later. These save to /home/USER/.config/fish/functions/c.fish
![Aliases](https://github.com/user-attachments/assets/18700474-e211-4ac3-a79d-ee72daf87707)  
### 35. Installed Firefox: sudo pacman -Sy firefox
### 36. The SysV runlevel I want is the graphical interface, which means my command to change the default boot is **systemctl set-default graphical.target**

## This is all you have to do to install Arch and set up a desktop environment for it!
