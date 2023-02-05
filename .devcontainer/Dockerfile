FROM node:18-bullseye

USER root

RUN apt-get update && \
    apt-get install -y \
    
    # utility used to download files from the Internet.
    wget \
    
    # Utilities for connecting to remote servers via SSH
    openssh-client \
    
    # This is done to keep the docker image size small by removing the `/var/lib/apt/lists/` 
    && rm -rf /var/lib/apt/lists/*

git config user.name