# -*- mode: ruby -*-
# vi: set ft=ruby :

# global configuration
VAGRANTFILE_API_VERSION = "2"
VAGRANT_BOX = "puphpet/debian75-x64"
VAGRANT_BOX_MEMORY = 2048
VIRTUAL_BOX_NAME = "zraydoctrine2demo"

# only change these lines if you know what you do
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box = VAGRANT_BOX
    config.vm.hostname = VIRTUAL_BOX_NAME + ".dev"

    # configure vhost ports, more vhosts => more port forwarding definitions
    config.vm.network :forwarded_port, guest: 80, host: 8080
    config.vm.network :forwarded_port, guest: 10081, host: 10081

    config.vm.synced_folder ".", "/vagrant", disabled: true

    # forward ssh requests for public keys
    config.ssh.forward_agent = true

    # ensure box name
    config.vm.define VIRTUAL_BOX_NAME do |t|
    end

    # configure virtual box
    config.vm.provider :virtualbox do |vb|
        vb.name = VIRTUAL_BOX_NAME
        vb.customize ["modifyvm", :id, "--memory", VAGRANT_BOX_MEMORY]
    end

    Vagrant.configure("2") do |config|
      config.vm.provision :docker_compose, yml: "/vagrant/docker-compose.yml", run: "always"
    end
end