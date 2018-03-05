# Firewall
A firewall typically allows you to protect or control network traffic. In the Spearhead Cloud our firewall allows you to manage and control access to and from your instances in a fine-grained centralized fashion.

For legacy virtualized instanced (KVM) you can also further use the platform firewall capabilities.

Firewall rules can be added via the Cloud Portal or through the use of the Spearhead Cloud Tools.

##Overview

Spearhead Cloud Firewall Rules are TCP, UDP, and ICMP traffic rules applied to your instances. By default, Firewall Rules do not apply to new instances that are provisioned. For docker containers, firewall rules are automatically created and enabled based on the exposed port specifications in the dockerfile or from your docker commands.

You can set and edit the rules in the Spearhead Cloud Portal or through the Spearhead Cloud Tools command line interface.

Through the use of Firewall rules you can define what network traffic will be allowed or blocked based on certain criteria:

* based on TCP, UDP, or ICMP port
* based on a specific instance
* based on an IP address
* based on a specific tag

## Creating Firewall rules

### Cloud Portal
To create a Firewall Rule using the portal follow these steps:

> the Spearhead Cloud Portal is still in development. The documentation will be updated once this is released.

### Spearhead Cloud Tools
Using the ```spearhead``` CLI tool you can manage firewall rules for your instances.

#### Creating a firewall rule
To create a firewall rule using the Spearhead Cloud Tools use the ```spearhead fwrule create``` command.

```
fwrule create \
"FROM any TO vm 64e8917b-9snf-8fj2-3846-ba2c9sj63 ALLOW tcp PORT 22"
```

The above wil create a rule allowing SSH (port 22) access to the vm with the ID 64e8917b-9snf-8fj2-3846-ba2c9sj63. For more help creating firewall rules use ```spearhead fwrule --help```

#### Deleting a firewall rule
To delete a firewall rule first get the id of the firewall rule. Now you can issue a detele command as follows:

````
$ spearhead fwrule delete ae9be030-c601-49d7-93e9-0852bf205125
````

#### Listing firewall rules for a specific instance
Firs get a list of your instances and note down the shortid of your vm.

```
$ spearhead ls

SHORTID   NAME                    IMG                    STATE    FLAGS  AGE
c0fe5e19  code.spearhead.inter    ubuntu-16.04@20170403  running  -      8w
```

Now you can get a list of all firewall rules applied to your instance.

```
$ spearhead instance fwrules list c0fe5e19
SHORTID                               ENABLED  GLOBAL  RULE
b31e9c4a-a6ff-407b-a78c-5b135bf2e4c6  true     true    FROM any TO all vms ALLOW icmp TYPE all
ae9be030-c601-49d7-93e9-0852bf205125  true     true    FROM any TO all vms ALLOW icmp6 TYPE all

```

#### Listing all firewall Rules
To list all firewall rules use ```spearhead fwrules``` command.

#### Updating firewall rules
Using the ```spearhead``` cli tool you can also update existing firewall rules. The following command

```
$ spearhead fwrule update b31e9c4a-a6ff-407b-a78c-5b135bf2e4c6 rule="from all vms to tag db allow tcp port 5432"
```

More details regarding the fwrules options can be found using ```spearhead fwrule --help``` or for a specific function such as updating a rule ```spearhead fwrule update --help```

#### Enabling and Disabling firewall rules
Firewall rules can be enabled or disabled as required. This can be achievied using the ```spearhead fwrule enable <FWRULE ID>```. Alternatively, disabling a rule would be ```spearhead fwrule disable <FWRULE ID>```.
