# Start and stopping instances
Before you can use the Spearhead Cloud you must create and account and provide your billing details.

## Using the Spearhead Cloud Portal
TBD: shortly

## Using the Spearhead CLI Tool

First install the [Spearhead CLI Tool](https://docs.spearhead.cloud/spearhead-cli/).

Now you can use the ```spearhead``` command to create your infrastructure container by running ```spearhead instance create```. Here is an example of launching a debian container:

```spearhead create -n webdeb -w debian-8  s1-general-1-2-100```

The command parameters define:

1. -n (--name) provides the name of the container. In our case it is "webdeb".
2. -w "waits" for the container to be created before returning the prompt.
3. We are using debian-8 as our image<sub>1</sub>.
4. We are using s1-general-1-2-100 as our package<sub>2</sub>.


<sup>1</sup><sub>to view available images use ```spearhead images```</sub>

<sup>2</sup><sub>to view available packages use ```spearhead packages```</sub>

## Deleting your instances
Once you are finished using your instances you can delete them using the ```speahread``` CLI tool or using the Spearhead Cloud Portal.

### Delete using the spearhead cli tool
Using the ```spearhead``` cli tool is the most convenient way to quickly delete your instances (including Docker containers). By issuing ```spearhead instance delete <UUID>``` you can remove any instance including Docker containers.

### Delete using the Spearhead Cloud Portal
To delete an instance using the Spearhead Cloud Portal
