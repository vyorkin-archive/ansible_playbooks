# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

RAM           = 1024
CPU_MAX_USAGE = 90
APPS_FOLDER   = 'apps'

PORT_MAPPINGS = {
  8080 => 8080, 8000 => 8000,
  3000 => 3000, 3001 => 3001
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "base"

  config.vm.box = "precise64_vmware"
  config.vm.box_url = "http://files.vagrantup.com/precise64_vmware.box"

  config.vm.provider :vmware_fusion do |v|
    v.name = "einstein.local"
    v.customize ["modifyvm", :id, "--memory", RAM]
    v.customize ["modifyvm", :id, "--cpuexecutioncap", CPU_MAX_USAGE]

    # v.gui = true
  end

  config.cache.auto_detect = true if Vagrant.has_plugin?("vagrant-cachier")

  config.ssh.forward_agent = true

  PORT_MAPPINGS.each do |from, to|
    config.vm.network :forwarded_port, guest: from, host: to, auto_corrent: true
  end
  config.vm.network :private_network, ip: '10.11.50.10'
  config.vm.synced_folder APPS_FOLDER, "/#{APPS_FOLDER}",
    :nfs => { :mount_options => ['dmode = 777', 'fmode = 777'] }

  config.vm.provision :ansible do |ansible|
    ansible.playbook = 'provisioning/kickass.yml'
    ansible.verbose = 'vv'
  end
end
