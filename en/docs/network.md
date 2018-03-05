# Networks

## Overview
The Spearhead Cloud provides rich networking facilities for your containers and virtual machines. Each container/vm gets a full IP stack and you can define your own networks to create complex environments.

## Types of Networks
Spearhead Cloud provides two types of networks by default.

1. **Networks defined by us**. These are networks we create for tasks such as providing internet access or for private access (non-routable). These networks are usually shared by many customers.
2. **Networks defined by you**. These types of networks are sometimes called VXLAN or overlay networks. These networks are private and may be used to isolate applications or segments of your virtual network topology.

> Spearhead Cloud creates a default VLAN with id 2 in all datacenters.

## Public network
Spearhead Cloud provides a public network named "External" which you can use to attach your vm's to for direct public Internet access. At present you must explicitly specify the External network otherwise provisioning may fail.

Here is an example provisioning an instance explicitly using the External network:

```spearhead create -n nginx03 -N External e74a9cd0 468c03e2```

Here is an example of launching a docker container with a full IP stack on the public Internet. 

```spearhead-docker --tls run -d -P 443 --network External d03e867e8cfe``` 

Docker containers require explicit mapping of networks/ports. If you do not specify a network, the default fabric vlan will be used (i.e. no public IP for your containers).

While our documentation is being written please contact [customer support](mailto:help@spearhead.systems) for additional details.