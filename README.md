Role Name
=========

Ansible role to create OVS Bridges, add port(s) to OVS Bridges and configure /etc/network/interfaces for OVS

Requirements
------------

None.

Role Variables
--------------

````
---
# defaults file for ansible-openvswitch
add_repos: false  #defines if apt repos should be added for obtaining newer code...only for testing.
apt_repos:
  - ppa:project-calico/kilo  #openstack kilo repo
  - ppa:tomeichhorn/ovs  #openvswitch repo
ovs_bridges:
  - name: ext-br  #defines the name of the ovs bridge to create
    int: em1  #defines the interface(s) to add to bridge
    state: present  #defines if the bridge should exist or not
    config_etc_interfaces: true  #defines if /etc/network/interfaces should be updated to include the configuration..config between reboots
    comment: external network  #defines the comment if desired to add to /etc/network/interfaces
    method: dhcp  #defines the method on how the interface should be configured...static,dhcp or manual
    address:  #define IP address if method=static
    netmask:  #define network subnet mask if method=static
    gateway:  #define the gateway if required when method=static
    wireless_network: false  #defines if the interface is a wireless interface...not working so keep false or not defined
    wpa_ssid: wireless  #defines the wireless SSID to connect to
    wpa_psk: wpapassword  #defines the wireless key
  - name: int-br
#    int: eth1  #commented out to define an OVS Bridge w/out any interface(s)
    state: present
    config_etc_interfaces: true
    comment: internal network
    method: static
    address: 192.168.203.69
    netmask: 255.255.255.0
#    gateway: 172.16.24.1
uninstall: false  #defines is OVS should be uninstalled and OVS Bridges destroyed
````

Dependencies
------------

Recommended to use Ansible Galaxy role mrlesmithjr.config-interfaces along with this role to manage non OVS interfaces.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: mrlesmithjr.config-interfaces }
         - { role: mrlesmithjr.openvswitch }

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com