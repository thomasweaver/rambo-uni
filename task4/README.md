# Task 4
In this task we will be using the shared folders to transfer files between the host and guest

Create a new folder rambo-t4

Initialize vagrant

```
vagrant init ubuntu/focal64
```

Now edit the Vagrantfile and add the following config:

```
config.vm.define "web" do |web|
    web.vm.box = "ubuntu/focal64"
    web.vm.network "forwarded_port", guest: 80, host: 8080
    web.vm.provider "virtualbox" do |vb|
        vb.cpus=2
        vb.memory="2048"
    end

    web.vm.provision "shell", inline: <<-SHELL
        apt update
        apt install -y apache2
    SHELL
end
```

Bring the guest up

```
vagrant up
```

Now lets curl our page again and see our response

```
curl http://127.0.0.1:8080
```

Now lets download our website. On your host machine run:

```
wget https://github.com/thomasweaver/rambo-uni/raw/main/task4/solution/html.tar.gz
```

We can now use a provisioner to provision this website to our guest

Add the following to your Shell script provisioner in your Vagrantfile:

```
tar -xzf /vagrant/html.tar.gz -C /var/www/html/
```

Your config should now look like this

```
    web.vm.provision "shell", inline: <<-SHELL
        apt update
        apt install -y apache2
	tar -xzf /vagrant/html.tar.gz -C /var/www/html/
    SHELL
```

Now provision the box again

```
vagrant reload --provision


curl http://127.0.0.1:8080
```

**What have we just done?**

**What does the output look like now?**

**What does it look like in a browser?**

We now want to get the apache logs off our server.
SSH to the box and copy the logs

```
vagrant ssh

sudo -s

cp -R /var/log/apache2 /vagrant/.

exit
exit
```

In your host now look to see what is in your folder

```
ls -l 
ls -l apache2/
```

**What have we just done?**

**What can you see in the apache2 logs?**

Lets destroy what we have done

```
vagrant destroy -f
```
