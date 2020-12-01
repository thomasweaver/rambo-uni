# Introduction
This is not a substitute for the official Vagrant docs which can be found here and should be used as reference:

[Vagrant Docs](https://www.vagrantup.com/docs)

Vagrant gives the user the ability to describe an environment in a text file allowing infrastructure to be representing as code. 
This allows environments to be shared with people witht he knowledge that it will run the same in any environment.
All this whilst also making it repeatable.

Multiple providers can be used, weverything discussed here will be using VirtualBox as the provisioner.

## Boxes
Boxes are packed up virtual machines, they are comparable to OVA files except they also come with Vagrantfile which provides a default Vagrant configuration.
They have also been created in such a way that makes them work with vagrant such as creating an account which can be logged on with.

[More Info](https://www.vagrantup.com/docs/boxes)

```
vagrant box list
```

## Folder Structure
Vagrant is configured with a single file named "Vagrantfile" a folder which contains that file can be considered a enviornment and can be controlled with Vagrant.
Example create default vagrant config:

```
mkdir myfirstvagrant
cd myfirstvagrant
vagrant init ubuntu/focal64
cat Vagrantfile 
```

Configuration for your running guests including private keys can be found in .vagrant folder

## Provisioning
Provisioners can be defined within the Vagrantfile configuration such as a Shell script, ansible playbook, chef recipe etc.

The simpliest form of provisioner is an inline shell script which can be written directly in the Vagrantfile.

[More Info](https://www.vagrantup.com/docs/provisioning)

## Networking
As all hosts are virtual machines the networking is directly linked to the providers configurations.
By default all Vagrant hosts use NAT networking and the box is managed by forwarding a port to 22 for SSH and 5985 for WinRM

However extra NIC's can be added to the VM or extra ports can be forwarded. 

### Port Forwarding
The simplest thing is to forward a port from the host to the VM.
For example to forward port 8080 from the host to port 80 on the guest the following line can be added to the Vagrantfile

```
config.vm.network "forwarded_port", guest: 80, host: 8080
```

[More Info](https://www.vagrantup.com/docs/networking/forwarded_ports)

### Private Network
Private Networking allows the host to acces the guest but does not allow anything outside of the host to acces it.
Either static addresses or DHCP can be used:

```
Vagrant.configure("2") do |config|
  config.vm.network "private_network", type: "dhcp"
end
```

```
Vagrant.configure("2") do |config|
  config.vm.network "private_network", ip: "192.168.50.4"
end
```

### Public Network
Public network in this case mean bridged i.e. it bridges with your hosts physical connection.

```
Vagrant.configure("2") do |config|
  config.vm.network "public_network"
end
```

## Synced Folders
By default vagrant syncs the current folder that you are in with /vagrant within the guest.
For windows the shared folder can be found by going to:

```
//vboxsvr/vagrant
```

in most cases /vagrant is a VirtualBox shared folder, in some other cases the folder is only synced on boot.

## Basic commands
### Initialise a configuration
BOXNAME is the name of the box you want to be the default guest
```
vagrant init BOXNAME
```

### Box commands
Prebuilt boxes can be found at [Discover Vagrant Boxes](https://app.vagrantup.com/boxes/search).
```
vagrant box add ubuntu/trusty64 #download the ubuntu/trusty64 box
vagrant box list #list all boxes in local inventory
```

### Up commands
To bring up or build your environment as described in the Vagrantfile

```
vagrant up #don't specify a name and all guest are brought up
vagrant up server1 #specifying server1 mean only bring up the guest with that name
```

### Halt commands
Using the halt keyword enables you to stop your guests, it will first try a graceful stop and then a hard stop if that fails

```
vagrant halt #don't specify a name brings all guests down
vagrant halt server1 #specifying server1 will only bring down this guest
```

### Destroy commands
The destroy keyword can destroy a single guest or all guests.

```
vagrant destroy #destroy all guests, youw ill be prompted to confirm
vagrant destroy -f #specifying -f means your are not prompted to confirm
```
