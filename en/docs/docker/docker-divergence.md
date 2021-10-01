# Divergence

> Last Edit: Fri Oct 1 14:41:40 EEST 2021

Spearhead Cloud's implementation of the Docker Remote API has some added features as well as omissions where the API conflicted with the needs of deploying containers across Spearhead Cloud data centers.

## Features

* [CPU and memory resource allocation](/docker-resource-allocation)
* Overlay networks
* Volumes
* Private registries

# Container behavior and contents

## Extra Processes

### zsched
In containers you may see a zsched process in addition to your other processes. It owns the kernel threads that do work on behalf of your zone. The PID of this process should always show up as 0, though some versions of ps on Linux filter this out.

### ipmgmtd
If you don't have docker:noipmgmtd set in your internal_metadata, you will have an additional process ipmgmtd. This is the SmartOS daemon that manages network interfaces and TCP/IP tunables. The default if you use spearheaddocker is that docker:noipmgmtd will be true when you're provisioning a regular LX docker container, and false if you're provisioning a SmartOS container.

### Extra Files

/var/log/sdc-dockerinit.log
This is the log from the dockerinit process which sets up the container for your initial process and then exec()s it. This log exists only for debugging problems with the way the initial process has been setup.

## Extra Filesystems

### /native/*

If you run mount or df you will see several filesystems mounted in from /native. These are bits from the SmartOS host mounted in to support the LX emulation.

### objfs on /system/object
This is the SmartOS kernel object filesystem. The contents of the filesystem are dynamic and reflect the current state of the system. See http://illumos.org/man/7fs/objfs for more information.

### ctfs on /system/contract
This is the SmartOS contract file system which is the interface to the SmartOS contract subsystem.

See http://illumos.org/man/4/contract for more information.

### Exit Statuses

When a container exits the exit status as returned by sdc-docker will currently be different from that which would be returned by Docker Inc's docker. This is due to differences in the way we handle processes within zones. This is currently considered to be a deficiency and should be improved by DOCKER-41.

# Performance of container management functions

Actions performed against spearhead-docker are slower than those same actions performed against a local docker. This is something we are working on, and intend to keep improving over time.

# Docker Remote API methods and Docker CLI commands

In most cases Spearhead has taken great efforts to be bug for bug compatible with Docker Inc's API implementation (see restart policies). Please see documentation for specific methods for any known divergence and file bugs as needed.

Spearhead Docker implements all the API methods necessary to build and deploy Docker containers in the cloud, though there are some missing docker API methods. Here's the list of API methods currently unimplemented:

docker diff, docker events, docker export, docker import, docker load, docker network, docker node, docker pause, docker save, docker service, docker swarm, docker unpause, docker update

## Roadmap:

spearhead-docker development has stopped. While future development will not be happening the system is fully functional and can be used in production. As the list of divergencies from Docker Inc's implementation is getting wider, we recommend setting up Docker hosts directly.

# Images and private registries

Spearhead Docker supports the integration with Docker Hub and third party registries through Docker's Registry v2 API (the deprecated v1 API is not supported).

## Container Logging

Spearhead Docker implements most of the log driver functionality as described in Docker's documentation. The differences in behavior when using these drivers are documented in the Log Drivers feature page.

In addition to the divergence in the use of these drivers, Spearhead Docker handles logging differently than Docker. With Docker, logs are only written to the host when the log driver is set to 'json-file'. With Spearhead Docker the logs are written to the host for every log driver except 'none'. These logs are captured because while the network-based log drivers are good for real-time log analysis, they can potentially lose messages due to conditions like remote host unavailablity and race conditions in startup/shutdown. The host logs are written to local storage by the zoneadmd process that manages the zone and therefore avoids these problems.

The storage for these log files will be within a container's quota but not visible from within the container itself. Operators will see these logs at /zones/:uuid/logs/stdio.log*. These will be rotated to another file in the same directory with a timestamp extension when they reach a max-size value. The max-size can be specified when using the json-file log driver. Otherwise the default (currently defaults to 50M) is set in the Spearhead Cloud configuration. This can be adjusted by an operator through the DEFAULT_MAX_LOG_SIZE property in the docker service's SAPI metadata.

For the user of Spearhead Docker, this additional log when using a log driver other than json-file is not of much use yet, but are important to know about as they do use space from the user's container. In the future, these logs will be automatically consumed and uploaded to Manta which will free this space and give users access to all their logs.

For the operator of Spearhead Docker these additional logs will require some cleanup process for long-lived containers when Manta support is not available.

# Container Labels

Spearhead Cloud uses (and reserves) a number of custom container labels, to provide specific Spearhead features. These are:

The `triton.*` namespace is reserved for Triton specific use cases, these label names are currently defined:

* `triton.cns.disable` (boolean): Can be set on a container to tell the CNS service to not serve records for this instance.
* `triton.cns.services` (string): Comma-separated list of DNS-name strings for the CNS service.
* `triton.cns.reverse_ptr` (string): DNS reverse pointer for this container. Used by the CNS service.
* `triton.network.public` (string): Set on a container, used to specify the external network name the instance will use.
* The `com.joyent.*` namespace is reserved for Spearhead/Triton specific use cases, these label names are currently defined:

* `com.joyent.package` (string): Set on a container, used to choose a specific package for the container. The value can be a package name like hvm-2cpu-8ram-200disk, a UUID, or the first 8 characters of a UUID (short-UUID).
