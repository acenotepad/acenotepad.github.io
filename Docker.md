---
title: Docker
filename: Docker.md
---

## This is a guide to install Docker Desktop on Ubuntu Linux

Pre-Step: In case Gnome is not installed, run sudo apt install gnome-terminal before beginning

Setting up the apt repository:
1. Run sudo apt-get update (updates everything)
2. Run sudo apt-get install ca-certificates curl
3. Run sudo install -m 0755 -d /etc/apt/keyrings
4. Run sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
5. Run sudo chmod a+r /etc/apt/keyrings/docker.asc
6. Run echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

Edit: turns out, I did NOT need docker desktop. Lolz
Download the latest DEB package. To install it, cd into your Downloads folder and run sudo apt-get install ./docker-desktop-amd64.deb. Be prepared to get an error message at the end about something being unsandboxed as root - this is expected.

Before opening the Docker Desktop, you must make sure the *CORRECT* kvm module is installed. I have the wrong one installed on my VMs apparently and it will not allow me to install the correct ones.
I will document what was used to attempt to fix this issue, but none of these fixed the problem with my machine. 
1. Altering the settings of the virtual machine to enable Virtualization before opening the machine: Edit Virtual Machine Settings -> Processors -> Virtualize Engine -> Click both checkboxes.
2. Disable Hypervisor on your home machine: Run cmd as Administrator -> bcedit /set hypervisorlaunchtype off


Installing Docker Engine!
1. Install the latest Docker packages (as of November 15th, 2024) by running sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
2. 
