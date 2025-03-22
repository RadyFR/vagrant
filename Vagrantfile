Vagrant.configure("2") do |config|
  # Servidor Web 1 - Apache
  config.vm.define "web1" do |web1|
    web1.vm.box = "ubuntu/bionic64"
    web1.vm.network "private_network", ip: "192.168.33.11"
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    web1.vm.synced_folder "./html", "/var/www/html"
    web1.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      sudo systemctl enable apache2
      sudo systemctl restart apache2
    SHELL
  end

  # Servidor Web 2 - Apache
  config.vm.define "web2" do |web2|
    web2.vm.box = "ubuntu/bionic64"
    web2.vm.network "private_network", ip: "192.168.33.12"
    web2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    web2.vm.synced_folder "./html", "/var/www/html"
    web2.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      sudo systemctl enable apache2
      sudo systemctl restart apache2
    SHELL
  end

  # Servidor de Balanceo - Nginx
  config.vm.define "loadbalancer" do |lb|
    lb.vm.box = "ubuntu/bionic64"
    lb.vm.network "private_network", ip: "192.168.33.10"
    lb.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    lb.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y nginx
      sudo rm /etc/nginx/sites-enabled/default
      echo 'upstream backend {
        server 192.168.33.11;
        server 192.168.33.12;
      }
      server {
        listen 80;
        location / {
          proxy_pass http://backend;
        }
      }' | sudo tee /etc/nginx/sites-available/loadbalancer
      sudo ln -s /etc/nginx/sites-available/loadbalancer /etc/nginx/sites-enabled/
      sudo systemctl restart nginx
    SHELL
  end
end