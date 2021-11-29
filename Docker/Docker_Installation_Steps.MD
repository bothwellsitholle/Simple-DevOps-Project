# Installing Docker on Amazon Linux server

### Pre-requisites
1. Amazon Linux EC2 Instance

## Installation Steps

1. Install docker and start docker services
   ```sh 
   yum install docker -y
   docker --version 
   
   # start docker services
   service docker start
   service docker status
   ```
1. Create a user called dockeradmin
   ```sh
   useradd dockeradmin
   passwd dockeradmin
   ```
1. add a user to docker group to manage docker 
   ```
   usermod -aG docker dockeradmin
   ```
### Validation test
1. Create a tomcat docker container by pulling a docker image from the public docker registry
   ```sh
   docker run -d --name test-tomcat-server -p 8090:8080 tomcat:latest
   ```

## Docker Installation on CentOS server
##### Referance URL : https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-repository
### Pre-requisites

Please follow below steps to install docker CE on CentoOS server instance. For RedHat only Docker EE available 

1. Install the required packages.

   ```sh 
   sudo yum install -y yum-utils \
   device-mapper-persistent-data \
   lvm2
   ```
  
1. Use the following command to set up the stable repository.
 
   ```sh 
   sudo yum-config-manager \
   --add-repo \
   https://download.docker.com/linux/centos/docker-ce.repo
   ```

### INSTALLING DOCKER CE

1. Install the latest version of Docker CE.
   ```sh 
   sudo yum install docker-ce
   ```

   Note: If prompted to accept the GPG key, verify that the fingerprint matches 
060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35, and if so, accept it.

1. Start Docker.
   ```sh 
   sudo systemctl start docker
   ```

1. Verify that docker is installed correctly by running the hello-world image.
   ```sh
   sudo docker run hello-world
   
   
   
  2. DOCKR COMMANDS
   
   - docker ps -a

   - docker image ls

   - docker pull tomcat. / docker pull tomcat:latest

   - docker run --name tomcat-container -p 8080:8080 tomcat:latest
                     Or
   - docker run -d --name tomcat-container -p 8080:8080 tomcat:latest

   - docker rm 9115cfe4fdbd

   - docker exec -it tomcat-container /bin/bash

   
   If you encount issue login in the docker container
   
   - install a lower version of tomcat image eh tomcat:8.0 or do the following:
   
   docker run -d --name tomcat-container -p 8080:8080 tomcat:latest
   
   copy content in webapps.dist to webapps: webapps by default is empty an thats where our app should run from.
   
   - hAdd new port 8081. 
   copy content in webapps.dist to webapps: webapps by default is empty an thats where our app should run from.under security group      in EC2 instance
   
   docker run -d --name tomcat-8 -p 8081:8080 tomcat:8.0
   
   
   
   3. INTERGATING DOCKER IN JENKINS.

   - install a plugin in jenkins called "publish over ssh"
   - Create user as above
   - Go to jenkins configurations under publish over ssh add your hostname and password and test connections
   - Under post post build add and select send build artifacts over ssh and add this path webapp/target/*.war to sorce target.
   - edit /etc/ssh/sshd_config to allow PasswordBasedAuthentication and reload by using this command  service sshd reload
   
   ```
