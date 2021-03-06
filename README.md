YasuhiroABE.homegw-iptables
=========

This role sets up iptables and ip6tables for the home router.
It expects the home router connected with pppoe to ISP like FLET's fiber-optic internet service.

### Note
Currently, this rule doesn't use the ansible iptables module.
This module relys on the ansible template module and iptable-restore and ip6table-restore commands in nature.

Requirements
------------

This role is tested on the following platforms.

### Ansible
- Version 2.4

### Distributions
- Ubuntu 16.04
- Debian 9

Role Variables
--------------

### Default 
    ## configuration filepath stored temporary.
    homegw_iptables_config_path: /etc/network/iptables.conf
    homegw_ip6tables_config_path: /etc/network/ip6tables.conf

    ## common setting for both iptables.conf and ip6tables.conf
    homegw_iptables_config_permission:
      owner: root
      group: root
      mode: '0644'

    homegw_iptables_script_permission:
      owner: root
      group: root
      mode: '0750'

    homegw_iptables_external_eth_device: 'eth0'
    homegw_iptables_internal_eth_device: 'eth1'

    # accept tcp port list
    homegw_iptables_external_accept_tcp_ports: []
	
	# accept udp port list
	homegw_iptables_external_accept_udp_ports: []

	# add path-through device into input chain (e.g. -A INPUT -i lo -j ACCEPT)
	homegw_iptables_accept_input_eth_devices: ['lo']

	# add path-through device into forward chain (e.g. -A FORWARD -i lo -o lo -j ACCEPT)
	homegw_iptables_accept_forward_eth_devices: [['lo','lo']]

	# add path-through device into output chain (e.g. -A OUTPUT -o lo -j ACCEPT)
	homegw_iptables_accept_output_eth_devices: ['lo']

Dependencies
------------

1. This role expects the machine having two different ethernet devices to provide external and internal connectivity.

2. This role is tested and optimized on the Japanese pppoe environment, the FLET's fiber-optic internet service.

Example Playbook
----------------

    - hosts: all
      vars:
        homegw_iptables_external_eth_device: 'eth0'
        homegw_iptables_internal_eth_device: 'br0'
        homegw_iptables_external_accept_tcp_ports: 
          - 22
          - 80
      roles:
         - YasuhiroABE.homegw-iptables

License
-------

Apache License 2.0

Author Information
------------------

[Yasuhiro ABE](http://www.yasundial.org/foaf.xml)

