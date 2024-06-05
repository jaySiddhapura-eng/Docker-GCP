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

## Docker Engine installation and commands

### GCP VM Instance
-   For this tutorial we will use GCP VM instance as our machine, on this machine we will install the docker engine and try different commands
-   Prerequisit : Create the VM instance on GCP 