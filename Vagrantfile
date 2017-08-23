# -*- mode: ruby -*-
# # vi: set ft=ruby :

$gopath = ENV["GOPATH"]
if $gopath.empty?
  abort("GOPATH env var must be set (or create a config.rb to specify it manually).\n")
end

Vagrant.configure("2") do |c|
  c.vm.define vm_name = "k8s-env" do |config|
    config.vm.hostname = vm_name

    config.vm.box = "centos/7"

    ip = "10.1.2.3"
    config.vm.network "private_network", ip: ip

    config.vm.boot_timeout = 3000

    config.vm.network "forwarded_port", guest: 2375, host: 2375, auto_correct: true

    config.vm.synced_folder $gopath, "/go"

    config.vm.provider :virtualbox do |vb|
      vb.memory = 4096
      vb.cpus = 2
    end

    config.vm.provision "shell", inline: "GUEST_IP=#{ip} /vagrant/setup.sh"
  end
end

