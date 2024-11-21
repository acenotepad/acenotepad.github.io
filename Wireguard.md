---
title: Wireguard
filename: Wireguard.md
---

## Project 3: Wireguard VPN via Docker
### DigitalOcean Setup:  

#### 1. Create your DigitalOcean account  

#### 2. Scroll and click on "Host a website or ..." and click on Deploy an Ubuntu Server.  
##### 2.1. Alternatively, scroll to 'Spin Up a Droplet' and follow the same guidelines below.  

#### 3. You will be choosing the data center that is closest to you (for me, this is New York) and the $6/month plan. Then, add either an SSH key or a password (I chose a password). You are free to install other add-ons, but they will not be pertinent to the rest of the project.  
![Droplet](https://github.com/user-attachments/assets/00e97e14-58dd-42f6-b112-fbf3448046b4)  


### Installing Docker:
#### 4. Pre-Installation packages: **sudo apt install curl apt-transport-https ca-certificates software-properties-common**  
![Pre_Install_Packages](https://github.com/user-attachments/assets/e20f7ab6-a0be-414c-87f8-7a6e9712d733)  

#### 5. Download the Docker GPG: **curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg**  

#### 6. Create docker.list repository in /etc/apt/sources.list.d: **echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null**  
![LongCommands](https://github.com/user-attachments/assets/5d1cb81c-5b44-4cc4-93b7-7ab55fd7ee05)  

#### 7. Update the system to ensure it knows there's been a change: **sudo apt update**  

#### 8. Install Docker Community Edition: **sudo apt install docker-ce -y**  

#### 9. Verify Docker service to ensure it is running properly: **sudo systemctl status docker**  
![Docker_Status](https://github.com/user-attachments/assets/35fc6bff-c381-4429-8c42-d50443ef69e0)  

### Installing & Starting Wireguard:
#### 10. Create the directories: 
##### 10.1. **mkdir -p ~/wireguard/**
##### 10.2. **mkdir -p ~/wireguard/config**
#### 11. Create the docker-compose.yml file. You will be altering the Timezone, ServerURL to your URL address, Peers (names will follow naming pattern)
##### 11.1. **nano ~/wireguard/docker-compose.yml**
##### 11.2. See this file below! 
![docker-compose-yml-file](https://github.com/user-attachments/assets/255a3b44-3cdc-4914-8370-4ff8314cc8a5)  

##### 11.3. Save the file and exit.
#### 12. Start Wireguard by cd-ing into the wireguard directory (created in step 13) and running **docker compose up -d**
![Start_Wireguard](https://github.com/user-attachments/assets/f20a9722-01be-490c-95f5-1b13422d163d)  


### Using Wireguard:
#### Run *docker-compose logs -f wireguard* to see the logs and QR codes of Wireguard. 
![QR_Start](https://github.com/user-attachments/assets/f1c977cd-de5f-4083-939e-c9ff773538cd)  

#### 13A. On a mobile device:
##### A1. Download the wireguard app. Once you go into the app, tap on the plus sign and choose 'Create from a QR code' and scan the QR Code that appeared in your terminal
##### A2. Go to ipleak.net and see your current IP really fast. Screenshot this.  
![Utulsa_Phone_IP](https://github.com/user-attachments/assets/6fed598b-7c1c-4f79-a9b3-40b6c87974d5)  

##### A3. Back to the Wireguard app! Turn on the VPN and screenshot it running.  
![Wireguard_Phone](https://github.com/user-attachments/assets/bc6e3a7b-4ead-407b-8a1d-28bfac1569bf)  

##### A4. Back to ipleak.net! Reload the page and note the new IP address. You are now on your own VPN!  
![VPN_Phone_IP](https://github.com/user-attachments/assets/6d3e4dd5-1528-4068-b8ce-a414ad5707fe)  


#### 13B. On a laptop:
##### B1. Download the Wireguard application and install it. In the bottom left corner, press 'Add Terminal'. 
##### B2. Search for your .conf file that you created in the wireguard directory by running wireguard. Since I used the console instead of opening a VM to run everything on, I copied the contents of the file to a text document and saved it on my desktop as a .conf file and it worked just fine.
##### B3. Go to ipleak.net and take a screenshot of your current IP.
![Utulsa_Laptop_IP](https://github.com/user-attachments/assets/cdb22c03-4c78-41bd-a9ca-5046f7b3c1c0)  

##### B4. Press 'Activate' in the Wireguard application - when the light turns green, your VPN has been activated! Screenshot this and reload the ipleak.net page. 
![Wireguard_Laptop](https://github.com/user-attachments/assets/d9ba3535-cc5e-4b90-94a1-4c7bc0f8bd5b)  


##### B5. Take another screenshot with the new IP. 
![VPN_Laptop_IP](https://github.com/user-attachments/assets/fa278423-e67d-45b8-97e4-2f0fa3d73e58)  

## Sources:  

[Blog: Install Docker on 24.04 Ubuntu](https://www.cherryservers.com/blog/install-docker-ubuntu)  

[The Matrix: VPN Server w/ Docker](https://thematrix.dev/setup-wireguard-vpn-server-with-docker/)
