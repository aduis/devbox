# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "4096"
  end
  
  config.vm.provision "file", source: "~/.vimrc", destination: "vimrc"

  config.vm.provision "shell", inline: <<-SHELL
    #update apt-get
    apt-get update

    #install core depencencies
    apt-get install -y curl git mercurial make binutils bison gcc build-essential

    #install docker-ce
    apt-get install -y linux-image-extra-$(uname -r) linux-image-extra-virtual
    apt-get install -y apt-transport-https ca-certificates software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    apt-get update
    apt-get install -y docker-ce 
    
    #install docker-compose
    curl -L --fail https://github.com/docker/compose/releases/download/1.13.0/run.sh > /usr/local/bin/docker-compose 
    chmod +x /usr/local/bin/docker-compose

    #install golang 
    gvm install go1.4 -B
    gvm use go1.4
    export GOROOT_BOOTSTRAP=$GOROOT
    gvm install go1.8.1
    gvm use go1.8.1 --default

  SHELL
end
