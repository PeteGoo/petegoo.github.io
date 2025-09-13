---
layout: post
title: "Configuring a Wireguard client on Ubuntu to connect to your Unifi VPN Server"
categories:
- blog
tags:
- unifi
- uniquiti
- petegoo
- networking
- wireguard
- vpn
- ubuntu
status: publish
type: post
published: true
meta: {}
author:
  login: petegoo
  email: pete@petegoo.com
  display_name: PeteGoo
  first_name: Peter
  last_name: Goodman

---

Lately on a trip to the UK I was tring to configure my Ubuntu laptop to connect
back home to my Unifi Dream Machine Pro via Wireguard.

It ended up being an absolutely nightmare due to the hotel Wifi, IPv6, and the fact that
wireguard can be a little hard to configure.

So here are the simple steps to get it up and running.


__1. Create a new VPN Server on Unifi Network__

[This tutorial](https://www.unihosted.com/blog/ultimate-guide-to-setting-up-unifi-wire-guard-vpn-for-secure-connectivity) covers what you need.

__2. Create a new Client and download the config file__

Again use [this tutorial](https://www.unihosted.com/blog/ultimate-guide-to-setting-up-unifi-wire-guard-vpn-for-secure-connectivity) for the right steps to setup the Client on the Unifi Network Settings.

__3. Make sure you have wireguard installed__

```bash
sudo apt install && sudo apt install wireguard
```

__4. Create a new link device with the name you want to call your VPN connection__

```bash
sudo ip link add dev test-client type wireguard
```

__5. Rename the Client file to the same name as your connection__

```bash
mv ~/Downloads/<file>.conf ~/Downloads/test-client.conf
```

__6. Update the conf file to add DNS, PostUp, PostDown, and Allow ipv6 traffic.__

```toml
[Interface]
PrivateKey = <private key>
Address = 192.168.14.4/32 # Take this IP from the Interface IP of the Client in Unifi console
DNS = 192.168.14.1,1.1.1.1,8.8.8.8 # Add additional DNS Servers
# Add PostUp and PostDown iptables config
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

[Peer]
PublicKey = <public key for VPN Config from Unifi Console>
AllowedIPs = 0.0.0.0/0,::/0 # Add ipv6 catch-all
Endpoint = <Your WAN IP Address>:51820
```

__7. Use nmcli to import the conf file.__
```bash
sudo nmcli conn import type wireguard file ~/Downloads/<file>.conf
```

__8. Set ipv6 method to ignore.__
```bash
nmcli c modify test-client ipv6.method ignore
```


__9. Set autoconnect to false.__

We don't really want the connection to start on login
```bash
nmcli c modify test-client connection.autoconnect no
```

__10. Start the connection.__

```bash
nmcli c up test-client
```

__11. Test.__

```bash
curl zx2c4.com/ip
```

You should see the IP of your home Dream Machine Pro WAN. You can also now connect and disconnect from the Network section of your Ubuntu Settings.
