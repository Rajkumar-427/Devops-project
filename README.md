This is used to deploy Django Blog application in AWS EC2 instance
https://www.jenkins.io/doc/book/installing/linux/#debianubuntu

# Docker installation
https://docs.docker.com/engine/install/ubuntu/
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
sudo chown root:docker /var/run/docker.sock
sudo chmod 660 /var/run/docker.sock
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
