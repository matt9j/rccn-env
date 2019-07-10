# -*- mode: ruby -*-
# vi: set ft=ruby :

box = "debian/contrib-stretch64"

Vagrant.configure(2) do |config|

  config.vm.define :rccn do |rccn|
    rccn.vm.box = box
    rccn.vm.hostname = "rccn"
    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    rccn.vm.network "private_network", ip: "192.168.40.200"
    # Create a bridged public network for access to this VM on the lan.
    rccn.vm.network "public_network", ip: "192.168.99.90", bridge: "enp0s31f6"
    rccn.vm.network "forwarded_port", guest: 80, host: 8080
    rccn.vm.synced_folder "./sources/" , "/home/vagrant/sources", type: "virtualbox"

    rccn.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", "4096"]
      vb.customize ["modifyvm", :id, "--cpus", "3"]

      # Pass through USB control of attached USRP
      vb.customize ['modifyvm', :id, '--usb', 'on']
      vb.customize ['modifyvm', :id, '--usbehci', 'on']
      vb.customize ['modifyvm', :id, '--usbxhci', 'on']
      vb.customize ['usbfilter', 'add', '0', '--target', :id, '--name', 'B200mini',
                    '--vendorid', '0x2500', '--productid', '0x0021']
      vb.customize ['usbfilter', 'add', '0', '--target', :id, '--name', 'B200',
                    '--vendorid', '0x2500', '--productid', '0x0020']
      vb.customize ["modifyvm", :id, "--ioapic", "on"]
    end

    rccn.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.host_key_checking = false
      ansible.playbook = "ansible/rccn_local_dev.yml"
      ansible.raw_arguments = ['--timeout=20']
      ansible.verbose = 'v'
    end

  end

end
