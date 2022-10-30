wireguard_vpn
=========

Configure a client-to-server Wireguard VPN service and provision client config files.

Description
-----------
The role configures a Wireguard-based VPN service on a Linux box with the following distinguishing features:

* All network configuration managed using `systemd-networkd`
* IPv4 networking in tunnel using RFC 1918 addressing and NAT
* IPv6 networking in tunnel using fully routable subnet and NDP proxying
* Support for Wireguard pre-shared keys
* Generates client `.conf` files for `wg-quick(8)`
* All `wg-quick(8)` configuration options supported

Role Variables
--------------

```yaml
vpn_interface: wg1

# generate with `wg genkey` ***protect with ansible-vault***
# vpn_private_key: "iP/lnL/zhicPMnAphlT6qAciKusTmR2X5utTAo7u5Ug="  # REQUIRED

# generate with `wg pubkey` from the above
# vpn_public_key: "OBGsZZxxX0jcehmFJc0L6v7FX3PMnVDFgdgpjJFU0k4="   # REQUIRED

## UDP listening port on public Internet
# vpn_listen_port: 1194  # REQUIRED

## Addresses on internal VPN tunnel
# vpn_address4: "192.168.2.252/24"            # at least one is REQUIRED      
# vpn_address6: '2a05:1111:0:3:8000::252/65'  # at least one is REQUIRED

## required to set up NDP entries for IPv6 VPN clients
# vpn_ndp_config: /etc/systemd/network/eth0.network  # OPTIONAL

## DNS resolvers which will be set at clients for use with VPN
# vpn_dns_resolvers: ["9.9.9.9", "2620:fe::fe"]  # OPTIONAL array

# enables generation of .conf files for clients' wg-quick utility
# vpn_clients_dir: /root/wireguard  # OPTIONAL

# vpn_clients:                                # OPTIONAL array
#   - name: client1                           # REQUIRED
#     address4: 192.168.2.110/32              # at least one of address4, address6 is REQUIRED
#     address6: "2a05:1111:0:3:8000::110/80"  # at least one of address4, address6 is REQUIRED
#     # only needed if generated .config files should come with private key already included
#     # otherwise they will contain a placeholder
#     # generate with `wg genkey` ***protect with ansible-vault***
#     # private_key: "+Noalz2HL9+nYFQpplZF2dYMmc7+MaXGuxMgc/QBbXU="  # OPTIONAL
#     # generate with `wg pubkey` from the above
#     public_key: "RfDKFurwFo/ytXd9Ko5oEy7I7H4hjNBiT1bc1t+V4Wc="              # REQUIRED
#     # tunnel route to set on client side
#     # allowed_ips: ["192.168.2.1/32"]         # OPTIONAL
#     # generate with `wg genpsk` ***protect with ansible-vault***
#     # psk: "151ODHNbvmiK/ox+2ndnZbVcfrIMRJjFjHXlb7o3ZeI="  # OPTIONAL
#     # mtu, table, preup, postdown - per wg-quick(8)
```
**Warning:** always use `ansible-vault` to protect private keys and pre-shared secrets in your variable files.


Example Playbook
----------------

```yaml
- hosts: vpn
  roles:
    - role: kravietz.wireguard_vpn
      vpn_private_key: "wIfnNpua6YlD4XzVGUvOCVknCo1LzAF6iGkp7Tho43o="
      vpn_public_key: "+RHdC7oc8O/dojCOMf7CtYBZc5pA2DZPiE4dNRHHhlw="
      vpn_listen_port: 1194
      vpn_address4: "192.168.1.1/24"
      vpn_address6: '2a05:1111:0:3:8000::1/65'
      vpn_clients_dir: /root/wireguard
      vpn_clients:
        - name: client1
          address4: 192.168.1.100/32
          address6: "2a05:1111:0:3:8000::100/128"
          private_key: "iJgcqx21xGETtzFIIdKfD/LvMqswJ2LWUiFPKUBLenw="
          public_key: "T4QbCHfGKLYFdmFeXVfDHP5AYpQ2AZapHIw+ZiCDIHs="
```

License
-------

GPLv3

Author Information
------------------

Paweł Krawczyk https://krvtz.net/
