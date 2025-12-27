---
title: "General Ideas"
date: "2025-09-02"
last_modified: "2025-09-02"
tags: ["docker", "text editor"]
draft: true
---


# Dockerfile

Define how to build the image for a single service. It has the following:

- Base Image ( pulled from docker hub )
- Dependencies ( when dev or deploy )
- Copy source code ( put files inside container )
- build steps 
- expose ports
- set startup command

# DockerCompose yml

Define how to build multiple services together locally. I has the following:

- specify services
- build context ( Dockerfile )
- ports
- dependencies ( middleware depends on backend )

# Docker Image

Immutable read-only templates that define:
- filesystem
- installed software
- env variables
- commands to run

Multiple containers can use same image. Images take disk space since they store all layers needed for the container to run.

## Layers

- Docker images are built with layers, which represent changes ( installed packages, copied files, configurations etc... ).
- Base image is the bottom layer.
- Each command in Dockerfile creates a new layer

Why??

1. Images can share layers Reusability
2. Rebuild only necessary layers Efficiency


# Container

Running instance of an image, built from that blueprint

## Volumes

Once container is deleted all changes are gone. Create volume and mount container on it to make it persisntant

`docker volume create mydata` |||   `docker run -v mydata:/app/data myimage`

Volumes are stored by default at : **/var/lib/docker/volumes**

### Bind mounts

Connects (binds) a specific folder to another folder inside the container.






