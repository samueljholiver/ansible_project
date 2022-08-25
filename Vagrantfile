$script_ansible = <<-SCRIPT
  apt-get update && \
  apt-get install -y software-properties-common && \
  apt-add-repository --yes --update ppa:ansible/ansible && \
  apt-get install -y ansible
SCRIPT

Vagrant.configure("2") do |config|

    config.vm.box = "ubuntu/bionic64"
    config.vm.box_download_insecure = true

    config.vm.define "wordpress" do |wordpress|
      wordpress.vm.network "private_network", ip: "10.2.0.11"
      wordpress.vm.provision "shell", inline: "cat /vagrant/configs/ansible_ssh_key.pub >> .ssh/authorized_keys"
      wordpress.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 2
        vb.name = "wordpress_server"
      end
    end
    
    config.vm.define "mysql" do |mysql|
      mysql.vm.network "private_network", ip: "10.2.0.12"
      mysql.vm.provision "shell", inline: "cat /vagrant/configs/ansible_ssh_key.pub >> .ssh/authorized_keys"
      mysql.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 2
        vb.name = "mysql_server"
      end
    end

    config.vm.define "ansible" do |ansible|
      ansible.vm.network "private_network", ip: "10.2.0.10"
      ansible.vm.provision "shell", inline: $script_ansible
      ansible.vm.synced_folder "./configs/", "/vagrant"

      ansible.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 2
        vb.name = "ansible_server"
        end
    end
end
