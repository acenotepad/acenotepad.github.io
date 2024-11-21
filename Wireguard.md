---
title: Wireguard
filename: Wireguard.md
---

## Project 3
### DigitalOcean Setup:  

#### 1. Create your DigitalOcean account  

#### 2. Scroll and click on "Host a website or ..." and click on Deploy an Ubuntu Server.  
##### 1. Alternatively, scroll to 'Spin Up a Droplet' and follow the same guidelines below.  

#### 3. You will be choosing the data center that is closest to you (for me, this is New York) and the $6/month plan. Then, add either an SSH key or a password (I chose a password). You are free to install other add-ons, but they will not be pertinent to the rest of the project.  

### Installing Docker:
#### 4. Pre-Installation packages: **sudo apt install curl apt-transport-https ca-certificates software-properties-common**  

#### 5. Download the Docker GPG: **curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg**  

#### 6. Create docker.list repository in /etc/apt/sources.list.d: **echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null**  

#### 7. Update the system to ensure it knows there's been a change: **sudo apt update**  

#### 8. Install Docker Community Edition: **sudo apt install docker-ce -y**  

#### 9. Verify Docker service to ensure it is running properly: **sudo systemctl status docker**  

### Installing & Starting Wireguard:
#### 10. Create the directories: 
##### 10.1. **mkdir -p ~/wireguard/**
##### 10.2. **mkdir -p ~/wireguard/config**
#### 11. Create the docker-compose.yml file
##### 11.1. **nano ~/wireguard/docker-compose.yml**
##### 11.2. See this file below! 
		14.2.1. Alter: Timezone, ServerURL to your URL address, Peers (names will follow naming pattern)
##### 11.3. Save the file and exit.
#### 12. Start Wireguard by cd-ing into the wireguard directory (created in step 13) and running **docker compose up -d**

### Using Wireguard:
#### 13A. On a mobile device:
##### A1. Download the wireguard app. Once you go into the app, tap on the plus sign and choose 'Create from a QR code' and scan the QR Code that appeared in your terminal
##### A2. Go to ipleak.net and see your current IP really fast. Screenshot this.
##### A3. Back to the Wireguard app! Turn on the VPN and screenshot it running.
##### A4. Back to ipleak.net! Reload the page and note the new IP address. You are now on your own VPN!
#### 13B. On a laptop:
##### B1. Download the Wireguard application and install it. In the bottom left corner, press 'Add Terminal'. 
##### B2. Search for your .conf file that you created in the wireguard directory by running wireguard. 
	1. Since I used the console instead of opening a VM to run everything on, I copied the contents of the file to a text document and saved it on my desktop as a .conf file and it worked just fine.
##### B3. Press 'Activate' - when the light turns green, your VPN has been activated! Reload the ipleak.net page and take another screenshot with the new IP. 

## Sources:  

[Blog: Install Docker on 24.04 Ubuntu](https://www.cherryservers.com/blog/install-docker-ubuntu)  

[The Matrix: VPN Server w/ Docker](https://thematrix.dev/setup-wireguard-vpn-server-with-docker/)
