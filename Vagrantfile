# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.box = 'centos/7'

  (0..0).each do |i|
    config.vm.define "zu-ovs-controller-#{i}" do |controller|
      controller.vm.hostname = "zu-ovs-controller-#{i}"
      controller.vm.network 'private_network', ip: "10.100.100.20#{i}"
      controller.vm.network 'private_network', ip: "10.101.101.20#{i}"
      controller.vm.network 'private_network', ip: "10.101.102.20#{i}", auto_config: false
      controller.vm.customize ['modifyvm', :id, '--nicpromisc2', 'allow-all']
      controller.vm.customize ['modifyvm', :id, '--nicpromisc3', 'allow-all']
      controller.vm.customize ['modifyvm', :id, '--nicpromisc4', 'allow-all']
      controller.vm.provider 'virtualbox' do |vb|
        vb.name = "zu-ovs-controller-#{i}"
        vb.memory = 8000
        vb.cpus = 4
      end
    end
  end

  (0..2).each do |i|
    config.vm.define "zu-ovs-compute-#{i}" do |compute|
      compute.vm.hostname = "zu-ovs-compute-#{i}"
      compute.vm.network 'private_network', ip: "10.100.100.21#{i}"
      compute.vm.network 'private_network', ip: "10.101.101.21#{i}"
      compute.vm.customize ['modifyvm', :id, '--nicpromisc2', 'allow-all']
      compute.vm.customize ['modifyvm', :id, '--nicpromisc3', 'allow-all']
      compute.vm.provider 'virtualbox' do |vb|
        vb.name = "zu-ovs-compute-#{i}"
        vb.memory = 8000
        vb.cpus = 4
      end
    end
  end

  config.vm.provision 'deploy', type: 'ansible', run: 'never' do |ansible|
    ansible.version = '2.5.5'
    ansible.groups = {
      'controllers' => %w[zu-ovs-controller-0],
      'computes' => %w[zu-ovs-compute-0 zu-ovs-compute-1 zu-ovs-compute-2]
    }
    ansible.playbook = 'provisioning/main.yml'
    ansible.extra_vars = { ansible_python_interpreter: '/usr/bin/python' }
  end
end
