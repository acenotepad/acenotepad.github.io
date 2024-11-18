---
title:Docker2
filepath:/Docker2.md
---

This is a guide to install Docker Desktop on Ubuntu Linux

I had to make myself a sudoer, which I did by escalating to root and editing the sudoers file by adding: echo ' username ALL=(ALL) ALL >> /etc/sudoers

Setting up the apt repository:
1. Run: **sudo apt-get update** and **sudo apt-get upgrade** (updates and upgrades everything)
2. Run: **sudo apt-get install ca-certificates curl** (installs ca-certificates and curl)
	1. This process installed libcurl4 and curl and updated 'ca-certificated libcurl4'
3. Run: **sudo install -m 0755 -d /etc/apt/keyrings** (installs in permisison mode and treats all arguments as directories)
	1. I received no output for this command, so we'll see if it's done correctly
4. Run: **sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc** (runs curl on the link and outputs the result to the filepath starting at /etc listed)
	1. This command also gave me no output
5. Run: **sudo chmod a+r /etc/apt/keyrings/docker.asc** (assigns user, group, and others the read permission for the file with the part starting at /etc)
	1. No output
6. Run: **echo \ "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \ $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \ sudo tee /etc/apt/sources.list.d/docker.list > /dev/null** 
	1. No output

Part 2: Installing Docker Engine!
1. Install the latest Docker packages (as of November 15th, 2024) by running **sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin**
	1. This process installs packages: docker-ce-rootless-extras git git-man liberror-perl libslirp0 pigz; slirp4netns
	2. This process installs NEW packages:  containerd.io docker-buildx-plugin docker-ce docker-ce-cli; docker-ce-rootless-extras docker-compose-plugin git git-man liberror-perl; libslirp0 pigz; slirp4netns
2. To test the install, run sudo docker run hello-world
	1. This runs a 'Welcome to Docker' message! If your output matches the screenshot below, congrats! You've installed Docker properly!
3. Docker Compose should have been installed either with the prior Docker install or the Docker Engine install. If you are able to run docker compose version and get a version number, you have Docker Compose ready to go!


Part 3: The OpenVas Container and the Security Scan
1. Run **sudo apt-get update and sudo apt-get upgrade to make sure your software is up to date before beginning installation**
2. **sudo docker run -d -p 443:443 --name openvas mikesplain/openvas**
	1. -d runs the docker container in the background - it does not find it locally, so it installs it to run
	2. -p publishes the container to the host
	3. The rest of the command names the new container 'openvas' and uses the image from mikesplain/openvas as the base for the container.
3. I created an directory titled 'openvasdocker' and made the docker-compose.yml file by downloading the yml file from the github repo to it.
4. After this is done TAKE A SNAPSHOT OF YOUR MACHINE. If you mess up the next part, you will want to have a snapshot to revert to so you do not lose your progress entirely. 
5. Running the Vulnerability Scan
	1. Go to https://localhost/ and login to the site with credentials admin/admin
	2. Create a task by clicking on Tasks under the Configuration tab and pressing the star. Keep the defaults, but change the IP to one of your IPs. 
	3. Create the scan by clicking on Tasks under the Scan tab and pressing the star on the left. Keep the defaults (make sure your Target is selected) and change the name of the scan.
	4. Click the green arrow to run the scan. It will show up as requested and begin running after about 30 seconds or so. Be patient as it can take a while to do.
6. Keep reloading the page every so often to check the progress. Eventually, it will say 'Done' and the bar will turn blue. Click on it and take a look at the vulnerability report that shows up. Congrats! You have completed the vulnerability scan!

Sources & Outside Guides:
[Install Docker Engine - Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
[Video from the Docker slideshow](https://utulsa.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=75912983-0806-47a5-a3ad-acc9018aaec3)
[Docker page on the docker run command](https://docs.docker.com/reference/cli/docker/container/run/)
[OpenVas Webpage Tutorial](https://community.greenbone.net/getting-started/configuring-and-running-first-scan/)

Part -1: I cannot stress this enough... Do NOT install Docker Desktop... don't be like me. Here is what I did in terms of it though. 
*Edit: turns out, I did NOT need docker desktop. Lolz*
*Download the latest DEB package. To install it, cd into your Downloads folder and run sudo apt-get install ./docker-desktop-amd64.deb. Be prepared to get an error message at the end about something being unsandboxed as root - this is expected.*
*Before opening the Docker Desktop, you must make sure the CORRECT kvm module is installed. I have the wrong one installed on my VMs apparently and it will not allow me to install the correct ones.*
*Altering the settings of the virtual machine to enable Virtualization before opening the machine: Edit Virtual Machine Settings -> Processors -> Virtualize Engine -> Click both checkboxes.*
*Disable Hypervisor on your home machine: Run cmd as Administrator -> bcedit /set hypervisorlaunchtype off*
