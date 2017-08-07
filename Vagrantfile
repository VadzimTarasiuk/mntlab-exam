# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.define "centos2" do |c|
    c.vm.box = "sbeliakou/centos-7.3-x86_64-minimal"
    c.vm.network "private_network", ip: "11.11.12.12"
    c.vm.hostname = "centos2"
    c.vm.provider "virtualbox" do |v|
		v.memory = "2048"
		v.name = "centos2"
    end
  end
end
