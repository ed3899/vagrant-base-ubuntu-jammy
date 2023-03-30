# -*- mode: ruby -*-
# vi: set ft=ruby :
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/jammy64"
  config.vm.post_up_message = "Ubuntu Jammy64 up and ready"
  config.vm.synced_folder "./ansible", "/vagrant", owner: "vagrant", group: "vagrant"
  # Additional folders
  # config.vm.synced_folder "./src", "/home/vagrant/src", owner: "vagrant", group: "vagrant", create: true
  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true
  config.vm.usable_port_range = 8000..8999
  # Create a private network, which allows host-only access to the machine.
  config.vm.network "private_network", type: "dhcp", netmask: "255.255.255.0", dhcp_ip:"192.168.56.100", dhcp_lower: "192.168.56.101", dhcp_upper: "192.168.56.254"
  config.vm.provider "virtualbox" do |vb|
    # VM name
    vb.name = "ubuntu_jammy64"
    # Customize the amount of memory on the VM:
    vb.memory = "2048"
    # Virtual CPUs
    vb.cpus = "2"
  end
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "ansible/playbooks/base.yml"
    # Comment out accordingly
    ansible.tags = [
      # "git",
      # "brew",
      # "gui",
      # "docker",
      # "aws",
      # "k8s",
      # "minikube",
      # "starship",
      # "rust",
    ]
  end
end