# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "generic/ubuntu2004"
  config.vm.network :private_network, ip: '172.42.42.115'
  config.vbguest.auto_update = true
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 1
  config.vm.provision "shell", inline: <<-SHELL
    apt -qq install fish -y 2>&1 /tmp/fish.log
    curl -s -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" 2>&1 /tmp/kubectl.log
    chmod 777 kubectl
    mv /root/kubectl /usr/local/bin/
    curl -fsSL https://get.docker.com/ | sh 2>&1 /tmp/docker.log
    wget https://golang.org/dl/go1.16.6.linux-amd64.tar.gz 2>&1 /tmp/golang.log
    tar zxf go1.16.6.linux-amd64.tar.gz -C /usr/local/ 2>&1 /tmp/tar.log
    export PATH=$PATH:/usr/local/go/bin/
    GO111MODULE="on" go get sigs.k8s.io/kind@v0.11.1 2>&1 /tmp/gobuild.log
    mv /root/go/bin/kind /usr/local/bin/
    kind version 2>&1 /tmp/kind.log
    kind create cluster 2>&1 /tmp/cluster.log
    kubectl cluster-info 2>&1 /tmp/kubectl.log
    apt -qq install zsh -y
    apt -qq install curl wget git -y
    sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" 2>&1 /tmp/ohmyzsh.log
  SHELL
  end
end
