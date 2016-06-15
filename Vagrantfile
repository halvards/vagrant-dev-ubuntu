# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|

  # Base box (from Hashicorp Atlas)
  config.vm.box = 'halvards/lubuntu1604'

  # Assign this VM a unique hostname
  config.vm.host_name = "#{ENV['USER']}.dev.ubuntu64.vagrantup.com"

  # Copy private key to VM for Git and other tools
  config.vm.provision 'file', source: "#{ENV['HOME']}/.ssh/id_rsa", destination: '~/.ssh/id_rsa'

  # Provision VM with Ansible (with workaround for Windows)
  require 'rbconfig'
  is_windows_host = (RbConfig::CONFIG['host_os'] =~ /mswin|mingw|cygwin/)
  if is_windows_host
    # Run Ansible on VM using shell provisioning (for Windows hosts)
    config.vm.provision 'shell' do |sh|
      sh.path = 'provision.sh'
      sh.args = 'playbook.yml'
    end
  else
  #   # Standard Ansible provisioning (for Mac and Linux hosts)
    config.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'playbook.yml'
      ansible.verbose = 'vv' # v, vv, vvv, vvvv
      ansible.sudo = true
      ansible.sudo_user = 'root'
    end
  end

  # Forward a port from the guest to the host
  config.vm.network 'forwarded_port', guest: 22,   host: 2190, id: 'ssh', auto_correct: true
  config.vm.network 'forwarded_port', guest: 3000, host: 3000, auto_correct: true # Web server
  config.vm.network 'forwarded_port', guest: 5432, host: 5432, auto_correct: true # PostgreSQL server
  config.vm.network 'forwarded_port', guest: 8080, host: 8080, auto_correct: true # Web server

  config.vm.provider 'virtualbox' do |vb|
    # Boot with a GUI so you can see the screen. (Default is headless)
    vb.gui = true

    # Set name of VirtualBox VM
    vb.name = 'dev-ubuntu64'

    # Set memory allocated to the VM in MB
    vb.customize ['modifyvm', :id, '--memory', '2048']
    vb.customize ['modifyvm', :id, '--cpus', '1']
    vb.customize ['modifyvm', :id, '--vram', '128']
    vb.customize ['modifyvm', :id, '--accelerate2dvideo', 'off']
    vb.customize ['modifyvm', :id, '--accelerate3d', 'on']

    # Enable shared clipboard
    vb.customize ['modifyvm', :id, '--clipboard', 'bidirectional']

    # Suppress some VirtualBox messages
    vb.customize ['setextradata', :id, 'GUI/SuppressMessages', 'remindAboutAutoCapture,remindAboutWrongColorDepth,remindAboutMouseIntegrationOn,remindAboutMouseIntegrationOff,confirmInputCapture,confirmGoingFullscreen']

    # Make DNS work across host VPN connection
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
  end
end
