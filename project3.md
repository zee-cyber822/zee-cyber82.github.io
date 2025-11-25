1-updating the system :
After accessing my digital ocean droplet and the console, the first step I did was update the system using these two commands:
apt update && apt update -y
They insured all packages were up-to-date before installing docker or wire guard

2-installing :
I installed docker using this command:
apt install docker.io -y 
After installation, I confirmed docker was running by observing the system output, and service restarts

3-Creating a directory for wire guard:
And then created a directory for the VPN set up using this command:
mkdir wireguard
And then I navigated into this folder:
cd wireguard 

4-Creating docker :
Inside the wire guard folder I created the docker compose file using the nano command
nano docker-compose.yml
And then I entered the following configuration into the file:
version: '3.8'
services: wireguard:
image: linuxserver/wireguard
container_name: wireguajd
cap_add:
- NET_ADMIN
- SYS-MODULE
environment:
-   PUID=1000
-   PGID=1000
-   TZ=UTC
-   SERVERURL=143.198.58.67
-   SERVERPORT=51820
-   PEERS=1
-   PEERDNS=auto
-   INTERNAL SUBNET=10.13.13.0
    volumes:
-    •/config:/config
-   /lib/modules:/lib/modules
  ports:
-   51820:51820/udp
    sysctls:
-   net.ipv4. conf.all.sro_valid_mark=1
restart: unless-stopped
I saved and closed the file

5-Installing docker compose:
Docker compose was not installed by default so I installed it manually using this command:
apt install docker-compose -y

6-Running wire guard with docker compose:
Once docker compose was installed, I started the wire guard container 
docker-compose up -d
The server then pulled the Linux server wire guard image created the network and launched the container successfully

7-Verifying configuration files:
To check that my files are inside the wire guard. I used this command to check if they all exist in the right place
ls /wireguard/config
And then I made sure that I was in the right folder with this command
pwd
And then ls command to see my list
And then listed the config files
And then I looked specifically inside the peer1 folder with this command
ls /root/wireguard/config/peer1
And then I installed the wire guard app on my phone and on my PC

8-Generating the QR code for my phone:
After that, I used this command to get a QR code so I can use it for the app on my phone
docker exec -it wireguard /app/show-peer 1
This displayed the full wire guard, QR code, which I scanned using the wire guard mobile app
After that, I connected my phone and also my PC and took screenshots where I put them here in a file of the IP address before and after for both my phone and my PC

9-Destroying the droplet :
After finishing all the configuration, I went back to my digital ocean dashboard to delete the droplet.
I selected my droplet and clicked on destroy on the side bar and digital ocean required me to manually type the droplet name to confirm the destroying step and once it was confirmed the droplet was permanently deleted
