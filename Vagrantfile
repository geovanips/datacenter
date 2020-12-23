# -*- mode: ruby -*-
# vi: set ft=ruby :

#################################
#Aluno: Geovani Pereira da Silva

################################
$script_apache = <<-SCRIPT
apt-get update
apt-get -y install apache2
SCRIPT
$script_mysql = <<-SCRIPT
apt-get update
apt-get -y install mysql-server
SCRIPT

$script_ansible = <<-SCRIPT
apt-get update
apt-get -y install software-properties-common
apt-add-repository --yes --update ppa:ansible/ansible
apt-get install -y ansible
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
    config.vm.define "apache" do |apache|
      apache.vm.hostname = "apache"
      apache.vm.network "forwarded_port", guest: 80, host: 8080
      apache.vm.network "public_network", ip: "192.168.1.72"

      #apache.vm.provision "shell", inline: $script_apache
      apache.vm.synced_folder ".", "/vagrant", disable: true
      apache.vm.synced_folder "./apache", "/vagrant"

      apache.vm.provision "shell", inline: "cat /vagrant/id_rsa.pub >> .ssh/authorized_keys"
    end
    config.vm.define "mysql" do |mysql|
      mysql.vm.hostname = "mysql"
      mysql.vm.network "public_network", ip: "192.168.1.70"

      mysql.vm.synced_folder ".", "/vagrant", disable: true
      mysql.vm.synced_folder "./mysql", "/vagrant"

      mysql.vm.provision "shell", inline: "cat /vagrant/id_rsa.pub >> .ssh/authorized_keys"
    end

  config.vm.define "ansible" do |ansible|
    ansible.vm.hostname ="ansible"
    ansible.vm.network "public_network", ip: "192.168.1.73"

    ansible.vm.synced_folder ".", "/vagrant", disable: true
    ansible.vm.synced_folder "./ansible", "/vagrant"
    ansible.vm.provision "shell", inline: $script_ansible
    ansible.vm.provision "shell", #inline: "cat /vagrant/id_rsa.pub >> .ssh/authorized_keys"
      inline: "cp /vagrant/id_rsa /home/vagrant && \
      chmod 600 /home/vagrant/id_rsa && \
      chown vagrant:vagrant /home/vagrant/id_rsa"
    ansible.vm.provision "shell",
        inline: "ansible-playbook -i /vagrant/hosts /vagrant/playbook.yml"
  end
end

  # Disable automa tic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port


  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.


  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
