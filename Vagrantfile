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
DENOVO_SOEKRIS_MGMT_IP = "192.168.103.1"



Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"


  # Cory Soekris - PILO SDN Controller
  config.vm.define "cory-soekris" do |server|
    server.vm.box = "ubuntu/trusty64"
    server.vm.hostname = "cory-soekris"

    # AF links
    server.vm.network "private_network",
                      virtualbox__intnet: "cory_icsi_1",
                      ip: CORY_ICSI_IP_1,
                      netmask: "255.255.255.0"

    server.vm.network "private_network",
                      virtualbox__intnet: "cory_icsi_2",
                      ip: CORY_ICSI_IP_2,
                      netmask: "255.255.255.0"

    # Management/Control network
    server.vm.network "private_network",
                      ip: CORY_SOEKRIS_MGMT_IP,
                      netmask: "255.255.255.0"

    server.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", 512]
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end

    config.vm.provision :ansible do |ansible|
      ansible.playbook = "ansible/soekris.yml"
    end
  end

  # ICSI Soekris - PILO SDN Client
  config.vm.define "icsi-soekris" do |server|
    server.vm.box = "ubuntu/trusty64"
    server.vm.hostname = "icsi-soekris"

    # AF links
    server.vm.network "private_network",
                      virtualbox__intnet: "cory_icsi_1",
                      ip: ICSI_CORY_IP_1,
                      netmask: "255.255.255.0"

    server.vm.network "private_network",
                      virtualbox__intnet: "cory_icsi_2",
                      ip: ICSI_CORY_IP_2,
                      netmask: "255.255.255.0"

    server.vm.network "private_network",
                      virtualbox__intnet: "cory_denovo_1",
                      ip: ICSI_DENOVO_IP_1,
                      netmask: "255.255.255.0"

    server.vm.network "private_network",
                      virtualbox__intnet: "cory_denovo_2",
                      ip: ICSI_DENOVO_IP_2,
                      netmask: "255.255.255.0"

    # Management/Control network
    server.vm.network "private_network",
                      ip: ICSI_SOEKRIS_MGMT_IP,
                      netmask: "255.255.255.0"

    server.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", 512]
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end

    config.vm.provision :ansible do |ansible|
      ansible.playbook = "ansible/soekris.yml"
    end
  end

  # ICSI Soekris - PILO SDN Client
  config.vm.define "denovo-soekris" do |server|
    server.vm.box = "ubuntu/trusty64"
    server.vm.hostname = "denovo-soekris"

    # AF links
    server.vm.network "private_network",
                      virtualbox__intnet: "denovo_icsi_1",
                      ip: DENOVO_ICSI_IP_1,
                      netmask: "255.255.255.0"

    server.vm.network "private_network",
                      virtualbox__intnet: "denovo_icsi_2",
                      ip: DENOVO_ICSI_IP_2,
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
      ansible.playbook = "ansible/soekris.yml"
    end
  end



#   # "INTERNET"
#   config.vm.define "internet" do |server|
#     server.vm.hostname = "internet"

#     # Server 1 tunnel transport network
#     server.vm.network "private_network",
#                       virtualbox__intnet: "server1_net",
#                       ip: SERVER1_TUNNEL_GATEWAY,
#                       netmask: "255.255.255.0"

#     # Server 2 tunnel transport network
#     server.vm.network "private_network",
#                       virtualbox__intnet: "server2_net",
#                       ip: SERVER2_TUNNEL_GATEWAY,
#                       netmask: "255.255.255.0"

#     # Management/Control network
#     server.vm.network "private_network",
#                       ip: INTERNET_MGMT_IP,
#                       netmask: "255.255.255.0"

#     server.vm.provider "virtualbox" do |vb|
#       vb.customize ["modifyvm", :id, "--memory", 512]
#       vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
#     end

#     # Not persistent across reboots!
#     server.vm.provision "shell", inline: "echo 1 > /proc/sys/net/ipv4/ip_forward"
#     server.vm.provision "shell", inline: "echo 'net.ipv4.ip_forward = 1' >> /etc/sysctl.conf"

#     server.vm.provision "puppet" do |puppet|
#       puppet.manifests_path = "puppet/manifests"
#       puppet.manifest_file  = "site.pp"
#       puppet.options = "--verbose --debug"
#     end
#   end


#   # SERVER 1
#   config.vm.define "server1" do |server|
#     server.vm.hostname = "server1"

#     # Tunnel transport network
#     server.vm.network "private_network",
#                       virtualbox__intnet: "server1_net",
#                       ip: SERVER1_TUNNEL_IP,
#                       netmask: "255.255.255.0"

#     # Management/Control network
#     server.vm.network "private_network",
#                       ip: SERVER1_MGMT_IP,
#                       netmask: "255.255.255.0"

#     server.vm.provider "virtualbox" do |vb|
#       vb.customize ["modifyvm", :id, "--memory", 512]
#       vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
#     end

#     # Not persistent across reboots!
#     server.vm.provision "shell", inline: "route add #{SERVER2_TUNNEL_IP}/32 gw #{SERVER1_TUNNEL_GATEWAY} dev eth1"

#     server.vm.provision "puppet" do |puppet|
#       puppet.manifests_path = "puppet/manifests"
#       puppet.manifest_file  = "site.pp"
#       puppet.options = "--verbose --debug"
#     end
#   end


#   # SERVER 2
#   config.vm.define "server2" do |server|
#     server.vm.hostname = "server2"

#     # Tunnel transport network
#     server.vm.network "private_network",
#                       virtualbox__intnet: "server2_net",
#                       ip: SERVER2_TUNNEL_IP,
#                       netmask: "255.255.255.0"

#     # Management/Control network
#     server.vm.network "private_network",
#                       ip: SERVER2_MGMT_IP,
#                       netmask: "255.255.255.0"

#     server.vm.provider "virtualbox" do |vb|
#       vb.customize ["modifyvm", :id, "--memory", 512]
#       vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
#     end

#     # Not persistent across reboots!
#     server.vm.provision "shell", inline: "route add #{SERVER1_TUNNEL_IP}/32 gw #{SERVER2_TUNNEL_GATEWAY} dev eth1"

#     server.vm.provision "puppet" do |puppet|
#       puppet.manifests_path = "puppet/manifests"
#       puppet.manifest_file  = "site.pp"
#       puppet.options = "--verbose --debug"
#     end
#   end


end
