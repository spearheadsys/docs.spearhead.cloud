# Start and stopping instances
Before you can use the Spearhead Cloud you must create an account and provide your billing details.

## Using the Spearhead Cloud Portal
TBD: shortly

## Using the Spearhead CLI Tool

First install the [Spearhead CLI Tool](https://docs.spearhead.cloud/spearhead-cli/).

Now you can use the ```spearhead``` command to create your infrastructure container by running ```spearhead instance create```. Here is an example of launching a debian container:

```spearhead create -n webdeb -w debian-8  s1-general-1-2-100```

The command parameters define:

1. -n (--name) provides the name of the container. In our case it is "webdeb".
2. -w "waits" for the instance to be created before returning the prompt.
3. We are using debian-8 as our image<sub>1</sub>.
4. We are using s1-general-1-2-100 as our package<sub>2</sub>.


<sup>1</sup><sub>to view available images use ```spearhead images```</sub>

<sup>2</sup><sub>to view available packages use ```spearhead packages```</sub>

## Deleting your instances
Once you are finished using your instances you can delete them using the ```speahread``` CLI tool or using the Spearhead Cloud Portal.

### Delete using the spearhead cli tool
Using the ```spearhead``` cli tool is the most convenient way to quickly delete your instances (including Docker containers). By issuing ```spearhead instance delete <UUID>``` you can remove any instance including Docker containers.

### Delete using the Spearhead Cloud Portal
TBD: To delete an instance using the Spearhead Cloud Portal

## Restart policies

Containers behave a bit differently than virtual machines in Spearhead Cloud when it comes to restart policies. These details will help you when we perform maintenance and you would like your contains to automatically restart (or not):

* Specifying --restart=no (this is default):
    * if the node your container is on is rebooted, your container will be off
    * if your container process exits it will remain powered off until you restart it.
* Specifying --restart=always:
    * if the node your container is on is rebooted, and your container was running at the time of the reboot, your container will be started when the node boots.
    * if your container process exits (regardless of exit status), the container will be restarted and the RestartCount will be incremented (see below on delays between restarts).
* Specifying --restart=on-failure[:maxretries]:
    * if the node your container is on is rebooted, your container will only be started when the node boots if the init process of your container exited non-zero as part of the CN reboot.
    * if your container process exits with a non-zero exit status, the container will be restarted and the RestartCount will be incremented. If you specified a maxretries and this is reached, the container will be stopped and not restarted again automatically.
    * if your container process exits with a zero status, the container will not be restarted again automatically.

When restarting your container automatically (the cases mentioned above) there is a delay between restarts. spearhead-docker uses the same delay frequency as Docker Inc's. After exiting but before starting again we delay ((2 ^ RestartCount) * 100) ms. So on the first restart (with RestartCount = 0) we will delay 100ms, then 200, then 400, etc. The amount of delay is not guaranteed. In the case of a CN reboot or in other operational situations a retry may occur sooner.

The main way that this is different from Docker Inc's docker is that with Docker Inc's docker, if you restart the docker daemon all containers will be stopped and those with --restart=always will be started again. With spearhead-docker restarting the management systems will not touch your container but restarting the compute node the container lives on will.

If you want your container to always be running you most likely want to specify --restart=always to avoid your containers being stopped when a CN reboots.

> Last edit: 2018 Thu 26 Jul 19:50 GMT+3 