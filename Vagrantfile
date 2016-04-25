# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# vagrant-proxyconf: https://tmatilai.github.io/vagrant-proxyconf
HTTP_PROXY="http://proxy:3128"
HTTPS_PROXY=HTTP_PROXY

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http     = HTTP_PROXY
    config.proxy.https    = HTTPS_PROXY
    config.proxy.no_proxy = "localhost, 127.0.0.1, .hacklab"
  end

  if Vagrant.has_plugin?("vagrant-hosts")
    config.vm.provision :hosts do |provisioner|
      provisioner.add_localhost_hostnames = false
      provisioner.autoconfigure = true
      provisioner.sync_hosts = true
    end
  end

  config.vm.box = "puppetlabs/centos-7.2-64-puppet"

  # puppet server + puppet agent
  config.vm.define "puppetserver" do |puppetserver|
    puppetserver.vm.hostname = "puppetserver.hacklab"
    puppetserver.vm.network :private_network, ip: "192.168.250.20"
    puppetserver.vm.provision "shell", path: "puppet/instala.sh"
    puppetserver.vm.provider "virtualbox" do |v|
      v.customize [ "modifyvm", :id, "--cpus", "2" ]
      v.customize [ "modifyvm", :id, "--memory", "1024" ]
      v.customize [ "modifyvm", :id, "--name", "puppetserver.hacklab" ]
      v.customize [ "modifyvm", :id, "--groups", "/pcp" ]
    end
  end

  # puppet agent + puppetdb + puppet explorer
  config.vm.define "puppetdb" do |puppetdb|
    puppetdb.vm.hostname = "puppetdb.hacklab"
    puppetdb.vm.network :private_network, ip: "192.168.250.25"
    puppetdb.vm.provider "virtualbox" do |v|
      v.customize [ "modifyvm", :id, "--cpus", "2" ]
      v.customize [ "modifyvm", :id, "--memory", "512" ]
      v.customize [ "modifyvm", :id, "--name", "puppetdb.hacklab" ]
      v.customize [ "modifyvm", :id, "--groups", "/pcp" ]
    end
    puppetdb.vm.provision "puppet_server" do |puppet|
      puppet.puppet_server = "puppetserver.hacklab"
      puppet.puppet_node = "puppetdb.hacklab"
      puppet.options = "--verbose"
    end
  end

  #  puppet agent + mcollective client + activemq
  config.vm.define "puppetmq" do |puppetmq|
    puppetmq.vm.hostname = "puppetmq.hacklab"
    puppetmq.vm.network :private_network, ip: "192.168.250.30"
    puppetmq.vm.provider "virtualbox" do |v|
      v.customize [ "modifyvm", :id, "--cpus", "2" ]
      v.customize [ "modifyvm", :id, "--memory", "512" ]
      v.customize [ "modifyvm", :id, "--name", "puppetmq.hacklab"]
      v.customize [ "modifyvm", :id, "--groups", "/pcp" ]
    end
    puppetmq.vm.provision "puppet_server" do |puppet|
      puppet.puppet_server = "puppetserver.hacklab"
      puppet.puppet_node = "puppetmq.hacklab"
      puppet.options = "--verbose"
    end
  end

end
