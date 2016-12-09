# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  N = 3
  (1..N).each do |machine_id|
    config.vm.define "riak#{machine_id}" do |machine|
      machine.vm.box = "ubuntu/trusty64"
      machine.vm.hostname = "riak#{machine_id}.local"
      machine.vm.network "private_network", ip: "192.168.77.#{20+machine_id}"
      machine.vm.provider "virtualbox" do |vb|
	vb.memory = "2048"
        #vb.memory = "1024"
        #vb.memory = "4096"
      end
      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if machine_id == 1
        machine.vm.network "forwarded_port", guest: 9000, host: 9000
	machine.vm.network "forwarded_port", guest: 8098, host: 8098
	machine.vm.network "forwarded_port", guest: 8087, host: 8087
      end
      if machine_id == N
        machine.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          #ansible.raw_arguments = "-vvv"
          ansible.playbook = "playbook.yml"
        end
      end
    end
  end
end
