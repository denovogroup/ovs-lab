# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# NOTE: Netmask is assumed 255.255.255.0 for all

CORY_ICSI_IP_1       = "10.1.1.1"
CORY_ICSI_IP_2       = "10.1.2.1"
CORY_SOEKRIS_MGMT_IP = "192.168.101.1"

ICSI_CORY_IP_1       = "10.1.1.2"
ICSI_CORY_IP_2       = "10.1.2.2"
ICSI_DENOVO_IP_1     = "10.1.3.1"
ICSI_DENOVO_IP_2     = "10.1.4.1"
ICSI_SOEKRIS_MGMT_IP = "192.168.102.1"

DENOVO_ICSI_IP_1       = "10.1.3.2"
DENOVO_ICSI_IP_2       = "10.1.4.2"
DENOVO_BTS_IP_1        = "10.1.5.1"
DENOVO_SOEKRIS_MGMT_IP = "192.168.103.1"

DENOVO_CPE_1           = "10.1.5.2"
DENOVO_CPE_1_MGMT_IP   = "192.168.104.1"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"


  # Cory Soekris - PILO SDN Controller
  config.vm.define "cory-soekris" do |server|
    server.vm.box = "ubuntu/trusty64"
    server.vm.hostname = "cory-soekris"

    # AF links
    # eth1
    server.vm.network "private_network",
                      virtualbox__intnet: "cory_icsi_1",
                      ip: CORY_ICSI_IP_1,
                      netmask: "255.255.255.0",

    # eth2
    server.vm.network "private_network",
                      virtualbox__intnet: "cory_icsi_2",
                      ip: CORY_ICSI_IP_2,
                      netmask: "255.255.255.0"

    # eth3
    # Management/Control network
    server.vm.network "private_network",
                      ip: CORY_SOEKRIS_MGMT_IP,
                      netmask: "255.255.255.0"

    server.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", 512]
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end

    config.vm.provision :ansible do |ansible|
      ansible.extra_vars = {
        ovs_interfaces: "eth1 eth2",
        pilo_client: false,
        pilo_controller: true,
        br_mac: "00:00:00:00:00:01",
        br_ip: "10.0.0.1/24",
        udp_ip: "10.0.0.255",
        br_interfaces: "eth1 eth2",
        ack_timer: 5,
        udp_ip: "10.0.0.255",
        udp_port: 5005,
        tmp_dst_mac: "00:00:00:00:00:02"

      }
      ansible.playbook = "ansible/soekris.yml"
    end
  end

  # ICSI Soekris - PILO SDN Client
  config.vm.define "icsi-soekris" do |server|
    server.vm.box = "ubuntu/trusty64"
    server.vm.hostname = "icsi-soekris"

    # AF links
    # eth1
    server.vm.network "private_network",
                      virtualbox__intnet: "cory_icsi_1",
                      ip: ICSI_CORY_IP_1,
                      netmask: "255.255.255.0"

    # eth2
    server.vm.network "private_network",
                      virtualbox__intnet: "cory_icsi_2",
                      ip: ICSI_CORY_IP_2,
                      netmask: "255.255.255.0"

    # eth3
    server.vm.network "private_network",
                      virtualbox__intnet: "icsi_denovo_1",
                      ip: ICSI_DENOVO_IP_1,
                      netmask: "255.255.255.0"

    # eth4
    server.vm.network "private_network",
                      virtualbox__intnet: "icsi_denovo_2",
                      ip: ICSI_DENOVO_IP_2,
                      netmask: "255.255.255.0"

    # eth5
    # Management/Control network
    server.vm.network "private_network",
                      ip: ICSI_SOEKRIS_MGMT_IP,
                      netmask: "255.255.255.0"

    server.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", 512]
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end

    config.vm.provision :ansible do |ansible|
      ansible.extra_vars = {
        ovs_interfaces: "eth1 eth2 eth3 eth4",
        pilo_client: true,
        pilo_controller: false,
        br_mac: "00:00:00:00:00:02",
        udp_ip: "10.0.0.255",
        pilo_addr: "10.0.0.2/24",
        udp_port: 5005,
        controller_mac: "00:00:00:00:00:01"
      }
      ansible.playbook = "ansible/soekris.yml"
    end
  end

  # Denovo office Soekris - PILO SDN Client
  # TODO: replace with openwrt?
  config.vm.define "denovo-soekris" do |server|
    server.vm.box = "ubuntu/trusty64"
    server.vm.hostname = "denovo-soekris"

    # AF links
    # eth1
    server.vm.network "private_network",
                      virtualbox__intnet: "icsi_denovo_1",
                      ip: DENOVO_ICSI_IP_1,
                      netmask: "255.255.255.0"

    # eth2
    server.vm.network "private_network",
                      virtualbox__intnet: "icsi_denovo_2",
                      ip: DENOVO_ICSI_IP_2,
                      netmask: "255.255.255.0"

    # eth3
    server.vm.network "private_network",
                      virtualbox__intnet: "denovo_bts_1",
                      ip: DENOVO_BTS_IP_1,
                      netmask: "255.255.255.0"

    # Management/Control network
    server.vm.network "private_network",
                      ip: DENOVO_SOEKRIS_MGMT_IP,
                      netmask: "255.255.255.0"

    server.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", 512]
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end

    config.vm.provision :ansible do |ansible|
      ansible.extra_vars = {
        ovs_interfaces: "eth1 eth2",
        pilo_client: true,
        pilo_controller: false,
        br_mac: "00:00:00:00:00:03"
        udp_ip: "255.255.255.255",
        pilo_addr: "10.0.0.3/24",
        udp_port: 5005,
        controller_mac: "00:00:00:00:00:01"
      }
      ansible.playbook = "ansible/soekris.yml"
    end
  end

  # Denovo CPE simple IP connected to denovo-soekris and not running PILO client
  config.vm.define "denovo-cpe-1" do |server|
    server.vm.box = "ubuntu/trusty64"
    server.vm.hostname = "denovo-cpe-1"

    # AF links
    server.vm.network "private_network",
                      virtualbox__intnet: "denovo_bts_1",
                      ip: DENOVO_CPE_1,
                      netmask: "255.255.255.0"

    # Management/Control network
    server.vm.network "private_network",
                      ip: DENOVO_CPE_1_MGMT_IP,
                      netmask: "255.255.255.0"

    server.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", 512]
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end

    config.vm.provision :ansible do |ansible|
      ansible.playbook = "ansible/cpe.yml"
    end
  end


end
