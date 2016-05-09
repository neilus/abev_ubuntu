# vi - set ft=ruby
Vagrant.require_version  ">= 1.8.1"
Vagrant.configure(2) do |config|

  config.vm.define "base", autostart: false do |base|
    base.vm.box = "ubuntu/trusty64"
    base.vm.hostname = "base"

    base.vm.provider "virtualbox" do |vbox|
      vbox.name = "Base for Abev"
      # vbox.linked_clone = true
      vbox.cpus = 2
      vbox.memory = 1024
    end

    base.vm.post_up_message = "package --base base --output base.box --vagrantfile Vagrantfile"
    base.vm.post_up_message += "vagrant box add base.box"
    base.vm.post_up_message += "vagrant up"
  end

  config.vm.provision "base", type: "shell", inline: <<-SHELL
    apt-get install -y -q python-pip
    sudo pip install --quiet --log /tmp/pip.log --timeout 60 ansible==1.9.6 || echo 'DAMMIT!' 1>&2
  SHELL

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook       = "./provision.yml"
    ansible.verbose        = true
    ansible.install        = true
    ansible.version        = "1.9.6"
    ansible.limit          = "all"
    ansible.inventory_path = "./inventory"
  end

  config.vm.define "abevjava", primary: true do |abevjava|
    abevjava.vm.box = "ubuntu/trusty64" #"base"
    abevjava.vm.hostname = "abevjava"

    abevjava.vm.provider "virtualbox" do |vbox|
      vbox.name = "Abev Java"
      # vbox.linked_clone = true
      # vbox.customize ["modifyvm", :id, "--cpuexecutioncap", "50", "--vrde", "on", "--vrdeport", "3369"]
      vbox.gui = true
    end
  end
end
