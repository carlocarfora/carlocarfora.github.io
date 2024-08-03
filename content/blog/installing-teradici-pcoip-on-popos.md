+++
title = "Installing Teradici PcoIP on Pop!_OS 22.04"
date = 2022-06-28
[taxonomies]
  tags = ["linux", "popos"]
[extra]
  toc = false
+++

Setting up a new computer with Pop!_OS, I need to use a Teradici PCoIP Software Client for remoting into work. I’d previously been using Fedora where it had to be run in a docker container. I thought it would run natively as it was built for Ubuntu, but there were some missing dependencies and so back to setting it up in a docker container.

Hopefully the instructions below help anyone trying to install the Teradici PCoIP Software client!

The Official Docs
The docs on installing it without a container:

https://www.teradici.com/web-help/pcoip_client/LInux/21.03/installation/install_client_ubuntu/

Docs on installing in a docker container:

https://www.teradici.com/web-help/pcoip_client/linux/21.01/reference/docker_containers/

Trying it with the official docs had no luck. The repo was throwing up some errors, so back to the drawing board.

What Worked
After some head scratching the official docs looked to be outdated so modifying the instructions a bit worked for me. Turns out the official dockerfile was pointing to the old repo and only by looking at the instructions to install without a container and then the container instructions did I realise this. Steps below!

Steps
Install Docker. I had to use docker.io for Pop!_OS.

$ sudo apt-get install docker.io
$ xhost +local:docker
Add docker to sudo so don't need sudo commands. Not required though.

$ sudo groupadd docker
$ sudo usermod -a -G docker  $USER
Create a directory for the dockerfile to live in first, cd to it and then build.

$ cd <directory with Dockerfile>
$ docker build -t pcoip-client .
Dockerfile for docker container. Note that the user name has been changed to my user name carlo and user/group id's were already 1000 for me. You need to make sure you set this up properly or it won't work. Use the command id to find your user/group id's and change carlo to your username.

FROM ubuntu:18.04

# Use the following two lines to install the Teradici repository package
RUN apt-get update && apt-get install -y wget && apt-get install -y curl

# This was the bit that is wrong in the example dockerfile
RUN curl -1sLf https://dl.teradici.com/DeAdBCiUYInHcSTy/pcoip-client/cfg/setup/bash.deb.sh | sh=ubuntu codename=bionic bash

# Install apt-transport-https to support the client installation
RUN apt-get update && apt-get install -y apt-transport-https

# Install the client application
RUN apt-get install -y pcoip-client

# Setup a functional user within the docker container with the same permissions as your local user.
# Replace 1000 with your user / group id
# Replace myuser with your local username
RUN export uid=1000 gid=1000 && \
    mkdir -p /etc/sudoers.d/ && \
    mkdir -p /home/carlo && \
    echo "carlo:x:${uid}:${gid}:carlo,,,:/home/carlo:/bin/bash" >> /etc/passwd && \
    echo "carlo:x:${uid}:" >> /etc/group && \
    echo "carlo ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/carlo && \
    chmod 0440 /etc/sudoers.d/carlo && \
    chown ${uid}:${gid} -R /home/carlo

# Set some environment variables for the current user
USER carlo
ENV HOME /home/carlo

# Set the path for QT to find the keyboard context
ENV QT_XKB_CONFIG_ROOT /user/share/X11/xkb

ENTRYPOINT exec pcoip-client
Command to run the docker container. Again don't forget to change carlo to your username.

$ docker run -d --rm -h myhost -v $(pwd)/.config/:/home/carlo/.config/Teradici -v $(pwd)/.logs:/tmp/Teradici/$USER/PCoIPClient/logs -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY pcoip-client
Hope that has helped anyone reading this to get it installed on their machine!

Carlo
