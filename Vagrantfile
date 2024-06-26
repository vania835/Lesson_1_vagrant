# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Указываем box с Ubuntu 20.04
  config.vm.box = "ubuntu/jammy64"
  
  # Настраиваем имя виртуальной машины
  config.vm.define "Бубунта"
  
  # Настраиваем сетевую конфигурацию
  config.vm.network "public_network", ip: "192.168.0.21"
  
  # Указываем путь к вашему локальному приватному ключу
  private_key_path = "C:/key"

  # Конфигурируем SSH 
  config.ssh.private_key_path = private_key_path
  config.ssh.insert_key = false  # Отключаем вставку нового ключа Vagrant
  
  # Настройка виртуальной машины
  config.vm.provider "virtualbox" do |vb|
    # Выделяем память и процессоры для виртуальной машины
    vb.memory = "1024"
    vb.cpus = 2
  end

  # Проброс приватного ключа в нужную папку на виртуальной машине
  config.vm.provision "file", source: "#{private_key_path}", destination: "/home/vagrant/.ssh/id_rsa"
  
  # Настройка прав доступа для ключа и установка Apache
  config.vm.provision "shell", inline: <<-SHELL
    chmod 600 /home/vagrant/.ssh/id_rsa
    chown vagrant:vagrant /home/vagrant/.ssh/id_rsa

    # Установка Apache
    apt-get update
    apt-get install -y apache2
  SHELL

  # Включаем ансибл для управления конфигурацией
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
