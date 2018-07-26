# Connecting to your instances
Before you can use the Spearhead Cloud you must create and account and provide your billing details.

SSH is the primary way to access your containers or vm's with the exception of Docker containers and Windows VM's. You must have the Spearhead CLI Tools installed.

## Connecting to Docker containers
You can connect to your Docker container as usual, executing ```spearhead-docker --tls exec -it <container name or id> bash``` which would give you shell access into your running container.

It may be tempting to run SSH within your container, however we do not recommend this method.

## Connecting to containers or virtual machines
Connect to your running containers (or virtual machines) by executing ```spearhead ssh <short id or instance name>```. Alternatively (or in case this does not work for your environment) you can obtain the IP address of your instance (```spearhead instance get <short-id or name>``) and SSH directly (```ssh root@x.x.x.x```).

TBD: Click here for detailed information on how to SSH into your instance.

## Connecting to Windows instances
All windows instances are preconfigured with RDP access enabled for the Administrator account. Obtain the IP address of your instance and then connect using your preferred RDP client.

```spearhead instance get <short-id or name>``` and look for the primaryIP field.

> Last edit: 2018 Thu 26 Jul 19:48 GMT+3 