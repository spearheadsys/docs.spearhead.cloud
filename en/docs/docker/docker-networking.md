# Networking

> Last edit: Fri Oct 1 16:09:12 EEST 2021

If fabric networking is enabled for spearhead-docker, each docker container is provisioned with a private nic on the user's default fabric. This allows only other docker containers provisioned on that fabric to connect to each other. The container is able to reach the external internet via Network Address Translation. Each default fabric network is private to a user - one user's containers cannot connect to another's fabric IP addresses, and vice-versa.

To isolate the traffic between different applications or groups of applications, you may want to create additional networks on your fabric with CloudAPI. Any of these user-defined networks can be designated as the 'default' network to be used for provisioning Docker containers. When you change the 'default' network, it affects only the newly provisioned containers. You may specify a different fabric network during provisioning by passing the --network argument in docker run and docker create, e.g.
```
   docker run --network=dev-net-123 busybox
   docker run --network=d8f607e4 -P -d nginx
```
The work to support docker network commands is in progress. Follow DOCKER-722, DOCKER-723, DOCKER-724, DOCKER-725 for updates.

All docker containers owned by a user have firewalls enabled by default, and their default policy is to block all incoming traffic and allow all outbound traffic. All docker VMs have a Cloud Firewall rule automatically created that allows them to communicate with each other on all ports.

If you specify the -p or -P options to docker run or docker create, the container will receive an external IP address that is reachable over the public internet, and Cloud Firewall rules are automatically created that allow incoming traffic to the appropriate ports from any IP address. For -P, this means all ports that the VM's image exposes. For -p, this means all ports specified as arguments. Port remapping (eg, -p 80:8080) is not supported. 

If fabric networking is not enabled, all docker containers are provisioned with a nic on the 'external' network by default.

The external network used by a container can be changed by setting the triton.network.public label to the name of the desired external network, e.g.

```docker run --label triton.network.public=external2```

Note that this only overrides the default public network selection. This means that when fabric networks are enabled you will still need to specify one of -p or -P to get the public NIC.