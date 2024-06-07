# Docker

## Content

-   What is Docker?
-   Docker terminologies
-   Docker installation
-   Docker Commands
-   Debugging a container
-   Docker in practice
-   Developing with containers
-   Docker Compose - will it be in cloud run ?
-   Dockerfile - custom docker image
-   Docker repository - Google Container Registry
-   Deploy the containerized app on cloud run 
-   cloud run tutorial 

## What is Docker?

Docker is an open-source platform designed to automate the deployment, scaling, and management of applications using containerization. 

Containers allow developers to package an application along with its dependencies and configuration files into a single, portable unit that can run consistently across different environments.

## Docker terminologies

### Containers
Containers are lightweight, stand-alone, and executable software packages that include everything needed to run a piece of software, including the code, runtime, libraries, and system tools. They are isolated from each other and the host system, ensuring that the application behaves the same regardless of where it is run.

### Docker Engine
The core of Docker, Docker Engine is the runtime that builds and runs containers. It is responsible for creating, running, and managing Docker containers on a host system.

### Docker Hub [Container Repository]
A cloud-based registry service that allows you to find and share container images with your team. You can use Docker Hub to host your own container images and make them available to others. It can be private too, if needed.

### Docker Compose
A tool for defining and running multi-container Docker applications. With Compose, you can use a YAML file to configure your application’s services and create and start all the services from that configuration with a single command.

## Difference between Docker container and Docker image
### Docker Image 
-   In simple term it is a blueprint for creating Docker Container
-   It is a read-only template that includes everything needed to run a piece of software, such as code, runtime, libraries etc
-   it is static in nature, once created, it does not change during the run cycle
### Docker Container
-   It is a running environment/instance of a Docker Image
-   It has all the necessary stuff to run a software, such as file system, environment configuration etc.
-   It has port binded to it, which facilitate external communication to the application running inside the container


## Docker Engine installation

### GCP VM Instance
-   For this tutorial we will use GCP VM instance as our machine, on this machine we will install the docker engine and try different commands
-   Prerequisit : Create the VM instance on GCP, ssh into the instance
-   Run the following command to uninstall all conflicting packages [ignore if docker never installed on machine]
```sh
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
-   set up docker apt repository
```sh
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
-   Install docker packages
```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
-   verify docker installation : This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.
```sh
sudo docker run hello-world
```

## Docker commands
-   Pulling the image from dockerhub repository [eg. redis]
-   Dockerhub : https://hub.docker.com/
```sh
sudo docker pull redis
```
-   To check exisisting images on your machine 
```sh
sudo docker images
```
-   To remove the docker image from the machine
```sh
sudo docker rm [stopped container id]
```
```sh
sudo docker rmi [IMAGE ID]
```
-   To run the image in a container [run it in new terminal window]
```sh
sudo docker run redis
```
-   To see running containers
```sh
sudo docker ps
```
-   Output of above command
```sh
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS      NAMES
533f1079b098   redis     "docker-entrypoint.s…"   6 seconds ago   Up 4 seconds   6379/tcp   exciting_ride
```
-   to run the container in detached mode
```sh
sudo docker run -d redis
```
-   to stop/start the container
```sh
sudo docker stop [CONTAINER ID]
sudo docker start [CONTAINER ID]
```

## Container ports and Port bindings
-   What happen when I run two redis containers using:  ```docker run redis``` twice
```sh
:~$ sudo docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS      NAMES
3106721e4fd9   redis     "docker-entrypoint.s…"   4 seconds ago    Up 4 seconds    6379/tcp   trusting_tu
533f1079b098   redis     "docker-entrypoint.s…"   18 minutes ago   Up 18 minutes   6379/tcp   exciting_ride
```
-   Here you can see the ports on which redis is listening is same : ```6379/tcp``` to avoid conflict between two port listening to the same port, the host port needed to be bind correctly, so that requests can be diverted to the appropreate container. [stop all the running container for next step]

- specifying port binding during start of the container ```d``` for detached mode, ```p1234``` host port, ```6379``` is a port on which redis container is listening [check result of first point of this section]
```sh
sudo docker run -d -p1234:6379 redis
```
-   In following snippet you can see the ports section
```sh
sudo docker ps
CONTAINER ID  IMAGE  COMMAND                 CREATED        STATUS        PORTS                                       NAMES
c00ce3c0d75b  redis  "docker-entrypoint.s…"  2 minutes ago  Up 2 minutes  0.0.0.0:1234->6379/tcp, :::1234->6379/tcp   priceless
```
-   Now run another redis container as follow 
```sh
sudo docker run -d -p9999:6379 redis
```
-   Now I have two redis container running and listening to the two different host port, which were specify when container were started
```sh
sudo docker ps

CONTAINER ID   IMAGE  COMMAND                  CREATED         STATUS         PORTS                                       NAMES
e665ded6ab67   redis  "docker-entrypoint.s…"   8 seconds ago   Up 7 seconds   0.0.0.0:9999->6379/tcp, :::9999->6379/tcp   wizardly
c00ce3c0d75b   redis  "docker-entrypoint.s…"   8 minutes ago   Up 8 minutes   0.0.0.0:1234->6379/tcp, :::1234->6379/tcp   engelbar
```

## Debugging a container
-   To get the logs of perticular container
```sh
docker logs [CONTAINER ID] 
or
docker logs [container name]
```
-   To provide custom container name
```sh
sudo docker run --name [customName nameOfImage]
```
-   Get the terminal of particular container [accessing the container]
```sh
sudo docker exec -it [CONTAINER ID] /bin/bash 
or
sudo docker exec -it [CONTAINER Name] /bin/bash
```
-   ```exit``` to exit the terminal
-   ```docker run``` : create a new container using the image
-   ```docker start``` : start the already existing/created container 

## Docker compose vs. Docker File

### Docker File

image
 ![dfile](./assets/dockerfile.jpg)

### Docker compose 

![compose](./assets/dockerCompose.png)

## Cloud store = GCP Docker engine 

    ![PATH](./assets/Screenshot%202024-06-06%20154228.png)
-   
