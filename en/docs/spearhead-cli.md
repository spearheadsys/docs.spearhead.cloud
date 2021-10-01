# Spearhead Cloud CLI tool

> Last edit: Fri Oct 1 16:05:33 EEST 2021

The Spearhead Cloud CLI tool allows you to manage your Spearhead Cloud infrastructure. While the Cloud Portal is being developed this tool is the preferred method for managing your infrastructure.

> The Spearhead Cloud Portal will provide all of the functionality available via the spearhead cli tool.

The *spearhead* CLI tool can currently handle the following tasks:

* manage compute instances (containers and virtual machines)
* create, delete, resize, snapshot
* view Spearhead Cloud packages
* create, delete and manage Spearhead Cloud images
* view networks
* create and delete firewall rules
* view Spearhead Cloud Datacenter's
* management of your own account
* manage your SSH key(s)

## Install Spearhead Cloud CLI tool
The *spearhead* CLI tool can be installed via npm (Node Package Manager). This requires that you have a recent version of npm installed from [http://www.npmjs.org](http://www.npmjs.org).

Once you have npm installed just run the following command to configure the Spearhead Cloud CLI tool. Please note that on windows you need to use the global flag (-g).

> npm install -g spearhead

On some platforms you may be required to use sudo (sudo npm install ...).

## Environment Variables
On Windows, setting up these variables is required.

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
To configure a new Spearhead Cloud Profile have the name of the datacenter API endpoint ready and run the following command and answer the questions.

```
$ spearhead profile create
```

For more information about Profile, run ```spearhead profile --help``` command.

## Create an instance
Creating an instance is possible via the ```spearhead create``` command. You will first need to identify the image and package to assign to you new instance.

### Finding on image
To list all available images run ```spearhead imgs``` and you will be presented with a list of our available images.

### Finding a package
To list all available packages run ```spearhead pkgs``` and you will be presented with a list of our available packages.

Note that the names starting with **k** are KVM hardware virtual machines.  

## Delete an instance
To remove an instance run ```spearhead delete <instance>``` command. Optionally you can use ```spearhead rm <instance>```. ```<instance>``` can be either an instance name, id or short id.

## Viewing instances
To view a list of all instances configured for your account run ```spearhead ls```.

```
$ spearhead ls
SHORTID   NAME                    IMG                    STATE    FLAGS  AGE
c0fe5e19  code.alberta.internal   ubuntu-16.04@20170403  running  -      8w
f49f145e  ns01                    ubuntu-16.04@20170403  running  -      7w
5940b249  agitated_turing         0227ae25               running  D      2d
fb174b04  ncmysql                 ff8ecad5               running  DF     2d
9adb0454  routeme                 ubuntu-16.04@20170403  running  -      2d
64e8917b  kubecon                 ubuntu-16.04@20170403  running  -      2d
```

## Useful commands

#### wait
The ```spearhead``` tool does not wait for commands to finish, it will return control back to you quickly. There are situation where waiting for one thing to finish before starting another is useful. For such scenarios you can wait by using the ```--wait``` or ```-w``` flags or by using ```spearhead instance create wait``` command.

#### script
Another useful command is the ```--script``` flag which will run a user specified script at the end of provisioning.

Below is an example of using the ```--script``` flag.

```
spearhead instance create \
    --name=some-instance \
    7b5981c4 \
    8b4fdd0b \
    --wait \
    --script=./myscrtip.sh
```
### instance details
To view all of your instance's details run ```spearhead instance get -j some-instance```. This will give you a full JSON blob of your instance details. You can optionally pipe the instance details output through the ```json``` command and parse or perform other operations.
