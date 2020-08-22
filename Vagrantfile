# -*- mode: ruby -*-
# vi: set ft=ruby :
$install_docker_script = <<SCRIPT
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum -y install docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker vagrant
sudo systemctl start firewalld
sudo systemctl enable firewalld
sudo firewall-cmd --permanent --add-port=2377/tcp
sudo firewall-cmd --permanent --add-port=7946/tcp
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --add-port=7946/udp
sudo firewall-cmd --permanent --add-port=4789/udp
sudo firewall-cmd --reload
sudo systemctl restart docker
SCRIPT

#Common variables
BOX_NAME = "centos/7"
MEMORY = "1024"
CPUS = 1
MANAGERS = 2
MANAGER_IP = "172.20.20.1"
WORKERS = 4
WORKER_IP = "172.20.20.10"

Vagrant.configure("2") do |config|
  config.vm.box = BOX_NAME
  config.vm.provision "shell",inline: $install_docker_script, privileged: true
  config.vm.provider "virtualbox" do |vb|
      vb.memory = MEMORY
      vb.cpus = CPUS
  end
  #Setup Manager Nodes
  (1..MANAGERS).each do |i|
      config.vm.define "Manager0#{i}" do |manager|
          manager.vm.network :private_network, ip: "#{MANAGER_IP}#{i}"
          manager.vm.hostname = "Manager0#{i}"
      end
  end
  #Setup Woker Nodes
  (1..WORKERS).each do |i|
      config.vm.define "Worker0#{i}" do |worker|
          worker.vm.network :private_network, ip: "#{WORKER_IP}#{i}"
          worker.vm.hostname = "Worker0#{i}"
      end
  end
end
