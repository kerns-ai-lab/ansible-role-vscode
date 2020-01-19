# -*- mode: ruby -*-
# vi: set ft=ruby :

ANSIBLE_DIR = "/ansible"
ANSIBLE_ROLE_NAME = "ansible-role-vscode"
ANSIBLE_ROLE_DIR = File.join(ANSIBLE_DIR, ANSIBLE_ROLE_NAME)

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/xenial64"
  # Ansible directory. Ansible won't load ansible.cfg from world-writeable directories
  # https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings
  config.vm.synced_folder ".", ANSIBLE_ROLE_DIR, create: true,
    owner: "vagrant", 
    group: "vagrant",
    mount_options: ["dmode=775,fmode=664"]

  # Skip time-intensive guest additions for headless testing
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
  end

  config.vm.provider "virtualbox" do |vb|
    # A linked clone is a copy of a virtual machine that shares virtual 
    # disks with the parent virtual machine in an ongoing manner. 
    # This conserves disk space, and allows multiple virtual machines 
    # to use the same master VM image installation.
    vb.linked_clone = true if Vagrant::VERSION =~ /^1.8/
    # Support working when on 3DS VPN
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  if File.exist?(File.join("..", ANSIBLE_ROLE_NAME)) then
    config.vm.provision :shell, inline: <<-SHELL
      apt-get update
      apt-get install -y python-pip
    SHELL

    config.vm.provision :ansible_local do |ansible|
      # Set provision path so role is discoverable by ansible
      ansible.provisioning_path = ANSIBLE_ROLE_DIR
    	ansible.playbook = "tests/test_playbook.yml"
      ansible.install = true
      # Using pip so we can set and freeze on a specific version of ansible 
      # instead of pulling from the package repository 
      ansible.install_mode = "pip"
      ansible.version = "2.7.12"
      ansible.verbose = true
  	end
  end

end