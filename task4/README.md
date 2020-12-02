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


