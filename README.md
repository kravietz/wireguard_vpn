wireguard_vpn
=========

Configure a IPv4/IPv6 Wireguard-based VPN service using `systemd-networkd` and generate client config files.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

```yaml
---
vpn_private_key: WG GENKEY (USE ANSIBLE-VAULT TO PROTECT)
vpn_public_key: WG PUBKEY
# UDP listening port on public Internet
vpn_listen_port: 1194
# Addresses on internal VPN tunnel
vpn_address4: "192.168.2.252/24"
vpn_address6: '2a05:1111:0:3:8000::252/65'

vpn_clients:
  - name: pine2
    address4: 192.168.2.110
    address6: "2a05:1111:0:3:8000::110"
    private_key: WG GENKEY (USE ANSIBLE-VAULT TO PROTECT)
    public_key: WG PUBKEY

  - name: fairphone4
    address4: 192.168.2.120
    address6: "2a05:1111:0:3:8000::120"
    private_key: WG GENKEY (USE ANSIBLE-VAULT TO PROTECT)
    public_key: WG PUBKEY
```

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

GPLv3

Author Information
------------------

Paweł Krawczyk https://krvtz.net/