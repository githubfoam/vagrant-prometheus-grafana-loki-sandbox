# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

# Minikube
KUBERNETES_VERSION = ENV['KUBERNETES_VERSION'] || "1.16.3" # OK
# X Exiting due to GUEST_MISSING_CONNTRACK: Sorry, Kubernetes 1.24.3 requires conntrack to be installed in root's path
# KUBERNETES_VERSION = ENV['KUBERNETES_VERSION'] || "1.24.3" # NOT OK conntrack missing

$ubuntu_docker_script = <<-SCRIPT

echo "current user is $(whoami)"
echo "current directory is $(pwd)"

# vg-pro-gr-lk01: Package 'docker.io' is not installed, so not removed
# vg-pro-gr-lk01: E: Unable to locate package docker
# vg-pro-gr-lk01: E: Unable to locate package docker-engine
# The SSH command responded with a non-zero exit status. Vagrant
# assumes that this means the command failed. The output for this command
# should be in the log above. Please read the output to determine what
# went wrong.
# Uninstall old versions
# apt-get remove docker docker-engine docker.io containerd runc -y

# Set up the repository
apt-get update -y
apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release -y

# Add Docker’s official GPG key    
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# set up the stable repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


# Install Docker Engine
apt-get update -y
apt-get install \
    docker-ce \
    docker-ce-cli \
    containerd.io -y


docker --version

# Verify that Docker Engine is installed correctly     
docker run hello-world

# Post-installation steps for Linux
# Manage Docker as a non-root user

# Create the docker group
groupadd docker
# Add your user to the docker group
# usermod -aG docker $USER # by default run by root
usermod -aG docker vagrant

#docker compose
apt-get install docker-compose

SCRIPT



Vagrant.configure("2") do |config|

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "1024"
    vb.cpus = 2

  end


    
    config.vm.define "vg-pro-gr-lk-02" do |kalicluster|
      # https://app.vagrantup.com/ubuntu/boxes/hirsute64
      # kalicluster.vm.box = "ubuntu/hirsute64" #21.04
      # https://app.vagrantup.com/ubuntu/boxes/impish64
      # kalicluster.vm.box = "ubuntu/impish64" #21.10
      # https://app.vagrantup.com/ubuntu/boxes/focal64
      # kalicluster.vm.box = "ubuntu/focal64" #Official Ubuntu 20.04 LTS (Focal Fossa) builds      
      # https://app.vagrantup.com/ubuntu/boxes/xenial64
      kalicluster.vm.box = "ubuntu/bionic64" #18.04      
      # https://app.vagrantup.com/ubuntu/boxes/xenial64
      # kalicluster.vm.box = "ubuntu/xenial64" #16.04
      kalicluster.vm.hostname = "vg-pro-gr-lk-02"
      #bridged network,DHCP disabled, manual IP assignment                  
      # kalicluster.vm.network "public_network", ip: "10.10.8.67"
      #bridged network,DHCP enabled,auto IP assignment
      # kalicluster.vm.network "public_network"
      kalicluster.vm.network "private_network", ip: "192.168.53.9"
      # kalicluster.vm.network "forwarded_port", guest: 80, host: 81
      #Disabling the default /vagrant share can be done as follows:
      # kalicluster.vm.synced_folder ".", "/vagrant", disabled: true
      kalicluster.vm.provider "virtualbox" do |vb|
          vb.name = "vbox-pro-gr-lk-02"
          vb.cpus = 2
          vb.memory = 2048          
          vb.gui = false       
      end
      kalicluster.vm.provision "shell", inline: $ubuntu_docker_script
    end


end

