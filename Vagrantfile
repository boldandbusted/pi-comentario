# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "bento/debian-12.5"

   if Vagrant::Plugin::Manager.instance.plugin_installed?('vagrant-vbguest')
     config.vbguest.auto_update = false
     config.vbguest.no_install = true
   end

  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # config.vm.network "private_network", ip: "192.168.33.10"

  # config.vm.network "public_network"

  # config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.synced_folder "./ansible/", "/vagrant", type: "rsync"
  config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
    vb.gui = false
  
     # Customize the amount of memory on the VM:
    vb.memory = "8192"
    vb.cpus = "2"
  end

  config.vm.provision 'shell' do |shell|
    content = %(
    apt-get update
    apt-get dist-upgrade -y
    apt-get install bash-completion python-is-python3 python-dev-is-python3 python3-pip -y
    update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1
    )
    shell.inline = content
    shell.reboot = false
  end

  config.vm.provision 'shell' do |shell|
    content = %(
    echo 'set -g default-terminal "tmux-256color"' >> ~/.tmux.conf
    )
    shell.inline = content
    shell.privileged = false
  end

  config.vm.provision 'ansible_local' do |ansible|
    ansible.install = true
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "playbook.yml"
    #ansible.install_mode = "pip"
    #ansible.verbose = "vvvv"
    #ansible.extra_vars = {
    #  admin_user: 'ubuntu'
    #}
  end

  config.vm.provision 'shell' do |reboot|
    reboot.reboot = true
  end
end
