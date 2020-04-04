Title: Automatically route VPN traffic (split tunnelling)
Date: 2016-03-26 10:52:49
Category: How To
Tags: osx, linux, networking, vpn
Authors: Shane Maloney
Summary: Route traffic over VPN based on destination IP

When using VPNs to access remote systems I often need to access local network resources as well additional so remote systems do not have internet access. A pretty simple solution is to create a specific routes when the VPN is created. As the most specific route is used only traffic for this network will be forwarded over the VPN tunnel, the rest will use the default route. The `if-up` and `if-down` scripts located at `/etc/ppp/` are called with when a ppp conection is created or ended. The following arguments: `interface-name, tty-device, speed, local-IP-address, remote-IP-address, ipparam` are passed to these scripts so we can use these to add/remove specific rules for specify networks.

```bash 
#/etc/ppp/if-up

#!/bin/evn bash

IFNAME="${1}"
LOCALIP="${4}"
REMOTEIP="${5}"

if [[ "${REMOTEIP}" == "192.0.2.1" ]]; then
  /sbin/route add -host lab_computer.foo -interface "${IFNAME}"
fi
```

```
#/etc/ppp/if-down

#!/bin/env bash

IFNAME="${1}"
LOCALIP="${4}"
REMOTEIP="${5}"

if [[ "${REMOTEIP}" == "192.0.2.1" ]]; then
  /sbin/route delete -host lab_computer.foo -interface "${IFNAME}"
fi
```
