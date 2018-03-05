# DNS

> The Container Name Service is currently not enabled for public use.

## Overview
Spearhead Container Name Service (CNS) is a completely automatic, universal DNS for your containers and VMs on Spearhead Cloud. It is tightly integrated with Spearhead Cloud to eliminate complexity and simplify operations.

Spearhead CNS serves address records (both A and AAAA) for containers by instance name and tags. Multiple containers providing the same service can share the same tag and will be returned in the same address record. Because Spearhead CNS knows when containers are started or stopped, including unexpected stops, the DNS information is automatically updated and requests will be sent to running containers.

##

Spearhead CNS offers two facilities:

1. Serving address records for instances by instance name.
2. Serve address records for instances grouped by service label/tag (multiple instances are listed in one DNS name, depending on their availability)

Once activated, running instances in Spearhead Compute Service with public network interfaces will be available in DNS using the following FQDN patterns:

<instance name>.inst.<account uuid>.<data center name>.spearhead.cloud
<service name>.svc.<account uuid>.<data center name>.triton.zone

When activated for an account, running instances in Spearhead Compute Service with private network interfaces will be available in DNS and accessible inside the data center using the following FQDN patterns:

<instance name>.inst.<account uuid>.<data center name>.spearhead.systems
<service name>.svc.<account uuid>.<data center name>.cns.spearhead.systems


All the DNS names for an instance can be found in the instance details in Cloud Portal or via the Spearhead Cloud Tool (```spearhead get <instance name>```).
