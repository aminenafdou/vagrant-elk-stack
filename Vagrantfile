# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # configure provisioning
  config.vm.provision "shell", path: "scripts/init.sh"

  # synced folders
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # VM configuration 
  config.vm.box = "generic/rhel7"
  config.vm.boot_timeout = 700 # shit !!! it's taking too much time to boot on my shitty...
  config.ssh.insert_key = true

  # VM dimensions
  config.vm.provider "virtualbox" do |vb|
      vb.cpus = 1
      vb.memory = 3000
      vb.gui = true
      vb.check_guest_additions = false
      vb.customize ["modifyvm", :id, "--usb", "off"]
      vb.customize ["modifyvm", :id, "--usbehci", "off"]
      vb.customize ["modifyvm", :id, "--ioapic", "on"]
      vb.customize ["modifyvm", :id, "--graphicscontroller", "vboxvga"]
      vb.customize ["modifyvm", :id, "--vram", "64"]
      vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
      vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
  end    
  
  ELASTIC_SEARCH = "elastic-search".freeze
  LOGSTASH_SEARCH = "logstash".freeze 
  KIBANA_SEARCH = "kibana".freeze

  boxes_names = [ELASTIC_SEARCH, LOGSTASH_SEARCH, KIBANA_SEARCH]

  boxes_names.each do |i|
      config.vm.define "node-#{i}" do |node|
        node.vm.hostname = "node-#{i}"

        if ELASTIC_SEARCH == "#{i}"
          # elastic search
          node.vm.network "forwarded_port", host: 9300, guest: 9300
          node.vm.network "private_network", ip: "192.168.100.1"
        end

        if LOGSTASH_SEARCH == "#{i}"
          # logstash
          node.vm.network "forwarded_port", host: 5000, guest: 5000
          node.vm.network "private_network", ip: "192.168.100.2"
        end

        if KIBANA_SEARCH == "#{i}"
           # kibana
           node.vm.network "forwarded_port", host: 5601, guest: 5601
           node.vm.network "private_network", ip: "192.168.100.3"
        end

      end
  end

end
