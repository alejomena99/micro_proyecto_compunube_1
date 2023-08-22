# -*- mode: ruby -*-
# vi: set ft=ruby :

$install_puppet = <<-PUPPET
sudo apt-get update -y
sudo apt-get install -y puppet
PUPPET

Vagrant.configure("2") do |config|

    if Vagrant.has_plugin? "vagrant-vbguest"
      config.vbguest.no_install  = true
      config.vbguest.auto_update = false
      config.vbguest.no_remote   = true
    end

    config.vm.define :webProject1 do |webProject1|
      webProject1.vm.box = "bento/ubuntu-22.04"
      webProject1.vm.network :private_network, ip: "193.168.100.1"
      webProject1.vm.hostname = "webProject1"
      webProject1.vm.boot_timeout = 800
      webProject1.vm.synced_folder "D:/Vagrant/synchronized_folder_web1", "/home/vagrant/synchronized_folder_web1"
  
      webProject1.vm.provision "shell", inline: $install_puppet
      webProject1.vm.provision :puppet do |puppet|
        puppet.manifests_path = "puppet/manifests"
        puppet.manifest_file = "site.pp"
        puppet.module_path = "puppet/modules"
      end

      webProject1.vm.provision "shell", inline: <<-SHELL
        echo 'export PRIVATE_IP=193.168.100.1' >> /home/vagrant/.bashrc
        echo 'export MICROSERVICE_NAME=my_microservice_1' >> /home/vagrant/.bashrc
      SHELL

    end

    config.vm.define :webProject2 do |webProject2|
      webProject2.vm.box = "bento/ubuntu-22.04"
      webProject2.vm.network :private_network, ip: "193.168.100.2"
      webProject2.vm.hostname = "webProject2"
      webProject2.vm.boot_timeout = 800
      webProject2.vm.synced_folder "D:/Vagrant/synchronized_folder_web2", "/home/vagrant/synchronized_folder_web2"

      webProject2.vm.provision "shell", inline: $install_puppet
      webProject2.vm.provision :puppet do |puppet|
        puppet.manifests_path = "puppet/manifests"
        puppet.manifest_file = "site.pp"
        puppet.module_path = "puppet/modules"
      end

      webProject2.vm.provision "shell", inline: <<-SHELL
        echo 'export PRIVATE_IP=193.168.100.2' >> /home/vagrant/.bashrc
        echo 'export MICROSERVICE_NAME=my_microservice_2' >> /home/vagrant/.bashrc
      SHELL

    end
end
  