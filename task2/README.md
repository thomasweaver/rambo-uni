# Task 2
In this task we will look at provisioning and basic networking

Using the same folder as previosly (rambo)

```
# Check the status of our guest
vagrant status

Current machine states:

default                   running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.
```

Open up the configuration file with your favourite editor
We are now going to add a simple inline shell provisioner

The configuration format is:

```
config.vm.provision "shell", inline: <<-SHELL
	#SHELL SCRIPT HERE
SHELL
```

So lets add a simple script that installs apache2, we must not forget that this is a none interactive shell 

```
config.vm.provision "shell", inline: <<-SHELL
        apt update
	apt install -y apache2
SHELL
```

We can now reload the guest but we need to tell vagrant to run the provisioner on reload

```
vagrant reload --provision
```

**What does the output show?**

**What has happened?**

Lets now ssh into the guest and check to see if apache2 is installed

```
vagrant ssh

curl http://127.0.0.1
```

**What response do you get?**

Now exit the guest by typing exit until you're back to the host prompt.

Perform the same curl on your host

```
curl http://127.0.0.1
```

**Do you get the same response?**

**If not why not?**

Now we can port forward to the guest to ensure we do get the correct response on the host.

Open up your Vagrantfile in your favourite editor and add the following

```
config.vm.network "forwarded_port", guest: 80, host: 8080

We can now reload the guest. Notice this time we do not need to run the provisioner

```
vagrant reload
```

**What have we just done?**

**What does the Networking look like in VirtualBox? Hint. Look at the forwarded ports**

Now do the curl command on your host.

```
curl http://127.0.0.1:8080
```

**What response did you get?**

Now destroy your guest without confirmation

```
vagrant destroy -f
```

