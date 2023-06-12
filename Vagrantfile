# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.post_up_message = "Ubuntu Jammy64 up and ready"
  # Sharing this folder is required as per provisioning via ansible locally
  config.vm.synced_folder "./ansible", "/ansible", owner: "vagrant", group: "vagrant"
  config.vm.synced_folder "./assets", "/assets", owner: "vagrant", group: "vagrant"

  config.vm.network "forwarded_port", guest: 8080, host: 8080, auto_correct: true
  config.vm.usable_port_range = 8000..8999
  config.vm.network "private_network", type: "dhcp", netmask: "255.255.255.0", dhcp_ip:"192.168.56.100", dhcp_lower: "192.168.56.101", dhcp_upper: "192.168.56.254"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "ubuntu_jammy64_dev"
    vb.memory = "4096"
    vb.cpus = "4"
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbooks/main.yml"
    ansible.provisioning_path = "/ansible"
    ansible.galaxy_role_file = "/ansible/galaxy/requirements.yml"
    # ansible.skip_tags = [
    #   "always",
    #   "init",
    # ]
    ansible.tags = [
      #? Cloud providers
      # "aws",
      #? Containerization
      # "docker",
      #? Dbs
      # "mysql",
      # "pg-15",
      #? IaC
      # "pulumi",
      #? Orchestration
      # "helm",
      # "kubectl",
      # "minikube",
      #? Programming languages
      # "dotnet_sdk",
      # "go",
      # "node_js",
      # "python_anaconda",
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