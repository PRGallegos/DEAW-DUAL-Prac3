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

      mkdir -p /var/www/nginx_pedro/html
      cp -vr /vagrant/nginx_pedro /var/www/nginx_pedro/html
      git clone https://github.com/cloudacademy/static-website-example /var/www/nginx_pedro/html
      chown -R www-data:www-data /var/www/nginx_pedro/html
      chmod -R 755 /var/www/nginx_pedro

      cp -v /vagrant/nginx_pedro /etc/nginx/sites-available/nginx_pedro
      ln -s /etc/nginx/sites-available/nombre_web /etc/nginx/sites-enabled/
      cp -v /vagrant/hosts /etc/hosts

      mkdir /home/nombre_usuario/ftp
      openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt -subj "/"
      cp -v /vagrant/vsftpd.conf /etc/vsftpd.conf

      systemctl restart nginx
      systemctl restart vsftpd 
      systemctl status nginx
      SHELL

  end # vm nginx_pedro

end
