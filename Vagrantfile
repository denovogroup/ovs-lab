# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

PILO_NETMASK = "255.255.0.0"
MGMT_NETMASK = "255.255.255.0"
PILO_BITMASK = "16"
UDP_PORT = 5005
PILO_BROADCAST = "10.1.255.255"
RETRANSMISSION_TIMEOUT = 3
HEARTBEAT_TIMEOUT = 30

CORY_ICSI_IP_1       = "10.1.1.1"
CORY_ICSI_IP_2       = "10.1.2.1"
CORY_SOEKRIS_MGMT_IP = "10.101.1.2"
CORY_PILO_MAC = "00:00:00:00:00:01"
CORY_PILO_IP = "10.1.100.1"

ICSI_CORY_IP_1       = "10.1.1.2"
ICSI_CORY_IP_2       = "10.1.2.2"
ICSI_DENOVO_IP_1     = "10.1.3.1"
ICSI_DENOVO_IP_2     = "10.1.4.1"
ICSI_SOEKRIS_MGMT_IP = "10.101.2.2"
ICSI_PILO_MAC = "00:00:00:00:00:02"
ICSI_PILO_IP = "10.1.100.2"

DENOVO_ICSI_IP_1       = "10.1.3.2"
DENOVO_ICSI_IP_2       = "10.1.4.2"
DENOVO_BTS_IP_1        = "10.1.5.1"
DENOVO_SOEKRIS_MGMT_IP = "10.101.3.2"
DENOVO_PILO_MAC = "00:00:00:00:00:03"
DENOVO_PILO_IP = "10.1.100.3"

DENOVO_CPE_1           = "10.1.5.2"
DENOVO_CPE_1_MGMT_IP   = "10.101.4.2"

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
                      netmask: PILO_NETMASK

    # eth2
    server.vm.network "private_network",
                      virtualbox__intnet: "cory_icsi_2",
                      ip: CORY_ICSI_IP_2,
                      netmask: PILO_NETMASK

    # eth3
    # Management/Control network
    server.vm.network "private_network",
                      ip: CORY_SOEKRIS_MGMT_IP,
                      netmask: MGMT_NETMASK

    server.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", 512]
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
      # Can be used for debugging if Vagrant doesn't bring up interfaces properly
      # vb.gui = true
    end

    config.vm.provision :ansible do |ansible|
      ansible.extra_vars = {
        ovs_interfaces: "eth1 eth2",
        pilo_client: false,
        pilo_controller: true,
        br_mac: CORY_PILO_MAC,
        br_addr: CORY_PILO_IP + "/" + PILO_BITMASK,
        br_ip: CORY_PILO_IP,
        udp_ip: PILO_BROADCAST,
        br_interfaces: "eth1 eth2",
        retransmission_timeout: RETRANSMISSION_TIMEOUT,
        heartbeat_timeout: HEARTBEAT_TIMEOUT,
        udp_port: UDP_PORT,
        client_macs: ICSI_PILO_MAC + "," + DENOVO_PILO_MAC
      }
      # ansible.limit = 'all'
      ansible.verbose = 'vvvv'
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
                      netmask: PILO_NETMASK

    # eth2
    server.vm.network "private_network",
                      virtualbox__intnet: "cory_icsi_2",
                      ip: ICSI_CORY_IP_2,
                      netmask: PILO_NETMASK

    # eth3
    server.vm.network "private_network",
                      virtualbox__intnet: "icsi_denovo_1",
                      ip: ICSI_DENOVO_IP_1,
                      netmask: PILO_NETMASK

    # eth4
    server.vm.network "private_network",
                      virtualbox__intnet: "icsi_denovo_2",
                      ip: ICSI_DENOVO_IP_2,
                      netmask: PILO_NETMASK

    # eth5
    # Management/Control network
    server.vm.network "private_network",
                      ip: ICSI_SOEKRIS_MGMT_IP,
                      netmask: MGMT_NETMASK

    server.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", 512]
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end

    config.vm.provision :ansible do |ansible|
      ansible.extra_vars = {
        ovs_interfaces: "eth1 eth2 eth3 eth4",
        pilo_client: true,
        pilo_controller: false,
        retransmission_timeout: RETRANSMISSION_TIMEOUT,
        heartbeat_timeout: HEARTBEAT_TIMEOUT,
        br_mac: ICSI_PILO_MAC,
        udp_ip: PILO_BROADCAST,
        pilo_addr: ICSI_PILO_IP + "/" + PILO_BITMASK,
        pilo_ip: ICSI_PILO_IP,
        udp_port: UDP_PORT,
        controller_mac: CORY_PILO_MAC
      }
      # ansible.limit = 'all'
      ansible.verbose = 'vvvv'
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
                      netmask: PILO_NETMASK

    # eth2
    server.vm.network "private_network",
                      virtualbox__intnet: "icsi_denovo_2",
                      ip: DENOVO_ICSI_IP_2,
                      netmask: PILO_NETMASK

    # eth3
    server.vm.network "private_network",
                      virtualbox__intnet: "denovo_bts_1",
                      ip: DENOVO_BTS_IP_1,
                      netmask: PILO_NETMASK

    # Management/Control network
    server.vm.network "private_network",
                      ip: DENOVO_SOEKRIS_MGMT_IP,
                      netmask: MGMT_NETMASK

    server.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", 512]
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end

    config.vm.provision :ansible do |ansible|
      ansible.extra_vars = {
        ovs_interfaces: "eth1 eth2 eth3",
        pilo_client: true,
        pilo_controller: false,
        br_mac: DENOVO_PILO_MAC,
        udp_ip: PILO_BROADCAST,
        retransmission_timeout: RETRANSMISSION_TIMEOUT,
        heartbeat_timeout: HEARTBEAT_TIMEOUT,
        pilo_addr: DENOVO_PILO_IP + "/" + PILO_BITMASK,
        pilo_ip: DENOVO_PILO_IP,
        udp_port: UDP_PORT,
        controller_mac: CORY_PILO_MAC
      }
      # ansible.limit = 'all'
      ansible.verbose = 'vvvv'
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
                      netmask: PILO_NETMASK

    # Management/Control network
    server.vm.network "private_network",
                      ip: DENOVO_CPE_1_MGMT_IP,
                      netmask: MGMT_NETMASK

    server.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", 512]
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end

    config.vm.provision :ansible do |ansible|
      ansible.verbose = 'vvvv'
      ansible.playbook = "ansible/cpe.yml"
    end
  end


end
