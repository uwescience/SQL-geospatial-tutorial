---

title: "Introduction to Docker"
teaching: 15
exercises: 0
questions:
- "How to use Docker?"
objectives:
- "Use Docker Hub to Find Docker Images"
- "Run a Docker Container"
- "Use Jupyter Notebooks with a Docker Container"
- "Delete Docker Containers and Images"
- "Link Data Volumes"
- "Create a Docker Image"
keypoints:
- "Docker images can be used instead of installing software natively on your computer"

---


### Install Docker
These instructions assume that Docker has already been installed on your computer.  If Docker has not been installed, follow these [instructions](https://geohackweek.github.io/preliminary/01-install-docker).
<br> <br>
   
### Find Docker Images
Existing docker images are available on [Docker Hub](https://hub.docker.com/).

![Docker Hub](https://raw.githubusercontent.com/geohackweek/Introductory/gh-pages/assets/img/dockertutorial/DockerHub1.png)


Enter "geohackweek" in the search box:

![Docker Hub](https://raw.githubusercontent.com/geohackweek/Introductory/gh-pages/assets/img/dockertutorial/DockerHub2.png)


You will see a list of repositories, click on geohackweek2016/vectortutorial:

![Docker Hub](https://raw.githubusercontent.com/geohackweek/Introductory/gh-pages/assets/img/dockertutorial/DockerHub3.png)


To obtain the docker image, copy the Docker Pull Command to use in your terminal:

![Docker Hub](https://raw.githubusercontent.com/geohackweek/Introductory/gh-pages/assets/img/dockertutorial/DockerHub4.png)
<br> <br>
  
### Docker Hub Resources for Geospatial Research
[geohackweek2016](https://hub.docker.com/u/geohackweek2016/)   
[Google Earth Engine](https://hub.docker.com/u/tylere/)   
[Anaconda](https://hub.docker.com/u/continuumio/) - The Anaconda Python Distribution.   
[Rocker](https://hub.docker.com/u/rocker/) - R and R Studio.
<br> <br>
 
### Download Docker Images
The `docker pull` command gets the latest version of the docker image from the Docker Hub.   

```bash
$ docker pull geohackweek2016/arraystutorial
```
<br> 
 
### View Docker Images
The `docker images` command shows you all the docker images that you have available on your local machine.   

```bash
$ docker images
```
<br> 
  
### Run Docker Containers
The `docker run` command starts a new **docker container** using a **docker image**.

A **docker image** is a filesystem and parameters to use at runtime. It doesn’t have state and never changes. A **docker container** is a running instance of an image.

**Mac OSX and Linux** 
    
```bash
$ docker run -i -t --name my_container geohackweek2016/arraystutorial
```

**Windows**   

```bash
$ docker run -it --name my_container geohackweek2016/arraystutorial
```

The '-i' flag specifies that you want to run the docker container interactively. The '-t' flag specifies that you want run a pseudoterminal when the container is started.  The '--name' flag specifies a name chosen by you for the container.  If you do not use the '--name' flag, then a name will be automatically assigned to the container.

<br>
Type `exit` on the command line to leave the docker container.   

```bash
root:/# exit
```
<br> 
  
### Start Docker Containers
To view existing docker containers, type `docker ps -a`

```bash
$ docker ps -a
```
The '-a' flag specifies that you want to see all the containers.

<br>
To start an existing docker container named 'my_container':   

**Mac OS X and Linux**

```bash
$ docker start -a -i my_container
```

**Windows**

```bash
$ docker start -ai my_container
```
The '-a' flag specifies that you want to attach the container and the '-i' flag specifies that you want to run the docker container interactively.

<br>
To rename a docker container:   

```bash
$ docker rename my_container better_name
$ docker ps -a
```
<br>  
  
### Docker Containers Work with Jupyter Notebooks
The following command will start a new container with the ports open for jupyter notebooks.   

**Mac OS X and Linux**  
 
```bash
$ docker run -i -t -p 8888:8888 --name jupyter_container geohackweek2016/arraystutorial
```

**Windows**   

```bash
$ docker run -itp 8888:8888 --name jupyter_container geohackweek2016/arraystutorial
```
The '-p 8888:8888' links the port 8888 on your local filesystem to the port 8888 in the container.

<br>
Start a jupyter notebook from the command line of the container.

```bash
root:/#  jupyter notebook --notebook-dir=/notebooks --ip='*' --port=8888 --no-browser
```
The '--notebook-dir' flag specifies the folder in the container where the jupyter notebooks are saved.  The '--ip' flag specifies that the port is open for all ip addresses.  The '--port' specifies the port for the browser to find the jupyter notebooks.  The port '8888' must match the ports in the '-p 8888:8888' flag in the `docker run` command.

<br>
**View the jupyter notebooks by opening an internet browser on your local filesystem and entering the address as http://localhost:8888**
<br>  <br>
 
### Deleting Docker Containers and Images
To remove a single docker container:   

```bash
$ docker rm my_container
```

<br>
To remove all docker containers:   

**Mac OS X and Linux**   

```bash
$ docker rm $(docker ps -a -q)
```

**Windows**   

```bash
$ docker rm $(docker ps -aq)
```
'$(docker ps -a -q)' lists all the container ids which are then removed by the `docker rm` comand.

<br> 
To remove a single docker image:   

```bash
$ docker rmi -f geohackweek2016/arraystutorial
```
The '-f' flag means force the removal of the image which is necessary if an existing docker container was based on the image.  The docker container still functions after the docker image is removed.

<br>
<hr style="border-color: black; border-width: 2">   

### Linking Data Volumes

Data for the tutorials have been included in the docker images for convenience.  However, you may want to store your data on your local filesystem particularly if the data files are large.  Data outside the container can be linked to the container using the following commmand:

**Mac OS X and Linux**   

```bash
$ docker run -i -t -v /Users/Home/Data/:/data --name my_container geohackweek2016/arraystutorial 
```
"/Users/Home/Data/" is the filepath to the data directory on your local filesystem. "/data" is the filepath to the folder in your container where the data will be linked. Note that any data in the "/data" folder will be replaced by the data from your local filesystem.  The '-v' flag specifies that you are linking a data volume.  **Any changes to the data made in the container will be propogated to the data on your local filesystem.** 
   

### Copy from a Container to your Local Filesystem.   

```bash
$ docker cp my_container:/file/path/within/container /host/path/target
```
<br>  

### Create a Docker Image for Your Project
Docker images are built based on on commands contained in a [Dockerfile](https://github.com/geohackweek/nDarrays/blob/gh-pages/docker/Dockerfile).

This is the contents of the Dockerfile for the nDarrays tutorial:

```
FROM continuumio/anaconda3
RUN apt-get install -y tar git curl nano wget dialog net-tools build-essential
RUN apt-get -y install pyqt4-dev-tools
RUN mkdir /data
RUN conda install matplotlib
RUN conda install pandas
RUN conda install numpy
RUN conda install xarray
RUN conda install dask
RUN conda install netcdf4
RUN conda install jupyter
RUN mkdir /notebooks
COPY /data/ /data
```

This Dockerfile uses the anaconda python 3.x distribution from continuumio [available on Docker Hub](https://hub.docker.com/r/continuumio/anaconda3/) as a base.  Some additional packages are added using `apt-get install` and `conda install`.  Data are copied into a folder in the docker image using `COPY /data/ /data`.  This data will be permanently included in the docker image.  A drawback to including data in the docker image is that the docker image will need more disk space.  As an alternative, see the Linking Data Volumes section above for linking data to the container.   
  
<br>  
A Dockerfile is used to create an image using the following command in the folder with the Dockerfile: 

```bash
$ docker build -t geohackweek2016/new_dockerimage .  
```

<br>
The docker image can be pushed to the Docker Hub. First, login using your Docker Hub username and password. 

```bash
$ docker login
```

<br>
Then push the image to Docker Hub. 

```bash
$ docker push geohackweek2016/new_dockerimage
```

If you want to be added to the Docker Hub team for geohackweek2016 (and have push access), please contact the organizers, and we will add you.
