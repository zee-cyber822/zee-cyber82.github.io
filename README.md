For this assignment i installed Docker and used Docker Compose to deploy the Uptime Kuma monitoring application on my Ubuntu virtual machine

Uptime Kuma is a monitoring tool that can check whether websites or services are up and how fast they respond

In my setup i used Uptime Kuma to monitor the website Twitch and in this documentation i'm explaining every step I followed, the commands I ran and how I verified that the container and application were working

1- updating the system
First I updated the package list so I could install the latest versions of packages:
sudo apt update

2- installation
Then I installed Docker and Docker Compose from the Ubuntu repositories in one command:
sudo apt install docker.io docker-compose -y

3- enabling docker
I configured Docker to start automatically and to begin running immediately:
sudo systemctl enable --now docker
At other times I also used the separate commands:
sudo systemctl start docker
sudo systemctl enable docker

3- creating the project directory
I checked the status of the Docker service
sudo systemctl status docker
I confirmed that the service status showed as active and running.
I created a folder in my home directory to hold the Uptime Kuma configuration and data:
mkdir uptime-kuma
cd uptime-kuma
<img width="1204" height="734" alt="Screenshot 2025-11-19 at 9 16 50 PM" src="https://github.com/user-attachments/assets/d214edaa-dd9b-4ae2-86d6-0b2e415bc414" />

5- creating the docker file
Inside the uptime-kuma directory I created a docker-compose.yml file using the nano text editor:
nano docker-compose.yml
In the editor I typed the following content
version: "3"
services:
  uptime-kuma:
    image: louislam/uptime-kuma:1
    container_name: uptime-kuma
    volumes:
      - ./uptime-kuma-data:/app/data
    ports:
      - "3001:3001"
    restart: always
    <img width="1165" height="754" alt="Screenshot 2025-11-19 at 9 20 21 PM" src="https://github.com/user-attachments/assets/208af362-1518-4336-b312-38018573d783" />

6- running the container    
after that i verified that the file exists:
ls
I ran:
sudo usermod -aG docker $USER
then I refreshed my group membership without rebooting:
newgrp docker
from inside the ~/uptime-kuma directory i started the container in detached mode:
docker-compose up -d
to make sure the Uptime Kuma container was running correctly i used:
docker ps
<img width="1203" height="735" alt="Screenshot 2025-11-19 at 9 18 22 PM" src="https://github.com/user-attachments/assets/9e5e24d4-d2a4-427b-8285-2773a2d6c66e" />

7- accessing uptime kuma
once the container was running i opened firefox inside the Ubuntu VM and went to:
http://localhost:3001
and it opened on the set up in Uptime kuma where i created an account
<img width="1118" height="738" alt="Screenshot 2025-11-19 at 8 51 15 PM" src="https://github.com/user-attachments/assets/3e81228a-e8ec-46c0-b216-ed325bf777f4" />

8- adding a monitor
from the Uptime Kuma dashboard i set up monitoring for Twitch
Friendly Name:Twitch
URL:https://www.twitch.tv
and the rest i left them as defult
<img width="1279" height="802" alt="Screenshot 2025-11-19 at 9 05 23 PM" src="https://github.com/user-attachments/assets/a61a8b9e-1bdf-4a87-8f16-0c31bcefb598" />

The graph showed response times for Twitch over the last few checks and this confirmed that Uptime Kuma was successfully monitoring https://www.twitch.tv from inside my Docker container.

9- issues i faced and fixes
During this project I ran into a few issues and learned how to fix them:
Using the wrong command syntax
at first I accidentally ran:
sudo docker compose up -d
This produced an error:
unknown shorthand flag: 'd' in -d
The correct command for my installation is:
docker-compose up -d
Docker permission error

Before I added my user to the docker group, docker-compose up -d failed with a PermissionError(13, 'Permission denied') when trying to talk to the Docker API
I fixed this by running:
sudo usermod -aG docker $USER
newgrp docker
After that the docker commands worked without needing sudo each time
These issues helped me better understand how Docker’s service, permissions, and command tools work together
