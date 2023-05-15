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
  # Sharing this folder is required as per provisioning via ansible locally
  config.vm.synced_folder "./ansible", "/vagrant", owner: "vagrant", group: "vagrant"
  # # Optional folders below. Sharing folders like this gives you the freedom to
  # # edit the files even when the VM is not booted. Meaning that they are linked
  # # from your host
  # # Just log in via `vagrant ssh` and do your work.
  # #
  # # If you're working with the Remote SSH extension on VSCode
  # # this won't be neccessary.
  # # However, you may only access your folders once the VM is up and running.
  # # It is up to the dev experience you want.
  # config.vm.synced_folder "./project", "/home/vagrant/project", owner: "vagrant", group: "vagrant", create: true

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  config.vm.network "forwarded_port", guest: 8080, host: 8080, auto_correct: true
  config.vm.usable_port_range = 8000..8999
  # Create a private network, which allows host-only access to the machine.
  # Feel free to comment this out if you don't need public reachability from your
  # host machine
  config.vm.network "private_network", type: "dhcp", netmask: "255.255.255.0", dhcp_ip:"192.168.56.100", dhcp_lower: "192.168.56.101", dhcp_upper: "192.168.56.254"
  # VirtualBox customizations
  config.vm.provider "virtualbox" do |vb|
    # VM name
    vb.name = "ubuntu_jammy64_dev"
    # Customize the amount of memory on the VM:
    vb.memory = "4096"
    # Virtual CPUs
    vb.cpus = "4"
  end
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbooks/main.yml"
    ansible.provisioning_path = "/vagrant"
    #! Comment out accordingly.
    #! By default A TAG HAS TO BE ENABLED
    # Otherwise it defaults to executing all of them.
    # Under the hood, there's a base file that always runs regardless.
    #
    # The order doesn't matter here.
    ansible.tags = [
      #? Cloud providers
      # "aws",
      #? Container tools
      # "docker",
      # "k8s_tools",
      # "minikube",
      #? Dbs
      # "mysql",
      # "pg",
      #? Programming languages
      # "go",
      # "node_js",
      # "python",
      # "rust",
      #? Terminal
      # "starship",
      #? UI
      # "gui",
      #? Vc
      # "git",
      # "github",
    ]
  end
end