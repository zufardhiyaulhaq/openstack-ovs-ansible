# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  config.vm.box = 'centos/7'

  (0..0).each do |i|
    config.vm.define "zu-ovs-controller-#{i}" do |db|
      db.vm.hostname = "zu-ovs-controller-#{i}"
      db.vm.network 'private_network', ip: "10.100.100.20#{i}"
      db.vm.network 'private_network', ip: "10.101.101.20#{i}"
      db.vm.network 'private_network', ip: "10.101.102.20#{i}", auto_config: false
      db.vm.provider 'virtualbox' do |vb|
        vb.name = "zu-ovs-controller-#{i}"
        vb.memory = 8000
        vb.cpus = 4
      end
    end
  end

  (0..2).each do |i|
    config.vm.define "zu-ovs-compute-#{i}" do |web|
      web.vm.hostname = "zu-ovs-compute-#{i}"
      web.vm.network 'private_network', ip: "10.100.100.21#{i}"
      web.vm.network 'private_network', ip: "10.101.101.21#{i}"
      web.vm.provider 'virtualbox' do |vb|
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
