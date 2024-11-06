# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y nginx
    systemctl status nginx
  SHELL
  

  config.vm.define "nginx_pedro" do |nginx_pedro|
    nginx_pedro.vm.box = "debian/bookworm64"
    nginx_pedro.vm.network "private_network", ip: "192.168.57.103"
  
    nginx_pedro.vm.provision "shell", inline: <<-SHELL

      cp -vr /vagrant/nginx_pedro /var/www/nginx_pedro/html
      chown -R www-data:www-data /var/www/nginx_pedro/html
      chmod -R 755 /var/www/nginx_pedro

      cp -v /vagrant/sites-available-nginx_pedro /etc/nginx/sites-available/nginx_pedro
      cp -v /vagrant/sites-available-nginx_pedro /etc/nginx/sites-enabled/nginx_pedro
      cp -v /vagrant/hosts /etc/hosts

      systemctl restart nginx
    SHELL

  end # vm nginx_pedro

end
