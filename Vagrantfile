# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y nginx
    systemctl status nginx
  SHELL
  

  config.vm.define "nginx_pedro" do |nginx_pedro|
    # Configuración de la maquina y la red 
    nginx_pedro.vm.box = "debian/bookworm64"
    nginx_pedro.vm.network "private_network", ip: "192.168.57.103"
  
    # Instalar Nginx, Git y vsftpd
    nginx_pedro.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y nginx git vsftpd
    SHELL

    # Configuración de nginx y ftp
    # nginx_pedro.vm.provision "shell", inline: <<-SHELL
    #   mkdir -p /var/www/nginx_pedro/html
    #   git clone https://github.com/cloudacademy/static-website-example /var/www/nginx_pedro/html
    #   chown -R www-data:www-data /vavr/www/nginx_pedro/html
    #   chmod -R 755 /var/www/nginx_pedro

    #   cp -v /vagrant/nginx_pedro /etc/nginx/sites-available/nginx_pedro
    #   ln -s /etc/nginx/sites-available/nginx_pedro /etc/nginx/sites-enabled/
    #   cp -v /vagrant/hosts /etc/hosts

    #   mkdir /home/pedro/ftp
    #   openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt -subj "/"
    #   cp -v /vagrant/vsftpd.conf /etc/vsftpd.conf

    #   systemctl restart nginx
    #   systemctl restart vsftpd 
    #   systemctl status nginx
    # SHELL
  end # nginx_pedro
end
