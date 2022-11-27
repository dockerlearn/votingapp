# Votingapp

This is a simple votingapp using Redis db as backend.

Vagrantfile created will install minikube on ubuntu 18 on which voting app and redis will be running as Pods having pvc to data persistance.



## Options to Install Vagrant and run script

1. In Ubuntu/Debain

You can directly run shell script vagrantinstall.sh to install virtualbox, vagrant on ubuntu/debian and then `vagrant up` command will bring up the app.

2. On Windows.

You can run vagrantinstall.ps1 on powershell with Adminstrator privalages. This will install virtualbox and vagrant for you and will run `vagrant up` command

3. Manually installing Vagrant and virtualbox

You can manually install virtualbox and vagrant on your system and run vagrant up command in votingapp folder by git clone.

## Manual installation

### Ubuntu

sudo apt update
sudo apt upgrade
sudo apt install virtualbox
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant

### On windows 

Download virtualbox using below link

https://download.virtualbox.org/virtualbox/6.1.40/VirtualBox-6.1.40-154048-Win.exe

Download vagrant

https://releases.hashicorp.com/vagrant/2.3.3/vagrant_2.3.3_windows_amd64.msi


Once Vagrant is installed run below command, assuming git is installed.

```
git clone https://github.com/dockerlearn/votingapp.git
cd votingapp
vagrant up
```
Once installed you can go vagrant ssh and use curl on the ip to check

>**Note!** To open votingapp on local laptop. Run below command:
>`ssh -fN -L 8081:192.168.49.2:32005 -i .vagrant/machines/default/virtualbox/private_key vagrant@127.0.0.1 -p 2222`
>This will create a reverseproxy through ssh.
>1. 192.168.49.2:32005 -> is the IP URL of the votingservice you get at end thorugh sudo minikube service azure-vote-front --url command
>2. .vagrant/machines/default/virtualbox/private_key -> Path to private_key for virtualbox

> **Note!**: CodeQL is installed on github to perform vulnerability check on PR creation.




