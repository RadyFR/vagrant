Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/bionic64"  # Usa Ubuntu 18.04 como sistema base
  
    config.vm.network "private_network", ip: "192.168.33.10"  # IP estática
    
    config.vm.provider "virtualbox" do |vb|
      vb.memory = "512"  # Asignar 512 MB de RAM
      vb.cpus = 1         # Asignar 1 CPU
    end
    
    config.vm.synced_folder "./html", "/var/www/html"  # Compartir directorio local con Apache
    
    config.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      sudo systemctl enable apache2
      sudo systemctl restart apache2
    SHELL
  end
  