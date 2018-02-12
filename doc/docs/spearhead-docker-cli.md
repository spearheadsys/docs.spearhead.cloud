# Spearhead Docker CLI

## Overview
The Spearhead Docker CLI tool installs compatible versions of the Docker and Docker Compose CLI known to work with the Spearhead Cloud. While the Spearhead Cloud supports most of the features from Docker's 1.22 spec there are some exceptions:

* docker build: building Docker images using the Spearhead Cloud Docker Host is not supported. Use your own local environment for building.
* docker network: Spearhead Cloud supports many enhanced network concepts which are not available in Docker however you will have to use the Cloud Portal, the Spearhead CLI tool or the CloudAPI to manage them for now.
* Docker Compose file format 2+: This works  if you define network_mode: bridge. Your containers will not be using bridge networking however. It is a workaround.
* docker volume: This family of commands in Docker is probably an antipattern that turns Docker hosts into pets, but take a look at the volumes section below.
* docker service: this is not yet supported. Workarounds are available by contacting [help@spearhead.cloud](mailto:help@spearhead.cloud)
* docker events: there are no plans to implement this feature

### Volumes
*spearhead-docker* volumes commands and concepts are different from Docker Inc's:

* there is a limit of 8 'data' volumes per container
* 'host' volumes (/hostpath:/containerpath) are not supported
* you cannot delete a container which has a volume that another container is sharing (via --volumes-from), you must first delete all containers using that volume.
* there is a limit of 1 --volumes-from argument per container
* When you use --volumes-from you are necessarily co-provisioned with the container you are sharing the volumes from. If the physical host on which the source container exists does not have capacity for the new container, provisioning a new container using --volumes-from will fail.
* When you use --volumes-from, volumes that don't belong to the container specified (including those that this container is sharing from others) are ignored. Only volumes belonging to the specified container will be considered.

## Installation
The following script will configure the Docker tools for use with the Spearhead Cloud. While you can use the native docker tools to communicate with the Spearhead Cloud Docker host we recommend using the spearhead-docker CLI tool as it takes into consideration deviations from the standard Docker API and will handle things accordingly.

The following script will leave your current Docker tools alone making it easy and convenient to switch between them.

```
sudo bash -c 'curl -o /usr/local/bin/spearhead-docker https://code.spearhead.cloud/Spearhead/spearhead-docker-cli/raw/branch/master/spearhead-docker && chmod +x /usr/local/bin/spearhead-docker && ln -Fs /usr/local/bin/spearhead-docker /usr/local/bin/spearhead-compose && ln -Fs /usr/local/bin/spearhead-docker /usr/local/bin/spearhead-docker-install'
```

That command will copy the spearhead-docker shell script from this repo, and link it as spearhead-compose and spearhead-docker-install.

To complete the installation, run sudo spearhead-docker-install to install the platform-specific versions of the Docker (now Moby) and Docker Compose CLI tools. These versions will not replace any existing Docker or Docker Compose versions you may have installed.

## Usage
Once installed, use *spearhead-docker* and *spearhead-compose* in place of docker and docker-compose when interacting with the Spearhead Cloud.
