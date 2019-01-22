bootstrap = <<SCRIPT
  useradd -m jaels --groups sudo -s /bin/bash
  mkdir /home/jaels/.ssh && curl --silent https://github.com/renderqwerty.keys >> /home/jaels/.ssh/authorized_keys
  echo "%jaels ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/jaels
  su -c "printf 'cd /home/jaels\nsudo su jaels' >> .bash_profile" -s /bin/sh vagrant
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
    config.vm.network "private_network", ip: "172.16.0.2" #this added to be sure that we knows ip of host machine (172.16.0.1)
    config.vm.provision "shell", inline: "#{bootstrap}", privileged: true
    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=775,fmode=664"]
    config.vm.provision "ansible_local" do |ansible|
        ansible.become = true
        ansible.playbook = "guest.yml"
        ansible.vault_password_file = "credentials/ansible_vault_password"
        ansible.galaxy_role_file = "roles/requirements.yml"
        ansible.galaxy_roles_path = "/etc/ansible/roles"
        ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path}"
        ansible.limit = 'all,localhost'
      end
      config.vm.provision "ansible_host", type: "ansible_local" do |ansible_host|
          ansible_host.playbook = "host.yml"
          ansible_host.inventory_path = "credentials/win_inventory"
          ansible_host.limit = 'all,localhost'
          ansible_host.verbose = true
      end
  end
