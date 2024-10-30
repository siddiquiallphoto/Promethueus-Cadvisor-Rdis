Process of doing it: 28-10-24

Create an instance, 
Install docker and docker compose. 
Sudo apt-get update
sudo apt-get install docker.io docker-compose
docker ps
(you will see pemission is denied, now you need to change user permission or append user to group)
sudo usermod -aG docker $USER
sudo systemctl restart docker
sudo reboot
sudo systemctl enable docker
2. 
Install docker compose: 
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

1.	Make it executable:
bash
sudo chmod +x /usr/local/bin/docker-compose
After this, try running docker-compose --version to confirm it's installed correctly. 
docker compose --version

3. make directory with the name of Prometheus and go into it to keep you all the tools install into it.
 First thing you to do is download promethues config file or create one if you need to. 
get prometheus config file.  (chatgpt for test purpose)
wget https://raw.githubusercontent.com/prometheus/prometheus/main/documentation/examples/prometheus.yml

Then
create a docker compose inside it. 
Go to cd promethueus
Then go to Prometheus  file, use vim file to create a docker compose.
vim docker-compose.yml
And now add your docker compose file what actually you want to do it.
version: '3.2'
services:

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - 9090:9090
    command:
      - --config.file=/etc/prometheus/prometheus.yml

    volumes:
      - /home/ubuntu/prometheus/configs/prometheus.yml:/etc/prometheus/prometheus.yml


  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    container_name: cadvisor
    ports:
      - 8080:8080
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:rw
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
    depends_on:
      - redis

  redis:
    image: redis:latest
    container_name: redis
    ports:
      - 6379:6379


docker-compose up -d
 

![image](https://github.com/user-attachments/assets/d883c6e7-0b33-4b1b-b3b2-7c8aafb33780)

![image](https://github.com/user-attachments/assets/0004595d-b918-47bd-9720-85017533a60e)

 
Now next task is to run a docker image on your server.

