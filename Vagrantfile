#VAGRANT_COMMAND = ARGV[0]
#if VAGRANT_COMMAND == "ssh"
#    config.ssh.username = 'jaels'
#  end

bootstrap = <<SCRIPT
  useradd -m jaels --groups sudo -s /bin/bash
  mkdir /home/jaels/.ssh && curl --silent https://github.com/renderqwerty.keys >> /home/jaels/.ssh/authorized_keys
  echo "%jaels ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/jaels
  apt install python-setuptools -y
SCRIPT

Vagrant.configure("2") do |config|

    config.vm.provider :virtualbox do |v|
        v.name = "linux_workstation"
        v.memory = 3192
        v.cpus = 2
      end

    config.vm.box = "ubuntu/bionic64"
    config.vm.hostname = "vm-fisakov"
    config.vm.network "public_network", ip: "192.168.19.76"
    config.vm.provision "shell", inline: "#{bootstrap}", privileged: true
    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=775,fmode=664"]
    config.vm.provision "ansible_local" do |ansible|
        ansible.become = true
        ansible.playbook = "site.yml"
        ansible.vault_password_file = "credentials/ansible_vault_password"
        ansible.galaxy_role_file = "roles/requirements.yml"
        ansible.galaxy_roles_path = "/etc/ansible/roles"
        ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path} --force"
        ansible.limit = 'all,localhost'
      end
  end
