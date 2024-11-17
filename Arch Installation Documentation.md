1. Download the ISO from the Arch Downloads
	1. Pick any of the US versions - verify the hash value
	2. You do NOT need to alter anything on your computer except within your VM - do not change anything else.
2. Before entering the VM, go into your settings and update it from loading the BIOS to loading UEFI. If done correctly, the command cat /sys/firmware/efi/fw_platform_size will return a value of 64 instead of "file does not exist".
3. Also before entering the VM, make sure you have at least a hard drive of 37 GB allocated for this VM. Arch recommends 1GB for the boot, 4GB for the SWAP, and between 23 and 32 GB for the root.
4. When the VM opens, press enter on "Arch Linux install medium"
5. Run the command cat /sys/firmware/efi/fw_platform_size. If it returns a value of 64, congrats! You have properly booted into UEFI mode.
6. Verify your connection to the internet by using ip link and ping archlinux.org. If the ping command works, you are successfully connected to the internet!
7. To set up the suggested UEFI table:
	1. See what disks you have available: fdisk -l. There should be one with sda, but make sure it's not one of the imaginary disks 
	2. Run fdisk /dev/disktobepartitioned 
	3. The boot should be 1GB. The measurements are off, so you will want the boot partition to be from 2048 to +1GB
	4. The SWAP should be the next default to +4GB
	5. The root should be the remainder of the space that you have - both defaults will work for this.
9. After this, you will need to go back in and change the type of the boot - it's default type will be Linux and you want EFI. Use t and the boot partition (1) with the hex code ef to change the partition type. 
10. Once again, use t and the swap partition with the hex code 82 to change the partition type to Swap for the SWAP partition.
11. Formatting the partitions:
	1. For the root partition (if you followed these instructions, it will be sda3), run mkfs.ext4 /dev/sda3
	2. For the swap partition (if you followed these instructions, it will be sda2), run mkswap /dev/sda2
	3. For the efi partition that you created (sda1), run mkfs.fat -F 32 /dev/sda1
12. Mounting:
13. Selecting the Mirrors: there's nothing you need to do for this step, but I highly recommend checking out the mirrors that you will have. You can do this by running cat /etc/pacman.d/mirrorlist
14. Installing essential packages. When you run this command, it will take a long time and that is okay and normal! Do not freak out...and you will 100% want to plug in your computer to do this.
	1. If you only want the necessary items, run pacstrap -K /mnt base linux linux-firmware
	2. In the documentation, it recommends you installing more packages than this to be of assistance. 
		1. I have an AMD processor on my computer, so I used amd-ucode. If yours has Intel, use intel-ucode.
		2. I also thought the sound open firmware would be cool, so I threw that in there as well with sof-firmware
		3. You will eventually want a network manager, so I used the networkmanager package.
			1. I chose the NetworkManager since it was the most compatible Network Manager. There are other options, but this is the one that I would recommend using due to its compatibility and its ease.
		4. Won't be caught dead without nano. Added onto the command by typing nano
		5. Need Vim too - vim
		6. All of the man pages... use man-db, man-pages, and texinfo addons to the command.
15. Run genfstab -U /mnt >> /mnt/etc/fstab to establish an fstab file. To ensure it was done correctly, run cat /mnt/etc/fstab
16. Change root into the arch system by running: arch-chroot /mnt
17. To set the timezone, run ln -sf /usr/share/zoneinfo/_Region_/_City_ /etc/localtime. Then, you will run hwclock --systohc to generate the /etc/adjtime file.
	1. I ran it as America/Chicago, as I am located in that timezone, but it is customizable.
18. Editing the files:
	1. nano /etc/locale.gen
		1. Uncomment en_US.UTF-8 UTF-8 and whatever other UTF-8s you need. I only needed the first one.
	2. nano /etc/locale.conf
		1. This will create the file. You will then set the language to the value above: LANG=en_US.UTF-8
	3. nano /etc/hostname
		1. This creates the file. You are free to name your hostname as you please. I named mine archibald.
19. Network Manager
	1. Enable and start NetworkManager by using the commands:
		2. systemctl start NetworkManager
			1. If this doesn't work, run systemctl enable NetworkManager first
	2. There is a chance that if you installed NetworkManager before that it did not actually install. To fix any error saying networkmanager.service doesn't exit, run pacman -Sy networkmanager to install NetworkManager.
20. Setting the passwd: run passwd and then choose a password. I used Archy for my password (idgaf)
21. Boot Loader:
	1. I chose to use the GRUB boot loader since it was the most compatible boot loader. There are other options, but the instructions will follow this one.
	2. Install grub and the efi manager by running: pacman -S grub efibootmanager
	3. You have already mounted the efi partition, so don't worry about these commands not running
	4. To install GRUB-EFI, run grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
	5. ~~After this, you will be given a choice to use CA keys or Shimlock to configure Secure Boot. I chose to use CA keys, so I reran the previous command with another variable added on: grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB --modules="tpm" --disable-shim-lock~~
	6. ~~After this, you will want to install sbctl to create and automate the CA keys. To do this, run pacman -Sy sbctl~~
		1. ~~To check that this has been run properly, run sbctl status. It should show that sbctl is NOT installed and Secure boot is disabled.~~
		2. ~~I shall be skipping secure boot bc I do not care~~
	7. Make the GRUB configuration file by running: grub-mkconfig -o /boot/grub/grub.cfg
22. Just in case something happened, rerun systemctl enable NetworkManager.service and systemctl start NetworkManager.service
23. After this, exit the chroot by running chroot and then run reboot. Boot into Arch Linux normally. Congrats! You have installed Arch Linux!
24. Before you move on, shut down the VM and change the CD setting to Use Physical Device instead of using the ISO


Now we move onto the DE:
24. Install Xorg by running: pacman -Sy xorg-server
25. // Check your video drivers by running: lscpi -v -d ::03xx and then running: pacman -Ss xf86-video
	1. I WAS WRONG. The above command shows a list of all of the available video drivers. Pick one and run it. I ran pacman -Sy xf86-video-vmware
26. // Can Xorg :0 -configure to configure Xorg ==(NOT SURE THIS WORKED)==
27. Installing LXQT: pacman -Sy lxqt
28. Installing Breeze Icons: pacman -Sy breeze-icons
29. Installing SDDM (recommended display manager): pacman -Sy sddm
30. After this, I would highly, highly recommend installing the sudo package. You will need it to add your users to sudoers. 
	1. pacman -Sy sudo
31. Before you move to enable and start sddm, you will have to add a user. You will NOT be able to login to access ANYTHING if you do not do this first.
	1. Add the user by using the useradd command. Do NOT create a home directory for them yet! 
		1. useradd yourname -m
		2. It's really important you do NOT use the -p option to set the password. Whatever you put after -p is what will be saved to /etc/shadow as the full password **hash**, thereby making your password something you don't actually know.
		3. It is also critical to add the -m. This creates the user's home directory. If you do not add this, you will not be able to access the desktop when you try to login!
		4. I decided to just go for it and create all of the users I needed. For my class, I had to add two other users. When created, they will show up on the login page in a row
	2. To properly set the password for your first account, run passwd yourname and follow the instructions in the terminal. Do this for any other user that you know what password to set up for them. You cannot login as this user until you set up a password for them.
	3. You can add the users as sudoers any time, but it's easier to do it now than later. For arch, this is the wheel group. The command to add a user to the wheel group is
		1. usermod -a -G wheel yourname
		2. You still have to nano into the /etc/sudoers file and uncomment the wheel group's access to root before you can run commands.
32. Enable and start the sddm service by running systemctl enable sddm.service and systemctl start sddm.service
	1. Note: By running systemctl start sddm.service, you are starting the desktop interface. If you need to return to your terminal, use Control+Alt+Any F Key to move back to your root terminal.
33. In the top left corner of the desktop interface, double-check that it is set to LXQt Desktop

Trial and Error with Sign In Page:
1. Installed nvidia to try to mitigate problems with sddm - did not work (snapshotted back later)
2. Deleted a ton of VMs (one of which i didn't know I had) - did not work Added 40 gigs of storage 
3. systemctl set-default graphical.target and then reboot - did not work (snapshotted back later)
4. This is the point where I messaged the professor. I forgot to create the home directory. This won't happen to you if you just create the home directory where is says to above. 

Desktop Environment
You finally have your desktop environment! Yay!
1. Sign in with the user you created before. Now you run into a problem - your Computer and Network links on the desktop throw an error "Operation not permitted". Aiden K and DietPi recommended installing gvfs, which allows apps to access remote resources. In this case, it gives the desktop environment permission to access the terminal.
2. Once in the environment, I need to install a terminal other than bash, so I installed zsh using pacman -Sy zsh
3. I also needed to install ssh, so I ran pacman -Sy ssh
4. Add some aliases. You will get tired of typing things. Here is a list of what I added:
	1. c='clear'
	2. sudoers='/etc/sudoers'
	3. sudoersd='/etc/sudoers.d'
	4. update='sudo pacman -Syu'
5. Installed Firefox: sudo pacman -Sy firefox
6. I'm having a really hard time finding documentation for color coding the terminal
7. 


