# Getting Started

> Get started with the Spearhead Cloud today. Sign-up for a [free trial](https://spearhead.cloud/trial).


## Overview
This document will describe how to set-up a Spearhead Cloud account.

### Create a Spearhead Cloud  account

1. Create a [Spearhead Cloud account](https://spearhead.cloud/start).
2. Add your SSH key(s).

That is it. Now you are ready to start working with the Spearhead Cloud.

### SSH Keys
You can use your existing key or create a new one in the Spearhead Cloud Portal.

* Manually create an SSH key pair from MacOS (or Linux/Unix)
* Manually create and SSH key pair from Windows

When you create your account you will be asked to add your SSH key. Do not skip this step as the key is required if you wish to log-in to your remote linux/unix systems or if you wish to use the Spearhead Cloud Tools<sub>1<sub>.

You may add your key at a later time by visiting your Profile settings in the Cloud Portal.

<sup>1</sup><sub>Without these SSH keys uploaded you will not be able to access your instances.</sub>
### Next steps

* Install the [Spearhead Cloud CLI tools](https://docs.spearhead.cloud/spearhead-cli) and manage your cloud from the command line
* Continue to the [Cloud Portal](https://docs.spearhead.cloud/cloud-portal).


## Terminology

* instance(s) - an instance in the spearhead cloud may refer to:
    * a hardware virtual machine
    * an infrastructure container
    * a Docker container
* image(s) - images contain your operating system, applications and services. Spearhead provides several images to help you get started. Some images contain complete applications and services for easy and quick deployments or start from scratch images where you can configure the applications yourself.
    * Docker container images - you can connect public or private container registries to run those images on Spearhead Cloud.
    * Infrastructure container images - are container native Linux and SmartOS systems running on bare-metal. These offer high performance, scalability by allowing on-line resizing and security. Take advantage of preconfigured images for many scopes such as databases and web servers.
    * Hardware virtual machines - these are typical virtual machines that run using KVM. We provide these for legacy systems or workloads that cannot be deployed as containers.
* package(s) - packages define the resources allocated to an instance.
