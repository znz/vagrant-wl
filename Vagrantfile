# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  #config.vm.box = 'bento/ubuntu-16.04'
  config.vm.box = 'boxcutter/ubuntu1610'
  config.vm.hostname = ENV['VM_HOSTNAME'] || 'wl'

  if Vagrant.has_plugin?('vagrant-cachier')
    config.cache.scope = :box
    config.cache.synced_folder_opts = {
      owner: '_apt',
      group: '_apt',
      mount_options: ['dmode=777', 'fmode=666']
    }
  end

  config.vm.synced_folder "#{Dir.home}/.gnupg", "/home/.gnupg"

  config.vm.provider 'virtualbox' do |vb|
    vb.memory = '1024'
    vb.cpus = 2
    vb.customize ['modifyvm', :id, '--nictype1', 'virtio']
    vb.customize [
      'modifyvm', :id,
      '--hwvirtex', 'on',
      '--nestedpaging', 'on',
      '--largepages', 'on',
      '--ioapic', 'on',
      '--pae', 'on',
      '--paravirtprovider', 'kvm',
    ]
  end

  config.vm.provision 'ansible' do |ansible|
    ansible.playbook = 'provision/playbook.yml'
    unless ENV['VM_SKIP_GALAXY']
      ansible.galaxy_role_file = 'provision/requirements.yml'
    end
    ansible.verbose = ENV['VM_ANSIBLE_VERBOSE']
  end
end
