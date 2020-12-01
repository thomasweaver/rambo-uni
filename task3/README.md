# Task 3
In this task we are ramping up the complexity and moving to having more than one guest in our configuration.

Create a new folder rambo-t3

```
vagrant init ubuntu/focal64
```

All the config we defined in the previous tasks inside the config variable scope we are going to move into a new variable scope.

Remove everything between the "|config|" and end tags

Now add the following, notice it is just the same config as used previously

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

Now lets start up our guest

```
vagrant up
```

**How does the output differ to the previous tasks?**

Do your curl command from your host again

```
curl http://127.0.0.1:8080
```

**What was the output?**

Now lets add another guest to our config, open up the Vagrantfile with your text editor.

Below the config you added above now add the following config

```
config.vm.define "web2" do |web2|
    web2.vm.box = "ubuntu/focal64"
    web2.vm.network "forwarded_port", guest: 80, host: 8081
    web2.vm.provider "virtualbox" do |vb|
        vb.cpus=2
        vb.memory="1024"
    end

    web2.vm.provision "shell", inline: <<-SHELL
        apt update
        apt install -y apache2
    SHELL
end
```

Now bring the host up

```
vagrant up
```

**How does the output differ?**

Lets look at vagrant status

```
vagrant status

Current machine states:

web                       running (virtualbox)
web2                      running (virtualbox)
```

**Whats different to previously?**

Now perform a curl on your host but this time on port 8081

```
curl http://127.0.0.1:8081
```

**Why are we using a different port?**

Lets SSH to web one and try and ping web2

```
#Fist SSH to web 2 to get its address

vagrant ssh web2

#Get the IP Address

ip addr

2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 02:98:fc:89:17:a5 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic enp0s3
       valid_lft 86190sec preferred_lft 86190sec
    inet6 fe80::98:fcff:fe89:17a5/64 scope link
       valid_lft forever preferred_lft forever
```

Exit back to the host and SSH into web 

```
vagrant ssh web

#Get the IP address

ip addr

2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 02:98:fc:89:17:a5 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic enp0s3
       valid_lft 85699sec preferred_lft 85699sec
    inet6 fe80::98:fcff:fe89:17a5/64 scope link
       valid_lft forever preferred_lft forever

#Ping web2

ping 10.0.2.15

#curl for page on 8081
curl http://127.0.0.1:8081
```

**Was the ping successfull?**

**Why was the ping successfull or not?**

**Was the curl successfull?**

**Why was the curl successfull or not?**


Lets destroy our guests

```
vagrant destroy -f
```

**How does this output differ?**
