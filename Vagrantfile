# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :
# Box / OS
VAGRANT_BOX = 'ubuntu/bionic64'
# Memorable name for your
VM_NAME = 'vagrant'
# VM User — 'vagrant' by default
VM_USER = 'vagrant'
# Username on your Mac
#MAC_USER = 'shrey'
# Host folder to sync
#HOST_PATH = '/Users/' + MAC_USER + '/' + VM_NAME
#HOST_PATH = 'C:\Users\shrey\OneDrive\Documents\vagrant'
# Where to sync to on Guest — 'vagrant' is the default user name
#GUEST_PATH = '/home/' + VM_USER + '/' + VM_NAME
# # VM Port — uncomment this to use NAT instead of DHCP
 VM_PORT = 8080
Vagrant.configure(2) do |config|
  # Vagrant box from Hashicorp
  config.vm.box = VAGRANT_BOX
  
  # Actual machine name
  config.vm.hostname = VM_NAME
  # Set VM name in Virtualbox
  config.vm.provider "virtualbox" do |v|
    v.name = VM_NAME
    v.memory = 2048
  end
  #DHCP — comment this out if planning on using NAT instead
  #config.vm.network "private_network", type: "dhcp"
  # # Port forwarding — uncomment this to use NAT instead of DHCP
  config.vm.network "forwarded_port", guest: 80, host: VM_PORT
  # Sync folder
  #config.vm.synced_folder HOST_PATH, GUEST_PATH
  # Disable default Vagrant folder, use a unique path per project
  #config.vm.synced_folder '.', '/home/'+VM_USER+'', disabled: true
  # Install Git, Node.js 6.x.x, Latest npm
  config.vm.provision "shell", inline: <<-SHELL
    apt-get install -y git
	sudo apt update -y
	sudo apt upgrade -y
    sudo apt-get install curl apt-transport-https ca-certificates software-properties-common -y
  SHELL
  config.vm.provision "shell", inline: $vagrant, privileged: false  
end

$vagrant = <<SCRIPT
#!/bin/bash
#curl -fsSL https://apt.dockerproject.org/gpg | sudo apt-key add -
#sudo apt-add-repository "deb https://apt.dockerproject.org/repo ubuntu-xenial main"
#sudo apt-get install -y docker-engine=17.03.1~ce-0~ubuntu-xenial
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get -y update
sudo apt-get install -y docker-ce
sudo systemctl start docker
sudo usermod -a -G docker vagrant
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo cp minikube-linux-amd64 /usr/local/bin/minikube
sudo chmod 755 /usr/local/bin/minikube
minikube version
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version -o json
#Setup minikube
#echo "127.0.0.1 minikube minikube." | sudo tee -a /etc/hosts
sudo mkdir -p $HOME/.minikube
sudo mkdir -p $HOME/.kube
sudo touch $HOME/.kube/config
sudo export KUBECONFIG=$HOME/.kube/config
sudo minikube start --force
sudo minikube addons enable metrics-server
wget https://get.helm.sh/helm-v3.9.3-linux-amd64.tar.gz
tar xvf helm-v3.9.3-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin
rm helm-v3.9.3-linux-amd64.tar.gz
rm -rf linux-amd64
sudo chown -R vagrant:vagrant $HOME/.kube
sudo chown -R vagrant:vagrant $HOME/.minikube
helm repo add my-repo https://charts.bitnami.com/bitnami
helm install my-release my-repo/redis --set architecture=standalone
git clone https://github.com/dockerlearn/votingapp
cd votingapp
kubectl apply -f votebank.yml
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=1 --max=5 
sudo minikube service azure-vote-front --url
SCRIPT
