Vagrant.configure("2") do |config|
  config.vm.define "pedro" do |jn|
    pedro.vm.box = "debian/bookworm64"
    pedro.vm.network "private_network", ip: "192.168.57.103"

    pedro.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y nginx vsftpd git

      # Crear directorio para el proyecto
      mkdir -p /var/www/pedro/html
      git clone https://github.com/cloudacademy/static-website-example /var/www/pedro/html
      chown -R www-data:www-data /var/www/pedro/html
      chmod -R 755 /var/www/pedro

      # Configurar directorio
      cp -v /vagrant/pedro.conf /etc/nginx/sites-available/pedro.conf
      ln -s /etc/nginx/sites-available/pedro.conf /etc/nginx/sites-enabled/
      cp -v /vagrant/hosts /etc/hosts

      # Crear usuario y dar permisos
      useradd -m pedro
      echo "pedro:pedro" | chpasswd
      mkdir /home/pedro/ftp
      chown -R pedro:www-data /home/pedro/ftp
      chmod -R 755 /home/pedro/ftp

      # Configurar FTP
      mkdir -p /home/pedro/ftp
      openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt -subj "/"
      cp -v /vagrant/vsftpd.conf /etc/vsftpd.conf

      # Reiniciar servicios
      systemctl restart nginx
      systemctl restart vsftpd 
      systemctl status nginx 
    SHELL
  end # pedro 
end