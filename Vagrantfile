# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANT_MIN_VERSION = '1.5.1'
VAGRANTFILE_API_VERSION = '2'

PORT_MAPPINGS = {
  8080 => 8080, 8000 => 8000,
  3000 => 3000, 3001 => 3001
}

Vagrant.require_version ">= #{VAGRANT_MIN_VERSION}"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.hostname = 'einstein.local'

  # config.vm.box = 'ubuntu-12.04.2-server-amd64'
  # config.vm.box_url = 'http://grahamc.com/vagrant/ubuntu-12.04.2-server-amd64-vmware-fusion.box'

  config.vm.box = 'precise64_vmware'
  config.vm.box_url = 'http://files.vagrantup.com/precise64_vmware.box'

  config.vm.provider :vmware_fusion do |v|
    v.name = 'einstein.local'
    v.customize ['modifyvm', :id, '--memory', 1024]
    v.customize ['modifyvm', :id, '--cpuexecutioncap', 90]

    # v.gui = true
  end

  config.ssh.forward_agent = true

  config.vm.network :private_network, ip: '10.11.50.10'
  config.vm.synced_folder 'apps', '/apps',
    :nfs => { :mount_options => ['dmode = 777', 'fmode = 777'] }

  PORT_MAPPINGS.each do |from, to|
    config.vm.network :forwarded_port, guest: from, host: to, auto_corrent: true
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = 'provisioning/boxed.yml'
    # ansible.inventory_path = 'inventory'
    ansible.verbose = 'vv'
  end
end
