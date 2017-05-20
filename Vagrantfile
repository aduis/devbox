# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.synced_folder "../data", "/vagrant_data"

  #core depencies
  config.vm.provision :shell, inline: "apt-get update"
  config.vm.provision :shell, inline: "apt-get -y install curl wget git mercurial make binutils bison gcc build-essential"   

  #Vundle
  config.vm.provision "file", source: "vimrc", destination: ".vimrc"
  config.vm.provision :shell, privileged: false, inline: <<-SHELL
    rm -rf /home/vagrant/.vim/bundle/Vundle.vim 
    git clone https://github.com/VundleVim/Vundle.vim.git /home/vagrant/.vim/bundle/Vundle.vim
  SHELL

  #Oh My ZSH    
  config.vm.provision :shell, inline: <<-SHELL 
    apt-get -y install zsh    
    git clone git://github.com/robbyrussell/oh-my-zsh.git /home/vagrant/.oh-my-zsh-temp
    mv /home/vagrant/.oh-my-zsh-temp /home/vagrant/.oh-my-zsh 
    rm -rf /home/vagrant/.oh-my-zsh-temp
    cp /home/vagrant/.oh-my-zsh/templates/zshrc.zsh-template /home/vagrant/.zshrc    
    chsh -s /bin/zsh vagrant    
  SHELL

  #Docker-ce    
  config.vm.provision "shell", inline: <<-SHELL
    apt-get install -y linux-image-extra-$(uname -r) linux-image-extra-virtual
    apt-get install -y apt-transport-https ca-certificates software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    apt-get update
    apt-get install -y docker-ce 
  SHELL

  #Docker-compose
  config.vm.provision "shell", inline: <<-SHELL
    curl -L --fail https://github.com/docker/compose/releases/download/1.13.0/run.sh > /usr/local/bin/docker-compose 
    chmod +x /usr/local/bin/docker-compose
  SHELL
    
  #Golang
  config.vm.provision "shell", inline: <<-SHELL
    wget -q https://storage.googleapis.com/golang/go1.7.4.linux-amd64.tar.gz
    tar -xvf go1.7.4.linux-amd64.tar.gz
    rm -rf /usr/local/go
    mv -f go /usr/local
    export GOROOT=/usr/local/go
    export GOPATH=$HOME/src
    export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
    echo "export GOROOT=/usr/local/go" >> /home/vagrant/.zshrc
    echo "export GOPATH=$HOME/src" >> /home/vagrant/.zshrc
    echo "PATH=$GOPATH/bin:$GOROOT/bin:$PATH" >> /home/vagrant/.zshrc
  SHELL
end
