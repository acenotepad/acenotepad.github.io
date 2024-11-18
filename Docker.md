---
title: Docker
filename: Docker.md
---

## This is a guide to install Docker Desktop on Ubuntu Linux
#### A few things to note:
##### All commands will be bolded for the sake of clarity
##### Negative parts are where I ran into problems and was doing the WRONG things. I documented them at the bottom of the page after my sources so that they are not seen except 

### Part 1: Setting up the apt repository:  

   #### 1. Run: **sudo apt-get update** and **sudo apt-get upgrade** (updates and upgrades everything)  
   
   #### 2. Run: **sudo apt-get install ca-certificates curl** (installs ca-certificates and curl)  
   
   #### 3. Run: **sudo install -m 0755 -d /etc/apt/keyrings** (installs in permisison mode and treats all arguments as directories). There was no output when I ran this command.  
   
   #### 4. Run: **sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc** (runs curl on the link and outputs the result to the filepath starting at /etc listed). There was no output when I ran this command.  
   
#### 5. Run: **sudo chmod a+r /etc/apt/keyrings/docker.asc** (assigns user, group, and others the read permission for the file with the part starting at /etc). There was no output when I ran this command.  
![0BundleCommand](https://github.com/user-attachments/assets/d51f2bc0-aee0-4b83-a95d-33648d36977f)  

#### 6. Run: **echo \ "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \ $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \ sudo tee /etc/apt/sources.list.d/docker.list > /dev/null**. There was no output when I ran this command.  


### Part 2: Installing Docker Engine!  

#### 1. Install the latest Docker packages (as of November 15th, 2024) by running **sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin** (installs all of the following packages listed)  

##### 1.1. This process installs packages: docker-ce-rootless-extras git git-man liberror-perl libslirp0 pigz; slirp4netns  

##### 1.2. This process installs NEW packages:  containerd.io docker-buildx-plugin docker-ce docker-ce-cli; docker-ce-rootless-extras docker-compose-plugin git git-man liberror-perl; libslirp0 pigz; slirp4netns  

#### 2. To test the install, run **sudo docker run hello-world**  

##### 2.1. This runs a 'Welcome to Docker' message! If your output matches the screenshot below, congrats! You've installed Docker properly!  

![0HelloWorld](https://github.com/user-attachments/assets/81a9d040-f3e5-4c72-9952-02ec782c441a)  

#### 3. Docker Compose should have been installed either with the prior Docker install or the Docker Engine install. If you are able to run docker compose version and get a version number, you have Docker Compose ready to go!


### Part 3: The OpenVas Container and the Security Scan
#### 1. Run **sudo apt-get update** and **sudo apt-get upgrade** to make sure your software is up to date before beginning installation  
#### 2. **sudo docker run -d -p 443:443 --name openvas mikesplain/openvas**  
##### 2.1. -d runs the docker container in the background - it does not find it locally, so it installs it to run  
##### 2.2. -p publishes the container to the host  
##### 2.3 The rest of the command names the new container 'openvas' and uses the image from mikesplain/openvas as the base for the container.  
#### 3. I created an directory titled 'openvasdocker' and made the docker-compose.yml file by downloading the yml file from the github repo to it.  
![0DockerComposeFile](https://github.com/user-attachments/assets/e93b673e-ca21-4cc0-bc5d-81c93a98a367)  
#### 4. After this is done TAKE A SNAPSHOT OF YOUR MACHINE. If you mess up the next part, you will want to have a snapshot to revert to so you do not lose your progress entirely.  
#### 5. Running the Vulnerability Scan  
##### 5.1. Go to https://localhost/ and login to the site with credentials admin/admin  
##### 5.2. Create a task by clicking on Tasks under the Configuration tab and pressing the star. Keep the defaults, but change the IP to one of your IPs.  
![Target](https://github.com/user-attachments/assets/07a7ee40-92e5-4644-8550-58bd5a7afdea)  
##### 5.3. Create the scan by clicking on Tasks under the Scan tab and pressing the star on the left. Keep the defaults (make sure your Target is selected) and change the name of the scan.  
![MenaceScan](https://github.com/user-attachments/assets/915752be-285c-4a50-98ea-d839cdfb8ec9)  
##### 5.4. Click the green arrow to run the scan. It will show up as requested and begin running after about 30 seconds or so. Be patient as it can take a while to do.  
#### 6. Keep reloading the page every so often to check the progress. Eventually, it will say 'Done' and the bar will turn blue. Click on it and take a look at the vulnerability report that shows up. Congrats! You have completed the vulnerability scan!  
![CompletedScan](https://github.com/user-attachments/assets/54d49779-dca9-4c68-b531-95d449d3aaf3)  


### Sources & Outside Guides:
#### [Install Docker Engine - Ubuntu](https://docs.docker.com/engine/install/ubuntu/)  
#### [Video from the Docker slideshow](https://utulsa.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=75912983-0806-47a5-a3ad-acc9018aaec3)  
#### [Docker page on the docker run command](https://docs.docker.com/reference/cli/docker/container/run/)  
#### [OpenVas Webpage Tutorial](https://community.greenbone.net/getting-started/configuring-and-running-first-scan/)  



### Part -1: I cannot stress this enough... Do NOT install Docker Desktop... don't be like me. Here is what I did in terms of it though. 
#### *Edit: turns out, I did NOT need docker desktop. Lolz*  
#### *Download the latest DEB package. To install it, cd into your Downloads folder and run sudo apt-get install ./docker-desktop-amd64.deb. Be prepared to get an error message at the end about something being unsandboxed as root - this is expected.*  
#### *Before opening the Docker Desktop, you must make sure the CORRECT kvm module is installed. I have the wrong one installed on my VMs apparently and it will not allow me to install the correct ones.*  
##### *Altering the settings of the virtual machine to enable Virtualization before opening the machine: Edit Virtual Machine Settings -> Processors -> Virtualize Engine -> Click both checkboxes.*  
##### *Disable Hypervisor on your home machine: Run cmd as Administrator -> bcedit /set hypervisorlaunchtype off*  
