#!/bin/env ruby

Vagrant.configure("2") do |config|
  config.ssh.pty = false
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

  config.vm.define "kong", primary: true do |kong|
    kong.vm.box = "ubuntu/xenial64"
    kong.vm.box_url = "http://cloud-images.ubuntu.com/xenial/20180126/xenial-server-cloudimg-amd64-vagrant.box"
    kong.vm.network "private_network", ip: "192.168.33.10"
   
    kong.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update && sudo apt-get install -y python
    SHELL

    kong.vm.provision "infrastructure", type: "ansible"  do |ansible|
      ansible.playbook = './ansible/infrastructure.yml'
    end

    kong.vm.provision "apigw", type: "ansible" do |ansible|
      ansible.playbook = './ansible/apigw.yml'
    end
  end

  config.vm.define "service" do |service|
    service.vm.box = "ubuntu/xenial64"
    service.vm.box_url = "http://cloud-images.ubuntu.com/xenial/20180126/xenial-server-cloudimg-amd64-vagrant.box"
    service.vm.network "private_network", ip: "192.168.33.11"

    service.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update && sudo apt-get install -y nginx
    SHELL
  end
end
