# Spearhead Cloud CLI tools

The Spearhead Cloud CLI tool allows you to manage your Spearhead Cloud infrastructure. While the Cloud Portal is being developed this tool is the preferred method for managing your infrastructure.

> The Spearhead Cloud Portal will provide all of the functionality available via the spearhead cli tool.

The *spearhead* CLI tool can currently handle the following tasks:
- manage compute instances (containers and virtual machines)
  - create, delete, resize, snapshot
- view Spearhead Cloud packages
- create, delete and manage Spearhead Cloud images
- view networks
- create and delete firewall rules
- view Spearhead Cloud Datacenter's
- management of your own account
- manage your SSH key(s)

## Install Spearhead Cloud CLI tool
The *spearhead* CLI tool can be installed via npm (Node Package Manager). This requires that you have a recent version of npm installed from [http://www.npmjs.org](http://www.npmjs.org).

Once you have npm installed just run the following command to configure the Spearhead Cloud CLI tool. Please note that on windows you need to use the global flag (-g).

> npm install -g spearhead

On some platforms you may be required to use sudo (sudo npm install ...).

## Environment Variables
On Windows, setting up these variables is required. On other platforms these are not required however we recommend configuring these.

```
export SC_PROFILE="env"
export SC_URL="https://eu-ro-1.api.spearhead.cloud"
export SC_ACCOUNT="<SC_USERNAME>"
unset SC_USER
export SC_KEY_ID="$(ssh-keygen -l -f $HOME/.ssh/id_rsa.pub | awk '{print $2}')"
unset SC_TESTING
unset SC_PROFILE
```

These variables will configure the *spearhead* env profile for you.

## Spearhead Profiles
The *spearhead* CLI tool allows for the use of profiles to manage multiple Spearhead Cloud accounts. A profile contains your user details (username), a Spearhead Cloud Datacenter API URL and SSH key(s).

### Configure a Spearhead Cloud Profile

## Create an instance

### Finding on image

### Finding a package

### Viewing instances

### Useful commands
wait
script
show instance details
cleaning up
