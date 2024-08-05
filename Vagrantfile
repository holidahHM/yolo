Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.network "private_network", type: "dhcp"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y docker.io docker-compose
    systemctl start docker
    systemctl enable docker
    usermod -aG docker vagrant
  SHELL

  config.vm.define "client" do |client|
    client.vm.network "forwarded_port", guest: 3000, host: 3000
    client.vm.network "forwarded_port", guest: 22, host: 2222
    client.vm.synced_folder ".", "/vagrant", type: "virtualbox"
    client.vm.provision "shell", inline: <<-SHELL
      cd /vagrant
      docker-compose up -d client
    SHELL
  end

  config.vm.define "backend" do |backend|
    backend.vm.network "forwarded_port", guest: 5000, host: 5000
    backend.vm.network "forwarded_port", guest: 22, host: 2200
    backend.vm.synced_folder ".", "/vagrant", type: "virtualbox"
    backend.vm.provision "shell", inline: <<-SHELL
      cd /vagrant
      docker-compose up -d backend mongo
    SHELL
  end
end
