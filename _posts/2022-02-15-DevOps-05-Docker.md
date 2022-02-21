---
title: DevOps 05 - Docker
layout: post
subtitle: null
date: '2022-02-15 15:20:00'
author: Lanzhou
header-img: img/post-bg-unix-linux.jpg
tags:
- study
- English
- wiki
- devops
---
**Research summary:**

- Basics
- Compose
- Networking


### 1. History of Docker

Our story began in 2010, at that time some IT engineers started a company "dotCloud". The company mainly provides cloud computing service based on PaaS. More specifically, a containerization technology related to LXC, which is Linux container.

Then dotCloud company simplified and standardized their containerization technology and renamed it as "Docker".

After they created Docker tech, it did not attract a lot of attention in the IT industry. So dotCloud, as a small start up company, is struggling under fierce competitions.

So the company then decided to make Docker an open source platform. And everybody can join in the community to contribute code and opinions. In March, 2013, one of the founder of dotCloud, Solomon Hykes officially announced the they'll open source Docker project.

Since then, more and more IT engineers discovered the benefit of Docker, and it became more and more popular. dotCloud company even changed its name to "Docker Inc."

and that leads to a question, why people like Docker?

### 2. Benefit of Docker

In the past, the most popular virtualisation technology is virtual machine (Hypervisors) - It is a software that allow you to emulate hardware, and you can install another operating system on top of it.

![hypervisor graph](/img/in-post/hypervisor.png)

For example, you can use VMware or OpenStack to start one or more virtual machine on your physical host computer. These virtual machines are separated and does not affect each other.

Comparing with Hypervisor, Docker use a different functioning mechanism, which is also the most significant difference between them — the way they boot up and consume resources.

Docker works on the host kernel itself. So it does not allow the user to create multiple instances of operating systems. Instead, they create containers that act as virtual application environments for the user to work on.

For Docker containers, all you had to do was pack up exactly what you need - the libraries, or anything "uncommon" that doesn't exist in the kernel by default. So comparing with Hypervisor virtual machines, Docker containers are much more lightweight. It provides apps with a very small "sandbox" environment.

Docker Benefits:

- Boots within seconds (Hypervisor - up to 1 min to boot up)
- Much less resource consumption (Hypervisor - consumes gigabytes of space)

![containers graph](/img/in-post/containers.png)

### 3. Docker basic concepts

Next, let's take a closer look of Docker.

In order to fully understand docker, we can have a look at their slogan on their official website:

1."Build safer, share wider, run faster"

2."Accelerate how you build, share, and run modern applications."

![docker slogan](/img/in-post/docker-slogan.png)

Key words: Build, Share and Run;

- Build:
    - Package applications as portable container images to run in any environment consistently.
- Share:
    - We need Repository and Registry. It's a lot like git repo & github.
    - A docker image can be compared to a git repo, your local git repo can be hosted inside a Github repository, but it could also be hosted on Gitlab, or BitBucket. Or you can just let it sit on your local machine without hosting it anywhere.
    - For a Docker image, you can choose to push it to the Docker Hub. Which is a public and private service for hosting Docker repositories. There are other 3rd party hosting service too, like cloudsmith, Cloudsmith also provides public & private registries for Docker images. And also Amazon ECR (a fully managed container registry offering high-performance hosting)
    - Docker repository is a place for me to publish and access the Docker images.
    - Docker Hub and CloudSmith, these 3rd party hosting services are "Registries". They store a collection of repositories.
    - A repository has many versions of the same image, that are individually versioned with tags.
- Run:
    - Enables applications to run the same way on all environments including testing, staging, and production.
    - Can also run applications with Docker commands or Docker-compose CLI, simpler to use.

For Docker technology, there are 3 core concepts: Image, Container and Repository.

The docker ecosystem, it has three primary parts to it - the client, the host and the registry:

![graph registry strucutre](/img/in-post/4-docker.png)

- We already covered Registry.
- The Client is the primary way of interacting with the Docker host. In very much the same way that client applications talk to an API, the Docker command line interacts with the Docker server.
- The Docker Host is the service that runs the API and the daemon that manages your images and containers. Note that we have this annotated as DOCKER_HOST, because while this is usually installed in the same system as the Client, your Docker Host can be in another machine.

### 4. Docker-compose

- Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services.
- Each of the containers here run in isolation but can interact with each other when required.
- Another great thing about Docker Compose is that users can activate all the services (containers) using a single command.
- Benefits:
    - Single host deployment - This means you can run everything on a single piece of hardware
    - Quick and easy configuration - Due to YAML scripts
    - High productivity - Docker Compose reduces the time it takes to perform tasks
    - Security - All the containers are isolated from each other

### 5. Docker networking

The reason why Docker is so powerful, so popular is that multiple isolated docker containers can connect with each other. And that leads to different modes of networking. Docker provides 5 network drivers by default. When you install docker it creates three networks automatically - Bridge, Host, and None.

- Bridge:
    - The default network driver. If you don't customise which driver to use, then by default it would be set as the network driver of the container. Normally when app is running on standalone containers and need network connections, we select bridge mode.
- Host
    - Host would remove the network boundary between container and Docker host machine. Containers would directly use host network configurations.
- None:
    - This mode will not configure any IP for the container and doesn't have any access to the external network as well as for other containers. It can be used for running batch jobs.
- Overlay:
    - The overlay network driver creates a distributed network among multiple Docker daemon hosts. This network sits on top of (overlays) the host-specific networks, allowing containers connected to it (including swarm service containers) to communicate securely when encryption is enabled.
- Macvlan:
    - container pretend to be an physical computer with real MAC address

### 6. Dockerfile

A Dockerfile is a text file used to build an image. The text contains all the instructions needed to build an image.

Docker automatically generates images by reading the instructions in the Dockerfile.

Dockerfile can be used to invoke any command on the command line.

When creating Docker containers, we should try our best to create "smaller" images, because it would be faster to transfer and deploy relatively small images. According to docker docs, the image defined by Dockerfile should generate containers that are "as ephemeral as possible". That means the contaienr can be stopped and destroyed, then rebuilt and replaced with minimum set up and configuration.

Some useful Dockerfile skills:
- First step: Choose the right base (smaller)
- reduce the number of layers
  - Use shared base images where possible
  - Limit the data written to the container layer
  - Chain RUN statements

Useful Resources:

- [DevOps-Girls/docker-101](https://github.com/DevOps-Girls/docker-101)
- [Visualizing Docker Containers and Images](http://merrigrove.blogspot.com/2015/10/visualizing-docker-containers-and-images.html)
- [[Video] Creating Effective Images](https://www.youtube.com/watch?v=pPsREQbf3PA)
- [docker docs - Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)