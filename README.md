Open vSwitch Lab
================

This is Denovo's fork of relaxdiego's ovs-lab for developing an in band
SDN implementation

Much thanks to relaxdiego:
https://github.com/relaxdiego/ovs-lab
http://www.relaxdiego.com.


Requirements
------------

1. VirtualBox. Install from https://www.virtualbox.org/wiki/Downloads

2. Vagrant v1.6.3 or above. Install from http://www.vagrantup.com/downloads.html

3. The vagrant-vbguest plugin. Install with `vagrant plugin install vagrant-vbguest`

4. Install ansible. Depending on OS, there are various options, but on OSX should be as easy as `pip install ansible`

Installation
------------

Estimated time for the following steps including automated provisioning:
30 minutes. Time will vary depending on your connection speed.

1. Clone this repo and cd to it

2. Run `vagrant up`


Stopping and starting the VMs
-----------------------------

To suspend your VMs, run `vagrant suspend`. To shut them down, run
`vagrant halt`.


Different VMs
-----------------------------
There are 4 different VMs, cory-soekris, icsi-soekris, denovo-soekris, and denovo-cpe-1.
cory-soekris is the pilo_controller
icsi-soekris and denovo-soekris are both pilo_clients
denovo-cpe-1 just represents a simple network connected host


Starting pilo
-----------------
For now you have to ssh into each machine:
`vagrant ssh cory-soekris` (from this folder)
`vagrant ssh icsi-soekris`
`vagrant ssh denovo-soekris`

On each box, you should be able to run the following:
`/etc/init.d/pilo_controller start'
or
`/etc/init.d/pilo_client start'


Configuration
-------------
Most of the relevant configurations are at the top of the Vagrantfile. 
This folder is mounted on each VM as /vagrant. 

So, the pox folder will be at /vagrant/shared/pox


Vagrant Commands
----------------
http://docs.vagrantup.com/v2/cli/index.html


Troubleshooting
---------------

PROBLEM: I'm getting an error about Vagrant not able to compile nokogiri

SOLUTION: You're missing developer tools needed to locally compile binaries
needed by nokogiri. In OS X, install the OS X Developer Tools for your OS X
version.
