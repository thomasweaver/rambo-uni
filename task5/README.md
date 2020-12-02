# Task 5
In this task we will look at creating a more complicated provision scipt by deploying a LAMP stack and Wordpress.

Create a new folder, initialise vagrant and add the config

```
mkdir rambo-t5

vagrant init ubuntu/focal64
```

Add the following config to your Vagrantfile

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
        export DEBIAN_FRONTEND=noninteractive
        apt install -y apache2 php libapache2-mod-php mysql-server php-mysql

        cat <<EOT >> /etc/apache2/sites-available/wordpress.conf
Alias /blog /usr/share/wordpress
<Directory /usr/share/wordpress>
    Options FollowSymLinks
    AllowOverride Limit Options FileInfo
    DirectoryIndex index.php
    Order allow,deny
    Allow from all
</Directory>
<Directory /usr/share/wordpress/wp-content>
    Options FollowSymLinks
    Order allow,deny
    Allow from all
</Directory>
EOT
        a2ensite wordpress
        systemctl reload apache2
        mysql -u root <<EOT
CREATE DATABASE wordpress;
CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'password1';
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER ON wordpress.* TO 'wordpress'@'localhost';
FLUSH PRIVILEGES;
EOT

        cd /tmp
        wget https://en-gb.wordpress.org/latest-en_GB.tar.gz
        tar -xzf latest-en_GB.tar.gz
        mv wordpress /usr/share/.

        mv /usr/share/wordpress/wp-config-sample.php /usr/share/wordpress/wp-config.php
        sed -i "s/put your unique phrase here/$(date | md5sum | cut -d' ' -f 1)/g" /usr/share/wordpress/wp-config.php
        sed -i "s/database_name_here/wordpress/" /usr/share/wordpress/wp-config.php
        sed -i "s/username_here/wordpress/" /usr/share/wordpress/wp-config.php
        sed -i "s/password_here/password1/" /usr/share/wordpress/wp-config.php

        systemctl restart apache2

    SHELL

end
```

Use your browser and naviagte to http://127.0.0.1:8080

**What do you see?**

**What does the provision script do?**

Destroy the guest

```
vagrant destroy -f
```
