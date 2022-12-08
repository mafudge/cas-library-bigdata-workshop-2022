# CAS Library Workshop: Open Source Big Data Systems

Welcome to the git repository for the Chinese Academy of Sciences Library Workshop on Open Source Systems for Big Data. This repository contains the slides, sample code, and virtualized environment so that you can setup and use each of the database systems discussed in the lecture. 

- Slides can be found in the `slides` directory.
- Sample code can be found in the `work` directory.
- Datasets used in the examples are in the `datasets` directory.
- The `docker-compose.yaml` file contains the configuration for the virtualized environment.
- Here is a link to the video from the workshop that demonstrates how to use the virtualized environment: TODO-Add-Url

## Technology Stack Required to Run The Big Data Systems In This Workshop

### Hardware Requirements

- A computer with an Intel-compatible CPU capable of hardware virtualization (VT-x or AMD-V)
- 8GB RAM (16GB recommended)
- 40GB free space for docker images and the datasets

### Software Requirements

- Windows 10 Pro, Enterprise, or Education (64-bit) or Ubuntu Linux 18.04 or later. I am sure other Linux distributions will work, but I have not tested them.
- Docker Desktop for Windows or Docker Engine for Linux https://get-docker.com 
- git SCM for cloning this code to your computer https://git-scm.com/downloads
- A Web Browser (Firefox recommended) https://www.mozilla.org/firefox/

### Setup Instructions for Ubuntu Linux

Make sure you have the latest updates:
`$ sudo apt-get update`

Install git, docker and firefox:
`$ sudo apt-get install -y git docker.io firefox`

Add your user to the docker group so you can run docker commands without sudo:
`$ sudo usermod -aG docker $USER`

**NOTE**: Log out and log back in to pick up the new docker group permissions.

Test docker
`$ docker run hello-world`

If this works you should see a message like this:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

Clone this git repository:
`$ git clone https://github.com/mafudge/cas_library_bigdata_workshop_2022.git`

Change to the directory containing the git repository:
`$ cd cas_library_bigdata_workshop_2022`

There's one-time prep you must do after cloning the git repository.  Have dinner or go for a walk, as it can take some time to clone the images.

Download the images:
`$ docker-compose pull`  

Create the containers from the images:
`$ docker-compose create`

At this point you are ready to use the virtualized environment. See the section below for common tasks.

## Common Tasks

### Starting Services 

Use `docker-compose start <services>` to start the services you need. As a best practice, start only the services you need. The required services are listed in the problem set / lab instruction document.    
For example, to start the `jupyter`, `drill` and `minio` services for the minio lab:
`$ docker-compose start jupyter drill minio`

### Stopping Services

Just like turning off the lights when you leave a room, when you are finished using the services, stop them:. This will free up resources on your computer.  
`$ docker-compose stop`  

You can also stop individual services:  
`$ docker-compose stop drill minio`

### Listing all available services

Can remember the names of all these services? Use this command to list all services in the `docker-compose.yml` file:  
`$ docker-compose config --services`

### Which services are running?

Need to know if a service is running? Or which port the service is available on? Try this:   

`$ docker-compose ps`   

The command will display which services are running and ports exposed to your host.

### Tips regarding ports

-Each database service uses its well known TCP port. For example Microsoft SQL Server is TCP/1433, Minio is TCP/9000.   
- Sample applications, such as `retwis` and `mongoapp` use ports in the 5xxx range.
- Web interfaces to databases like `jupyter`, `rediscommander` or `mongoexpress` use ports in the 8xxx range.

### Checking the container logs

When things go wrong with a service (and you can count on that happening), you will need to check the container logs. For example to view the logs for the `jupyter` service:   

`$ docker-compose logs jupyter`

Searching the web for the error in the log usually gives you an indication as to what is going awry.


### Login Credentials for Services

Check the `docker-compose.yaml` file for the credentials for each service.


### Updating the git repository.

At times you may need to update the git repository. For example, if I fix an issue with this setup. To get the latest updates:

`$ git pull`