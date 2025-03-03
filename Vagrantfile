# -*- mode: ruby -*-
# vi: set ft=ruby :
# by https://github.com/ahmedsmaali5
# make sure to replace NET_RANGE to fit your private network range
# change ansible user and password

NET_RANGE = "192.168.56"
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "centos/8"
  config.vm.provider :virtualbox do |v|
    v.memory = 1024
    v.linked_clone = true
  end

  config.vm.provision "shell", inline: <<-SHELL
    #!/bin/bash
    USER="ansible"
    PASSWORD="ansible"
    sudo useradd -m -s /bin/bash "$USER"
    echo "$USER:$PASSWORD" | sudo chpasswd
    sudo usermod -aG wheel "$USER"
    echo "$USER ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/$USER
    sudo chmod 440 /etc/sudoers.d/$USER

    echo "User '$USER' created and configured successfully."

    #delete all line with PasswordAuthentication
    sudo sed -i '/^PasswordAuthentication/d' /etc/ssh/sshd_config

    echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config

    sudo systemctl restart sshd

    echo "Password authentication enabled, default username/password: ansible/ansible"

  SHELL


  # 1st node
  config.vm.define "ansible1" do |app|
    app.vm.hostname = "ansible1"
    app.vm.network :private_network, ip: NET_RANGE+".10"
  end

  # 2nd node
  config.vm.define "ansible2" do |app|
    app.vm.hostname = "ansible2"
    app.vm.network :private_network, ip: NET_RANGE+".11"
  end

end