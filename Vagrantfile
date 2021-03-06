# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |cluster|
  cluster.vm.define :elk1 do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "private_network", ip: "10.0.0.6"
    config.vm.hostname = "elk1"
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
   end
  end

  cluster.vm.define :dbc1 do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "private_network", ip: "10.0.0.5"
    config.vm.hostname = "dbc1"
  end

  cluster.vm.define :dbc2 do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "private_network", ip: "10.0.0.4"
    config.vm.hostname = "dbc2"
  end

  cluster.vm.define :pgpool do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "private_network", ip: "10.0.0.3"
    config.vm.hostname = "pgpool"
  end

  cluster.vm.define :web1 do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "private_network", ip: "10.0.0.2"
    config.vm.hostname = "web1"
  end

  cluster.vm.provision :ansible do |ansible|
    ansible.playbook = "provision.yml"
    ansible.groups = {
      "dbc-primary" => ["dbc1"],
      "dbc-replica" => ["dbc2"],
      "dbc-pgpool" => ["pgpool"],
      "application" => ["web1"],
      "monitoring-elk" => ["elk1"]
    }
  end
end
