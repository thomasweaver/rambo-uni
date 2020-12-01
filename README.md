# Rambo University
In this repository you will find a basic tutorial aimed to show how to use Vagrant to simplify tasks and making the repeatable as well as sandboxed.


## Pre-Requisites
This presumes your host operating system is Windows 10, you must have Virtualbox installed:

[VirtualBox](https://www.virtualbox.org/wiki/Downloads)

You then either install Vagrant for Windows:

[Vagrant](https://www.vagrantup.com/downloads)

Installing vagrant for windows will limit the use or provisioners such as ansible.

### WSL
An alternative however is to install WSL giving a working version of ubuntu then installing vagrant in this:

```
apt install -y vagrant
```

You must then ensure you have the following environment variables set:

```
export VAGRANT_WSL_WINDOWS_ACCESS_USER_HOME_PATH=/mnt/c/vagrant
export VAGRANT_WSL_ENABLE_WINDOWS_ACCESS=1
export PATH="$PATH:/mnt/c/Windows/System32/WindowsPowerShell/v1.0:/mnt/c/Program Files/Oracle/VirtualBox:/mnt/c/Windows/System32"
```

With a folder created at C:\vagrant
If you get errors spinning windows machines up then you may need the winrm gem installing

```
gem install -r winrm
```

### Basic Box Downloads
Throughout these tutorials the following boxes are required:

```
vagrant box add ubuntu/focal64
```

## Contents

[Introduction](introduction/README.md)

[Task 1](task1/README.md)

[Task 2](task2/README.md)

[Task 3](task3/README.md)
