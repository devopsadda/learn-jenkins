# Learn and Build Pipeline on jenkins

## Create a CentOs Linux Server with Jenkins user (having admin privileges)
```shell
useradd jenkins
passwd jenkins
usermod -aG wheel jenkins
```

## Install Docker and Docker Compose on the CentOS Server
```shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl start docker
sudo systemctl enable docker
docker ps
sudo usermod -aG docker jenkins
docker ps

curl -SL https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose
docker pull jenkins:jenkins
docker pull jenkins/jenkins
docker info | grep -i root
sudo du -sh /var/lib/docker/
```

## Write a Docker Compose file for running the Jenkins as a container.
```shell
mdkri jenkins-data
cd jenkins-data
vim docker-compose.yml

version: '3.8'
services:
  jenkins:
    container_name: jenkins
    image: jenkins/jenkins:lts
    privileged: true
    user: root
    ports:
      - 8080:8080
      - 50000:50000
    volumes:
      - $PWD/jenkins_home:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
    networks:
      - net
networks:
  net:

docker-compose up -d
docker ps
docker logs jenkins
sudo cat /var/jenkins_home/secrets/initialAdminPassword
```


