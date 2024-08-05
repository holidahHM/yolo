Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"

  # Common configuration for all VMs
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y docker.io docker-compose
    systemctl start docker
    systemctl enable docker
    usermod -aG docker vagrant
    docker network create yolo_default
  SHELL

  # Client VM definition
  config.vm.define "client" do |client|
    client.vm.network "forwarded_port", guest: 3000, host: 3000
    client.vm.network "forwarded_port", guest: 22, host: 2222
    client.vm.provision "shell", inline: <<-SHELL
      cd /vagrant
      docker-compose up -d
    SHELL
  end

  # Backend VM definition
  config.vm.define "backend" do |backend|
    backend.vm.network "forwarded_port", guest: 5000, host: 5000
    backend.vm.network "forwarded_port", guest: 22, host: 2200
    backend.vm.provision "shell", inline: <<-SHELL
      cd /vagrant
      docker-compose up -d
    SHELL
  end

  # MongoDB VM definition
  config.vm.define "mongo" do |mongo|
    mongo.vm.network "forwarded_port", guest: 27017, host: 1234
    mongo.vm.network "forwarded_port", guest: 22, host: 1234
    mongo.vm.boot_timeout = 600  # Increase boot timeout to 10 minutes
    mongo.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.extra_vars = {
        vars_file: "/vagrant/vars/vars.yml"
      }
    end
  end
end
