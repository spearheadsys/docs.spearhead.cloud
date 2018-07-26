# DNS

> The Container Name Service is *now* enabled for public use. 

## Overview
Spearhead Container Name Service (CNS) is a completely automatic, universal DNS for your containers and VMs on Spearhead Cloud. It is tightly integrated with Spearhead Cloud to eliminate complexity and simplify operations.

Spearhead CNS serves address records (both A and AAAA) for containers by instance name and tags. Multiple containers providing the same service can share the same tag and will be returned in the same address record. Because Spearhead CNS knows when containers are started or stopped, including unexpected stops, the DNS information is automatically updated and requests will be sent to running containers.

##

Spearhead CNS offers two facilities:

1. Serving address records for instances by instance name.
2. Serve address records for instances grouped by service label/tag (multiple instances are listed in one DNS name, depending on their availability)

Once activated, running instances in Spearhead Compute Service will be available in DNS using the following FQDN patterns:

<instance name OR alias>.inst.<account uuid OR login_name>.<data center name>.spearhead.cloud
<service name>.svc.<account uuid OR login_name>.<data center name>.spearhead.cloud

Here is an example output from `spearhead instance get` <name>:
```
"dns_names": [
        "11163447-6bc4-4d5f-a168-9981824857c9.inst.41824237-4616-42e3-d189-f9bd0242b4e4.ro-1.on.spearhead.cloud",
        "mpana-fab-lan.11163447-6bc4-4d5f-a168-9981824857c9.inst.41824237-4616-42e3-d189-f9bd0242b4e4.ro-1.on.spearhead.cloud",
        "11163447-6bc4-4d5f-a168-9981824857c9.inst.mpana.ro-1.on.spearhead.cloud",
        "mpana-fab-lan.11163447-6bc4-4d5f-a168-9981824857c9.inst.mpana.ro-1.on.spearhead.cloud",
        "cinema.inst.41824237-4616-42e3-d189-f9bd0242b4e4.ro-1.on.spearhead.cloud",
        "mpana-fab-lan.cinema.inst.41824237-4616-42e3-d189-f9bd0242b4e4.ro-1.on.spearhead.cloud",
        "cinema.inst.mpana.ro-1.on.spearhead.cloud",
        "mpana-fab-lan.cinema.inst.mpana.ro-1.on.spearhead.cloud"
    ]
```

> Last edit: 2018 Thu 26 Jul 19:48 GMT+3 