# molecule test

This README file demonstrate molecule work.

## Getting Started

### Requirements
The main requirements are documented in `requirements.txt` but this demo assumes you have a few other things installed including
- `docker`
- `python`

## Virtual environment
Practice safe python. 

### Create virtual environment with virtualenv
Create a virtual environment with python 3.8. 
```
virtualenv venv --python=python3.8
```

Activate virtual env.
```
source ./venv/bin/activate
```

Install Molecule and requirements.
```
pip install -r ./molecule/requirements.txt
```

Make sure Molecule is installed.
```
molecule --help
```

### Create virtual environment with PyEnv 
PyEnv more convenient tool for creating and managing virtual environments.

Install PyEnv build dependencies for Ubuntu/Debian.
```
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev \
libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python-openssl
```

After you’ve installed the build dependencies, you’re ready to install pyenv itself.

```
curl https://pyenv.run | bash

```
This will install pyenv along with a few plugins that are useful:

1. pyenv: The actual pyenv application
2. pyenv-virtualenv: Plugin for pyenv and virtual environments
3. pyenv-update: Plugin for updating pyenv
4. pyenv-doctor: Plugin to verify that pyenv and build dependencies are installed
5. pyenv-which-ext: Plugin to automatically lookup system commands

```
pyenv install --list | grep " 3\.[8]"
```

Once you find the version you want, you can install it with a single command:
```
pyenv install -v 3.8.16
```

Create virtual environment with pyenv:
```
pyenv virtualenv 3.8.16 ansible-role-java
```

For activate and deactivate Virtual Environment:
```
pyenv activate ansible-role-java
pyenv deactivate # or if not working
pyenv shell system
```

## Run molecule tests

Run all tests
```
molecule test
```

Run scenario
```
molecule test -s scenario_name
```

Run scenario with verbose
```
molecule converge -s scenario_name -- -vvv
```

### Ansible vault
If used ansible vault, write variable ANSIBLE_VAULT_PASSWORD_FILE. Example:
```
ANSIBLE_VAULT_PASSWORD_FILE=$HOME/.vault.txt molecule converge
```

# Molecule with vagrant for testing on KVM virtual machines
Molecule’s default use Docker driver. That means if you want to test your role with Ubuntu 20.04 Molecule will instruct the Docker driver to launch a Docker container with Ubuntu 20.04.
But there are times when you need to test on a "real" operating system.

## Installing KVM with libvirt on Ubuntu 20.04
Run the following command to install KVM and additional virtualization management packages:

```
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager
```
qemu-kvm - software that provides hardware emulation for the KVM hypervisor.
libvirt-daemon-system - configuration files to run the libvirt daemon as a system service.
libvirt-clients - software for managing virtualization platforms.
bridge-utils - a set of command-line tools for configuring ethernet bridges.
virtinst - a set of command-line tools for creating virtual machines.
virt-manager - an easy-to-use GUI interface and supporting command-line utilities for managing virtual machines through libvirt.

To be able to create and manage virtual machines, you’ll need to add your user to the “libvirt” and “kvm” groups. To do that, enter:

```
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
```

$USER is an environment variable that holds the name of the currently logged-in user.
Log out and log back in so that the group membership is refreshed.

## Network Setup
A bridge named “virbr0” is created during the installation process. This device uses NAT to connect the guests' machines to the outside world.

You can use the brctl tool to list the current bridges and the interfaces they are connected to:

```
brctl show
bridge name	bridge id		      STP enabled	interfaces
virbr0		  8000.52540089db3f	yes		      virbr0-nic
```
The “virbr0” bridge does not have any physical interfaces added. “virbr0-nic” is a virtual device with no traffic routed through it. The sole purpose of this device is to avoid changing the MAC address of the “virbr0” bridge.
This network setup is suitable for most Ubuntu desktop users but has limitations. If you want to access the guests from outside the local network, you’ll need to create a new bridge and configure it so that the guest machines can connect to the outside world through the host physical interface.


## Install vagrant

Download the Vagrant package with wget :

```
curl -O https://releases.hashicorp.com/vagrant/2.2.9/vagrant_2.2.9_x86_64.deb
```
Once the file is downloaded, install it by typing:

```
sudo apt install ./vagrant_2.2.9_x86_64.deb
```

To verify that the installation was successful, run the following command that will print the Vagrant version:

```
vagrant --version
Vagrant 2.2.9
```

### Install vagrant plugins

For installation vagrant plugins use command:
```
vagrant plugin install vagrant-libvirt vagrant-hosts vagrant-hostsupdater
```

vagrant-libvirt - Vagrant plugin that adds a Libvirt provider to Vagrant, allowing Vagrant to control and provision machines via Libvirt toolkit.
vagrant-hosts - Manage vagrant guest local DNS resolution. 
vagrant-hostsupdater - Adds an entry to your /etc/hosts file on the host system.

## Install requirements in virtualenv

Practice safe python. Create a `virtualenv`.
```
virtualenv venv --python=python3.8
```

Activate virtual env.
```
source ./venv/bin/activate
```

Install the python requirements
```
pip install -r ./molecule/requirements.txt
```

## Run molecule test

Create new scenario
```
molecule init scenario kvm --role-name githubixx.ansible_role_wireguard --driver-name vagrant
```

Run molecule test with ansible vault, write variable ANSIBLE_VAULT_PASSWORD_FILE. Example:
```
ANSIBLE_VAULT_PASSWORD_FILE=$HOME/.vault.txt molecule test -s converge
```

Enter to vagrant virtual machine

```
vagrant global-status
vagrant ssh <id>
```
