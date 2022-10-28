wireguard_vpn
=========

Configure a client-to-server Wireguard VPN service and provision client config files.

Description
-----------
The role configures a Wireguard-based VPN service on a Linux box with the following distinguishing features:

* All network configuration managed using `systemd-networkd`
* IPv4 networking in tunnel using RFC 1918 addressing and NAT
* IPv6 networking in tunnel using fully routable subnet and NDP proxying
* Generates client `.conf` files for `wg-quick`

Role Variables
--------------

```yaml
# where to write `.conf` files for clients
vpn_clients_dir: /root/wireguard  # OPTIONAL
vpn_interface: wg1                # OPTIONAL, defaults to `wg1`

# REQUIRED
# client private key generated with `wg genkey`
vpn_private_key: "iP/lnL/zhicPMnAphlT6qAciKusTmR2X5utTAo7u5Ug=" # protect with ansible-vault
vpn_public_key: "OBGsZZxxX0jcehmFJc0L6v7FX3PMnVDFgdgpjJFU0k4="

# REQUIRED
# UDP listening port on public Internet
vpn_listen_port: 88  # REQUIRED 

# Addresses on internal VPN tunnel
# at least one is REQUIRED
vpn_address4: "192.168.2.252/24"
vpn_address6: '2a05:1111:0:3:8000::252/65'

# required to set up NDP entries
vpn_ndp_config: /etc/systemd/network/eth0.network  # OPTIONAL

# DNS resolvers which will be set at clients for use with VPN
vpn_dns_resolvers: ["9.9.9.9", "2620:fe::fe"]  # OPTIONAL

# where client config files will be output
vpn_clients_dir: /root/wireguard
vpn_clients:
   - name: client1                       # REQUIRED
     address4: 192.168.2.110             # at least one of address4, address6 is REQUIRED
     address6: "2a05:1111:0:3:8000::110" # at least one of address4, address6 is REQUIRED
     public_key: "RfDKFurwFo/ytXd9Ko5oEy7I7H4hjNBiT1bc1t+V4Wc="   # REQUIRED
     # client private key generated with `wg genkey`, only needed to add to `.conf` file
     private_key: "+Noalz2HL9+nYFQpplZF2dYMmc7+MaXGuxMgc/QBbXU="  # OPTIONAL protect with ansible-vault
     routing: ["192.168.2.1/32"]         # OPTIONAL, defaults to 0.0.0.0/0, ::/0
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