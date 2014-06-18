# -*- mode: ruby; -*-
require 'etc'

$script = <<SCRIPT
#!/bin/bash
echo "Finalizing docker setup"

DOCKER_CONF=/etc/sysconfig/docker
[[ `tail -n 1 ${DOCKER_CONF} | grep 0.0.0.0:4243` ]] || echo 'other_args="${other_args} -H tcp://0.0.0.0:4243 -H unix:///var/run/docker.sock"' >> ${DOCKER_CONF}
service docker restart
SCRIPT


Vagrant.configure("2") do |config|
  config.vm.define :box, primary: true, autostart: true do |box|
    box.vm.box = "sooz"
    box.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v6.4.2/centos64-x86_64-20140116.box"

    box.vm.hostname = "dev.local"
    box.vm.network :private_network, ip: "10.1.2.4"
    box.vm.network :forwarded_port, guest: 8000, host: 8000, auto_correct: true
    box.vm.network :forwarded_port, guest: 8080, host: 8080, auto_correct: true
    box.vm.network :forwarded_port, guest: 5432, host: 5432, auto_correct: true
    box.vm.network :forwarded_port, guest: 4243, host: 4243, auto_correct: true
    
    box.vm.synced_folder ".", "/opt/app"

    box.vm.provider :virtualbox do |v|
      v.memory = 1024
      v.cpus = 2
    end

    box.vm.provision "docker" do |d|
      d.pull_images "ubuntu:14.04"
    end
    
    box.vm.provision "shell", inline: $script
  end
end
