# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.box_version = "20170330.0.1"
  config.vm.box_check_update = false
  config.vbguest.auto_update = false

  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "public_network"
  config.vm.network :private_network, ip: "172.16.10.20"
  config.vm.synced_folder "./", "/vagrant", type: "virtualbox"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "v4-vm-2017"
    vb.gui = false
    vb.memory = "3076"
  end

  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  config.vm.provision "shell", inline: <<-SHELL

    ## locales
    echo "ja_JP.UTF-8 UTF-8" > /etc/locale.gen
    locale-gen ja_JP.UTF-8
    update-locale LANG=ja_JP.UTF-8

    ## timezone
    echo "Asia/Tokyo" > /etc/timezone
    dpkg-reconfigure -f noninteractive tzdata

    apt-get -y install \
      apt-transport-https \
      ca-certificates \
      curl \
      software-properties-common

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

    add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable"

    apt-get update
    apt-get -y install docker-ce=17.03.1~ce-0~ubuntu-trusty
    #groupadd docker
    usermod -aG docker vagrant
    service docker restart

    curl -Ls https://github.com/docker/compose/releases/download/1.11.2/docker-compose-`uname -s`-`uname -m` > ~/docker-compose
    mv ~/docker-compose /usr/local/bin/docker-compose
    chown root:root /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose

  SHELL
end
