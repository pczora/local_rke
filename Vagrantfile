# -*- mode: ruby -*-
# vi: set ft=ruby :

master_count = (ENV["K8S_MASTERS"] || "1").to_i
worker_count = (ENV["K8S_WORKERS"] || "2").to_i
extra_count  = (ENV["K8S_EXTRAS"] || "1").to_i
vm_memory = (ENV["K8S_VM_MEMORY"] || "1024").to_i
vm_image = ENV["K8S_VM_IMAGE"] || "bento/ubuntu-18.04" 
vagrant_prefix = ENV["LOCAL_RKE_VM_PREFIX"] || ""

Vagrant.configure("2") do |config|

  config.vm.box = vm_image
  config.vm.box_check_update = false

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 1
    vb.memory = vm_memory
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  (0..(master_count - 1)).each do |i|
    config.vm.define "#{vagrant_prefix}controller-#{i}" do |node|
      node.vm.hostname = "controller-#{i}"
      node.vm.network "private_network", ip: "192.168.100.10#{i}"
      node.vm.provider "virtualbox" do |vb|
        vb.name = "#{vagrant_prefix}controller-#{i}"
      end
    end
  end

  (0..(worker_count - 1)).each do |i|
    config.vm.define "#{vagrant_prefix}worker-#{i}" do |node|
      node.vm.hostname = "#{vagrant_prefix}worker-#{i}"
      node.vm.network "private_network", ip: "192.168.100.20#{i}"
      node.vm.provider "virtualbox" do |vb|
        vb.name = "#{vagrant_prefix}worker-#{i}"
      end
    end
  end

  (0..(extra_count - 1)).each do |i|
    config.vm.define "#{vagrant_prefix}extra-#{i}" do |node|
      node.vm.hostname = "#{vagrant_prefix}extra-#{i}"
      node.vm.network "private_network", ip: "192.168.100.25#{i}"
      node.vm.provider "virtualbox" do |vb|
        vb.name = "#{vagrant_prefix}extra-#{i}"
      end
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/playbook_extra.yml"
  end
end
