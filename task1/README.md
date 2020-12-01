# Task 1
In this task we will be initialising a very simple vagrant environment, checking the configuration and then destroying it.

```
#Create a new folder to contain our environment
mkdir rambo
cd rambo

#Initialise the configuration
vagrant init ubuntu/focal64

ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.

#What has been created?
ls -l
```

As stated we are now ready to bring the guest up

```
vagrant up

Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'ubuntu/focal64'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'ubuntu/focal64' version '20201016.0.0' is up to date...
==> default: A newer version of the box 'ubuntu/focal64' for provider 'virtualbox' is
==> default: available! You currently have version '20201016.0.0'. The latest is version
==> default: '20201126.0.0'. Run `vagrant box update` to update.
==> default: Setting the name of the VM: solution_default_1606852543516_87949
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: SSH auth method: private key
    default: Warning: Connection reset. Retrying...
    default: Warning: Connection aborted. Retrying...
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default:
    default: Guest Additions Version: 6.1.10
    default: VirtualBox Version: 6.0
==> default: Mounting shared folders...
```

**What is happening in the above?**

**What is in VirtualBox now?**

**What name does the guest have?**

**What network configuration does the guest have?**

Now lets gain access to the guest and run some commands

```
#Lets drop into a shell on the guest
vagrant ssh

Welcome to Ubuntu 20.04.1 LTS (GNU/Linux 5.4.0-51-generic x86_64)

#We are now in the guest
```

**What user are we running on?**

**Can we elevate to root?**

Now we are on the guest lets see how much memory we have and how many cpus we have

```
free -m

              total        used        free      shared  buff/cache   available
Mem:            981         129         583           0         268         703
Swap:             0           0           0

cat /proc/cpuinfo

processor       : 0
vendor_id       : GenuineIntel

```

Lets now bring the guest down and edit some
 of the guest virtual settings such as memory and CPU.

```
vagrant halt
==> default: Attempting graceful shutdown of VM...
```

Not edit the Vagrantfile with your favourite editor

```
nano Vagrantfile

#Add the following ensuring it is below "Vagrant.configure("2") do |config|" but above "end"

  config.vm.provider "virtualbox" do |vb|
     vb.cpus=2
     vb.memory = "2048"
  end

#Bring the guest up

vagrant up
```

**Whats does the above configuration do?**

**Check in VirtualBox has anything changed?**

**Rather than doing an up and down what other vagrant command could we have done?**



