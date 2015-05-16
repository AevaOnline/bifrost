# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = 'ubuntu/trusty64'

  config.vm.define 'bifrost' do |bifrost|
    bifrost.vm.provider :virtualbox do |vb|
      vb.customize ['modifyvm', :id, '--memory', '4196', '--cpus', '2', '--cpuexecutioncap', '100']
    end

    # Set up private NAT'd network
    bifrost.vm.network 'private_network', ip: '192.168.99.10' # it goes to 11
    # This assumes you have statically configured your eth0 port with IP 192.168.2.1
    # and then assigns the VM to .2, and places it on a network bridge with eth0
    # thus allowing the Bifrost VM to manage any hardware attached to eth0
    bifrost.vm.network 'public_network', ip: '192.168.2.2', bridge: 'eth0'

    bifrost.vm.synced_folder "./", "/home/vagrant/bifrost"
    bifrost.vm.synced_folder "vagrant_src/", "/opt/stack"

    bifrost.vm.provision 'ansible' do |ansible|
      ansible.verbose = 'v'
      ansible.playbook = 'vagrant.yml'
      ansible.extra_vars = {
          ip: '192.168.2.2'
      }
    end
  end
end
