# molecule test

This README file demonstrate molecule work.

## Getting Started

### Requirements
The main requirements are documented in `requirements.txt` but this demo assumes you have a few other things installed including
- `docker`
- `python`

## Virtual environment
Practice safe python. 

### Create virtual environment with PyEnv 
PyEnv more convenient tool for creating and managing virtual environments.

Install PyEnv build dependencies for Ubuntu/Debian.
```bash
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev \
libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python-openssl
```

After you’ve installed the build dependencies, you’re ready to install pyenv itself.

```bash
curl https://pyenv.run | bash

```
This will install pyenv along with a few plugins that are useful:

1. pyenv: The actual pyenv application
2. pyenv-virtualenv: Plugin for pyenv and virtual environments
3. pyenv-update: Plugin for updating pyenv
4. pyenv-doctor: Plugin to verify that pyenv and build dependencies are installed
5. pyenv-which-ext: Plugin to automatically lookup system commands

```bash
pyenv install --list | grep " 3\.[8]"
```

Once you find the version you want, you can install it with a single command:
```bash
pyenv install -v 3.8.16
```

Each installed python version and virtual environments that you have is located in your pyenv root directory:
```bash
ls ~/.pyenv/versions/ #or
pyenv versions
```

Create virtual environment with pyenv:
```bash
pyenv virtualenv 3.8.16 molecule-test
```

For activate and deactivate Virtual Environment:
```bash
pyenv activate molecule-test 
pyenv deactivate # or if not working
pyenv shell system
```

Delete Virtual Environment:
```bash
pyenv versions
pyenv uninstall 3.8.16/envs/molecule-test
```

## Run molecule tests
Run all tests
```bash
molecule test
```

Run scenario
```bash
molecule test -s scenario_name
```

Run scenario with verbose
```bash
molecule converge -s scenario_name -- -vvv
```

List status of instances
```bash
molecule list
```

### Ansible vault
If used ansible vault, write variable ANSIBLE_VAULT_PASSWORD_FILE. Example:
```bash
ANSIBLE_VAULT_PASSWORD_FILE=$HOME/.vault.txt molecule converge
```
