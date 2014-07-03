# -*- mode: ruby; -*-
require 'etc'

$script = <<SCRIPT
#!/bin/bash
echo "Finalizing docker setup"

DOCKER_CONF=/etc/default/docker
[[ `tail -n 1 ${DOCKER_CONF} | grep 0.0.0.0:2375` ]] || echo 'other_args="${other_args} -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"' >> ${DOCKER_CONF}
service docker restart
SCRIPT


Vagrant.configure("2") do |config|
  config.vm.define :box, primary: true, autostart: true do |box|
    box.vm.box = "ubuntu/trusty64"

    box.vm.hostname = "dev.local"
    box.vm.network :private_network, ip: "10.1.2.44"
    
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
