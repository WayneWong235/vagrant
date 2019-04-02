@ansible_home = "/home/vagrant/.ansible"

Vagrant.configure("2") do |config|
  # ... other configuration

  config.vm.box = "ubuntu/trusty64"

  # Copy the Ansible playbook over to the guest machine, run rsync-auto to automatically
  # pull in the latest changes while a VM is running.
  config.vm.synced_folder "./ansible-logrotate", "#{@ansible_home}/roles/ansible-logrotate", type: 'rsync'

  # The working ansible directory created by ansible_local is owned by root
  config.vm.provision "shell", inline: "chown -R vagrant:vagrant #{@ansible_home}"

  # Disable the new default behavior introduced in Vagrant 1.7, to
  # ensure that all Vagrant machines will use the same SSH key pair.
  # See https://github.com/hashicorp/vagrant/issues/5005
  config.ssh.insert_key = false

  config.vm.provision "ansible_local" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "playbook.yml"
  end

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
  end
end