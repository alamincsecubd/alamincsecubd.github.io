+++
title = 'Understanding Containers with Docker'
date = 2024-03-17T14:01:20+06:00
tags= ["Docker", "Container", "docker-cli", "dockerd", "containerd", "containerd-shim", "runc", "DevOps"]
draft = false
url= '/posts/understanding-containers-with-docker'
+++

## Motivation

Recently, I had to buy a new laptop because my 6-years-old laptop's motherboard got burnt, and it was not fixable. But luckily, my hard drive storage was okay, and I use Git and GitHub a lot, so most of my important projects are on GitHub.

Though all of my personal projects were running perfectly on my old Linux/Ubuntu laptop, they are not running on my new one because of a Python version mismatch, and some packages are not directly available for Apple Silicon. Manually installing all the required packages is quite a pain and still doesn't guarantee that your code will work.

Facing this problem led me into the world of containers. In the following sections, I will share my journey and what I have learned about what a container is, how they are run, and how Docker comes into the picture.

So let's dive in and solve the iconic saying around the dev community: *"It worked on my machine!"* 🙂



## Docker, the origin story
Now lets dive into little bit of history for a moment to know why and how docker is integrated into modern software development.

So it all started with the virtualization of operating systems. As we all know we can use any/multiple of operating systems(ie. Ubuntu/Linux, MacOs, Windows)  on top of our host operating system. This is hardware level virtualization and this kind of virtualization is usually done via a hypervisor (i.e. Virtualbox, VMware). Running a hardware level virtual machine is resource intensive and takes a lot of time to boot. And to run a software on different machines we have to set up multiple virtual machines to run the software on each machines separately.


To solve this problem of hardware level virtualization and resource hungry of doing things, the idea of operating system’s application layers level virtualization came in to solve this problem, LXC (Linux Containers) was released in 2008, where using some linux kernel features (*`namespace`*, *`cgroups`*) from the host operating system's kernel we can create a fully isolated environment by virtualizing the only application layer of the host operating system. ( cool right?)

And these isolated environments are known as containers, we can control how much RAM, CPU, Storage, File I/O is allowed on each container. And the cool thing is when an app runs inside a container it has no idea that it is running inside an isolated environment/container other than the host operating system.

![VM vs Container](/posts/docker-intro/Containers-Virtual-Machines.png)
<center><small> Figure 1: Virtual Machine vs Container. <a href="https://bi-insider.com/posts/virtual-machines-vs-containers/"  target="_blank">source</a></small></center>
<br />

**Virtual machine:** *sits on top of the hypervisor layer with their own operating system and kernel. VMs don't share resources with the host operating system's kernel.*

**Container:** *Sits on the application layer of the host operating system, shares resources with the host operating system kernel.* 

Therefore to make use of this groundbreaking way of running applications inside containers, *docker* is born, as a container management tool.




## How does docker works?

![Docker Components](/posts/docker-intro/docker-componets.png)
<center><small> Figure 2: Components of Docker </small></center>

<br />
Docker is comprised of 3 major components


1. **Docker Client(`docker-cli`)**

Users use this command line tool to communicate with docker deamon. Then docker demaon executes the user requested command.

few examples are:

`docker build`: to build an image

`docker run image_name`: to run a container off of that image 

`docker stop cotainer_id`: to stop a running container



2. **Docker Host**

**Docker image**:
A Docker image is a snapshot or blueprint of the libraries and dependencies required inside a container for an application to run.

**Docker container**: 
It is like a super lightweight VM running the actual application with only required libraries and dependencies to run that application.

**Docker Demaon**: 
It is a persistent background process(`dockerd`) which manages docker images, containers, networks, storages. It continously listens for Docker API requests from Docker client (docker-cli).

3. **Docker Registry**

Docker registry is used for distributing docker images. It is like Github for the docker images.
When we run `docker pull <image_name>`, docker deamon searches the image in the docker registry and if the image is found it pulls the image and stroes it locally so that we can run containers from that image.


<br />



</ol>








## How docker runs a container internally

As we disscussed earlier, docker is a container management tool. It uses client-server architecture to manage the containers. It has a docker client(`docker-cli`) which communicates with the docker deamon (`dockerd`) via *REST API*.

Then `dockerd` is responsible for handling the request to another deamon known as `conatinerd` (a deamon which manages container's lifecycle) to  spawn up the requested docker container by the user. `dockerd` and `containerd` uses *gRPC* protocol to communicate with each other.

`containerd` then invokes `runc`, a tool which is responsible for running the actual container with the help of linux kernel `namespace` and `cgroups` primitive features.


![How docker runs containers](/posts/docker-intro/docker-architecture-complete-overview.png)
<center><small>Figure 3: How a container runs internally</small></center>

<br />


If you have come this far. I hope you are feeling like a ***Contai Nerd*** 😀

In the next post, I will discuss how we can run a simple web app inside a container using docker. cheers!






