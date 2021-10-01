# Docker Remote API implementation for Spearhead Cloud

> Last edit: Fri Oct1 14:41:40 EEST 2021

This document explains how to set the Docker client to manage Docker images and containers in Spearhead Cloud's Docker implementation. Familiarity with Docker is assumed. If you are new to Docker, read the [Docker User Guide](https://docs.docker.com/get-started/) to understand some of the basic concepts before you start using Triton.

Spearhead Cloud can be viewed as one large Docker host and does not require you to manage your own Docker hosts. Your Docker client applications can interact with a single API endpoint for a given Spearhead Cloud availbilty zone.

## Connecting to the API

Docker client applications, including the Docker CLI and Docker Compose, can connect to the Spearhead Cloud Docker remote API endpoint to launch and control Docker containers across an entire availbility zone.

Connecting to the API requires an account on Spearhead Cloud, SSH key, and the CloudAPI URL for that data center, as well as the Docker CLI or some other Docker client. Spearhead  provides a helper script to configure a Docker client, including the Docker CLI.

### Docker version

Spearhead Cloud Docker supports clients using Docker Remote API v1.20 to 1.24. For the Docker CLI, this includes Docker v1.8 to 1.12.

You can see the version of your currently installed Docker CLI:

$ docker --version
Docker version 1.8.1, build d12ea79
Please install or upgrade the Docker CLI if you do not have it or have an older version.

### API endpoint

Each availabity zone (data center) is a single Docker API endpoint. CloudAPI is used as a helper to configure the client to connect to the Docker Remote API. Determining the correct CloudAPI URL depends on which data center you're connecting to.

### User accounts, authentication, and security

User accounts in Spearhead Cloud require one or more SSH keys. The keys are used to identify and secure SSH access to containers and other resources in Spearhead Cloud.

Spearhead Cloud Docker uses Docker's TLS authentication scheme both to identify the requesting user and secure the API endpoint. The Joyent provided Docker helper script will generates a TLS certificate using your SSH key and write it to a directory in your user account.

### Installation
You must first install node-spearhead as detailed [here](/spearhead-cli/#install-spearhead-cloud-cli-tool) and a profile.

Once you have the spearhead CLI available you can use the command `spearhead profile docker-setup` to configure your environment for docker.

Once done, you can use either your own Docker CLI or the ones provided by Spearhead *[here](http://127.0.0.1:8000/spearhead-docker-cli/#installation).

*the reason we provide these packaged is to assure compatibility between our Docker Remote API implementation and the Docker Client. There may come a time when Docker decided to no longer support these API's in which case our provided binaries will continue to work.

## Features

Spearhead Cloud Docker offers a number of unique features to our container-native infrastructure, including:

Placement: automatic placement of containers across the entire data center.
Networking: one or more real IP addresses for each container.
Resource allocation: specify RAM, CPU, and storage for each container.
Volumes: container-native volume management.
Private repositories: image repository management
Docker CLI commands and Docker Remote API methods


## Divergence

The Spearhead Cloud Docker implementation does have some differences from Docker Inc.'s implementation. [See the full list](/docker-divergence).