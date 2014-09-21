#!/usr/bin/env ruby
Vagrant.configure("2") do |config|

  config.vm.box = "trusty64"
  config.vm.box_url = 'https://vagrantcloud.com/ubuntu/trusty64/version/1/provider/virtualbox.box'

  # config.vm.provider :virtualbox do |virtualbox|
  #   virtualbox.customize ["modifyvm", :id, "--memory", 512]
  # end

  config.vm.define :db1 do |db|
    db.vm.box = "trusty64"
    db.vm.hostname = "db1"
    # db.vm.network :forwarded_port, guest: 22, host: 2253
    db.vm.network :private_network, ip: "192.168.111.253"
  end

  config.vm.define :app1 do |app|
    app.vm.box = "trusty64"
    app.vm.hostname = "app1"
    # app.vm.network :forwarded_port, guest: 22, host: 2101
    app.vm.network :private_network, ip: "192.168.111.101"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.host_key_checking = false
    ansible.ask_vault_pass = true
    ansible.verbose = "vvvv"
    ansible.groups = {
      "appservers" => ["app1"],
      "dbservers" => ["db1"],
      "all_groups:children" => ["appservers", "dbservers"]
    }
    ansible.extra_vars = {
      vagrant: true,
      webserver_ip: "192.168.111.101",
      db_ip: "192.168.111.253",
      ssl: true
    }
  end

end
