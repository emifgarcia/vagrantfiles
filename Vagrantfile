# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  # config.vm.box = "generic/debian12"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", type: "dhcp"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Disable the default share of the current code directory. Doing this
  # provides improved isolation between the vagrant box and your host
  # by making sure your Vagrantfile isn't accessible to the vagrant box.
  # If you use this you may want to enable additional shared subfolders as
  # shown above.
  # config.vm.synced_folder ".", "/vagrant", disabled: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt update
  #   apt install -y nginx
  # SHELL

  #cria a vm controle com debian12, 2G de ram, 2 cpus, uma interface hot-only e executa o script update.sh 
  config.vm.define "controle" do |controle|
    controle.vm.box = "generic/debian12"
    controle.vm.network "private_network", ip: "172.17.177.100"
    controle.vm.hostname = "controle"
    controle.vm.provider "virtualbox" do |vb|
      vb.name = "controle" #nome no virtualbox
      vb.memory = "2048"
      vb.cpus = 2
    end
    controle.vm.provision "shell", path: "scripts/update.sh"
    config.vm.synced_folder ".", "/vagrant", disabled: false
  end

  #cria a vm web com debian12, 512MB de ram, 1 cpu, uma interface hot-only e executa o script update.sh  
  config.vm.define "web" do |web|
    web.vm.box = "generic/debian12"
    web.vm.network "private_network", ip: "172.17.177.101"
    web.vm.hostname = "web"
    web.vm.provider "virtualbox" do |vb|
      vb.name = "web"
      vb.memory = "512"
      vb.cpus = 1
    end
    web.vm.provision "shell", path: "scripts/update.sh"
    web.vm.synced_folder ".", "/vagrant", disabled: false
  end

  #cria a vm db com debian12, 512MB de ram, 1 cpu, uma interface hot-only e executa o script update.sh  
  config.vm.define "db" do |db|
    db.vm.box = "generic/debian12"
    db.vm.network "private_network", ip: "172.17.177.102"
    db.vm.hostname = "db"
    db.vm.provider "virtualbox" do |vb|
      vb.name = "db"
      vb.memory = "512"
      vb.cpus = 1
    end
    db.vm.provision "shell", path: "scripts/update.sh"
    db.vm.synced_folder ".", "/vagrant", disabled: false
  end


  #cria a vm master com centos7, 2GB de ram, 2 cpus e uma interface hot-only  
  config.vm.define "master" do |master|
    master.vm.box = "geerlingguy/centos7"
    master.vm.network "private_network", ip: "172.17.177.110"
    master.vm.hostname = "master"
    master.vm.provider "virtualbox" do |vb|
      vb.name = "master"
      vb.memory = "2048"
      vb.cpus = "2"
    end
    master.vm.synced_folder ".", "/vagrant", disable: false
  end

  #cria as vms node1, node2 e node3 com centos7, 512MB de ram, 1 cpu, uma interface hot-only e adiciona no grupo nodes  
  (1..3).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "geerlingguy/centos7"
      node.vm.network "private_network", ip: "172.17.177.11#{i}"
      node.vm.hostname = "node#{i}"
      node.vm.provider "virtualbox" do |vb|
        vb.name = "node#{i}"
        vb.memory = "512"
        vb.cpus = 1
      end
    end
    config.group.groups = {
      "nodes" => [
        "node#{i}"
        ]
    }
  end

  # cria o grupo controle com a vm controle e o grupo clientes com as vms web e db
  config.group.groups = {
  "controle" => [
    "controle",
  ],
  "clientes" => [
    "web",
    "db",
  ],
}

end
