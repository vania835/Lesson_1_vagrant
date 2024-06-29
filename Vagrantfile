# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Указываем провайдер
  config.vm.provider "virtualbox" do |vb|
    vb.name = "ubuntu-2204-test"
    vb.memory = "1024"
    vb.cpus = 2
  end
  config.vm.define "Bubunta"
  # Указываем базовый образ
  config.vm.box = "ubuntu/jammy64"  
  config.vm.box_version = "20240627.0.0"

  # Настраиваем сетевой мост
  config.vm.network "public_network", ip: "192.168.0.21"

  # Настраиваем доступ по SSH
  config.ssh.insert_key = false

  config.vm.provision "shell", inline: <<-SHELL
    echo 'root:1q2w3e4R' | sudo chpasswd
  # Разрешаем указывание пароля root-пользователя
   sudo sed -i 's/^#PermitRootLogin prohibit-password/PermitRootLogin prohibit-password/' /etc/ssh/sshd_config
   sudo sed -i 's/#AuthorizedKeysFile/AuthorizedKeysFile/' /etc/ssh/sshd_config
   sudo sed -i 's/authorized_keys2/id_rsa.pub/' /etc/ssh/sshd_config
   sudo sed -i 's/#PubkeyAuthentication/PubkeyAuthentication/' /etc/ssh/sshd_config
   sudo echo 'RSAAuthentication yes' >> /etc/ssh/sshd_config
   sudo mkdir -p /home/vagrant/.ssh
  # Перезапускаем SSH
   sudo systemctl restart sshd
  SHELL

  # Указываем ключ
  config.vm.provision "file", source: "C:/key/id_rsa.pub", destination: "/home/vagrant/.ssh/id_rsa.pub"
  config.vm.provision "shell", inline: <<-SHELL
    sudo mkdir -p /root/.ssh
    sudo cp /home/vagrant/.ssh/id_rsa.pub /root/.ssh/id_rsa.pub
    sudo chown root:root /root/.ssh/id_rsa.pub
    sudo chmod 600 /root/.ssh/id_rsa.pub
    
    # Удаляем ключ из /home/vagrant/.ssh
    sudo rm /home/vagrant/.ssh/id_rsa.pub
    
  SHELL

  # Устанавливаем обновления и Apache
  config.vm.provision "shell", inline: <<-SHELL
    #sudo apt-get update
    sudo apt-get install -y apache2
  SHELL

  # Проброс порта 80
  config.vm.network "forwarded_port", guest: 80, host: 8080
end
