# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "./project", "/var/www/wp", owner: "www-data", group: "www-data"

 config.vm.provision "shell", inline: <<-SHELL
  
  apt-get update
  apt-get install -y apache2
  apt-get install -y mysql-server
  apt-get install -y curl php php-mysql
  apt-get install -y php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip
  

echo -e "
<VirtualHost _default_:80>
    CustomLog \${APACHE_LOG_DIR}/wordpress.log combined
    DocumentRoot /var/www/wp
    <Directory /var/www/wp>
      AllowOverride all
      Allow from all
      Options -MultiViews
      Require all granted
    </Directory>
</VirtualHost>" | sudo tee /etc/apache2/sites-available/wp.conf
  
  sudo a2dissite 000-default.conf   
  sudo a2ensite wp.conf
  a2enmod rewrite
  systemctl restart apache2
  
  mysql  -e  "CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;"
  mysql  -e  "GRANT ALL ON wordpress.* TO 'wp'@'localhost' IDENTIFIED BY 'wp';"
  mysql  -e  "FLUSH PRIVILEGES;"
  
SHELL
end
