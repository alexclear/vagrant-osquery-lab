# -*- mode: ruby -*-
# vi: set ft=ruby :

$OSQUERY1_IP = "172.16.137.132"
$OSQUERY2_IP = "172.16.137.133"
$OSQUERY3_IP = "172.16.137.134"

ANSIBLE_RAW_SSH_ARGS = []

Vagrant.configure("2") do |config|
  config.vm.define "osquery1" do |osquery1|
    osquery1.vm.box = "generic/ubuntu1804"
    osquery1.vm.hostname = "osquery1"
    osquery1.vm.network "private_network", ip: $OSQUERY1_IP

    osquery1.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    osquery1.vm.provider :libvirt do |v, override|
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = 2
      v.memory = 4096
    end

    osquery1.vm.provision "shell", inline: "apt-get install -y python"
  end

  config.vm.define "osquery2" do |osquery2|
    osquery2.vm.box = "generic/ubuntu1604"
    osquery2.vm.hostname = "osquery2"
    osquery2.vm.network "private_network", ip: $OSQUERY2_IP

    osquery2.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    osquery2.vm.provider :libvirt do |v, override|
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = 2
      v.memory = 4096
    end

    osquery2.vm.provision "shell", inline: "apt-get install -y python"
  end

  config.vm.define "osquery3" do |osquery3|
    osquery3.vm.box = "debian/stretch64"
    osquery3.vm.hostname = "osquery3"
    osquery3.vm.network "private_network", ip: $OSQUERY3_IP

    osquery3.vm.provider :virtualbox do |v, override|
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/osquery1/virtualbox/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/osquery2/virtualbox/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/osquery3/virtualbox/private_key "
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    osquery3.vm.provider :libvirt do |v, override|
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/osquery1/libvirt/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/osquery2/libvirt/private_key "
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/osquery3/libvirt/private_key "
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = 2
      v.memory = 4096
    end

    osquery3.vm.provision "shell", inline: "apt-get install -y python"
    osquery3.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.config_file = "ansible/ansible.cfg"
      ansible.inventory_path = "ansible/inventory"
      ansible.become = true
      ansible.limit = "all"
      ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
    end
  end
end
