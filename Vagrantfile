# -*- mode: ruby -*-
# vi: set ft=ruby :

# Assumes a box from https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box

# This sets up 4 instances, one with fabric-store and 3 nodes.


fabric_nodes = {
  'prm1' => {
    'ip' => '192.168.70.61',
    'playbook' => 'prm-node.yml'
  },
  'prm2' => {
    'ip' => '192.168.70.62',
    'playbook' => 'prm-node.yml'
  },
  'prm3' => {
    'ip' => '192.168.70.63',
    'playbook' => 'prm-node.yml'
  },
}


Vagrant.configure("2") do |config|
  config.vm.box = 'centos65-x86_64-20140116'
  config.vm.box_url = 'https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box'
  config.vm.boot_timeout = 600
  # Create all three nodes identically except for name and ip
  fabric_nodes.each_pair { |node, node_params|
    config.vm.define node do |node_config|
      node_config.vm.hostname = "#{node}.local"
      node_config.vm.network :private_network, ip: node_params['ip']
      node_config.vm.provision :ansible do |ansible|
        ansible.verbose = 'vv'
        ansible.extra_vars = { ansible_ssh_user: 'vagrant' }
        ansible.playbook = 'provisioning/' + node_params['playbook']
        ansible.host_key_checking = false
        ansible.limit = 'all'
      end
    end
  }
end

