# -*- mode: ruby -*-
# vi: set ft=ruby :
VM_BOX  = 'ubuntu/kinetic64'
NETWORK = 'forwarded_port'
$script = <<-SCRIPT
echo 'ubuntu:ubuntu' | sudo chpasswd
sudo apt-get update
sudo apt-get -y install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get -y install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo usermod -aG docker "vagrant" && newgrp docker
cd /home/vagrant
curl -L https://raw.githubusercontent.com/RocketChat/Docker.Official.Image/master/compose.yml -O
curl -L https://raw.githubusercontent.com/RocketChat/Docker.Official.Image/master/env.example -O
mv env.example .env
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box = VM_BOX
  config.vm.network NETWORK, guest: 3000, host: 3000
  config.vm.synced_folder "./", "/shared", owner: "vagrant"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
    vb.gui = false
  end
  config.vm.provision 'shell',  inline: $script
end
