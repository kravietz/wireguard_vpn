wireguard_vpn
=========

Configure a IPv4/IPv6 Wireguard-based VPN service using `systemd-networkd` and generate client config files.

Description
-----------
The role configures a Wireguard-based VPN service on a Linux box with the following distinguishing features:

* Network configuration managed using `systemd-networkd`
* IPv4 network using RFC 1918 addressing and NAT
* IPv6 network using fully routable subnet
* Generates client .conf files

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
# required to set up NDP entries
# vpn_ndp_config: /etc/systemd/network/eth0.network

# where client config files will be output
vpn_clients_dir: /root/wireguard
vpn_clients:
  - name: client1
    address4: 192.168.2.110
    address6: "2a05:1111:0:3:8000::110"
    private_key: WG GENKEY (USE ANSIBLE-VAULT TO PROTECT)
    public_key: WG PUBKEY

  - name: client2
    address4: 192.168.2.120
    address6: "2a05:1111:0:3:8000::120"
    private_key: WG GENKEY (USE ANSIBLE-VAULT TO PROTECT)
    public_key: WG PUBKEY
```
**Warning:** always use `ansible-vault` to protect private keys in your variable files.


Example Playbook
----------------

```yaml
- hosts: vpn
  roles:
      - role: kravietz.wireguard_vpn
```

License
-------

GPLv3

Author Information
------------------

Paweł Krawczyk https://krvtz.net/