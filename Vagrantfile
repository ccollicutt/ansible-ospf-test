# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.ssh.forward_agent = true

  config.vm.define "router" do |router|
    router.vm.synced_folder '.', '/vagrant', disabled: true
    router.vm.hostname = "router"
    router.vm.box = "tmatilai/openbsd-5.6"
    # External
    router.vm.network :private_network, ip: "10.0.10.2",
    :netmask => "255.255.255.0"
    # Internal
    router.vm.network :private_network, ip: "172.0.1.2",
    :netmask => "255.255.255.0"
    router.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
      v.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
    end

    router.vm.provision "shell",
    inline: "echo 'nameserver 8.8.8.8' > /etc/resolv.conf"

    router.vm.provision "shell",
    inline: "export PKG_PATH=http://ftp.openbsd.org/pub/OpenBSD/`uname -r`/packages/`arch -s` && pkg_add python-2.7.8"
  end

  config.vm.define "zone0" do |zone0|
    zone0.vm.hostname = "zone0"
    zone0.ssh.insert_key = false
    zone0.apt_proxy.http  = "http://192.168.122.1:3142"
    zone0.apt_proxy.https = "DIRECT"
    zone0.vm.box = "ubuntu/trusty64"
    zone0.vm.network :private_network, ip: "172.0.1.10",
    :netmask => "255.255.255.0"
    zone0.vm.provider "virtualbox" do |v|
      v.memory = 768
      v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
    end
  end

  config.vm.define "zone1" do |zone1|
    zone1.vm.hostname = "zone1"
    zone1.ssh.insert_key = false
    zone1.apt_proxy.http  = "http://192.168.122.1:3142"
    zone1.apt_proxy.https = "DIRECT"
    zone1.vm.box = "ubuntu/trusty64"
    zone1.vm.network :private_network, ip: "172.0.1.11",
    :netmask => "255.255.255.0"
    zone1.vm.provider "virtualbox" do |v|
      v.memory = 768
      v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
    end
  end

  config.vm.define "client" do |client|
    client.vm.hostname = "client"
    client.ssh.insert_key = false
    client.apt_proxy.http  = "http://192.168.122.1:3142"
    client.apt_proxy.https = "DIRECT"
    client.vm.box = "ubuntu/trusty64"
    client.vm.network :private_network, ip: "10.0.10.10",
    :netmask => "255.255.255.0"
    client.vm.provider "virtualbox" do |v|
      v.memory = 512
    end
  end

end
