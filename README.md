# dhcp plugin

NOTE: The code is from [Official CNI DHCP Plugin](https://github.com/containernetworking/plugins/tree/master/plugins/ipam/dhcp).  It's modified for passing the container's name as hostname in DHCP request, enabling dns resolver binding the hostname to the container.

## Overview

With dhcp plugin the containers can get an IP allocated by a DHCP server already running on your network.
This can be especially useful with plugin types such as [macvlan](../../main/macvlan/README.md).
Because a DHCP lease must be periodically renewed for the duration of container lifetime, a separate daemon is required to be running.
The same plugin binary can also be run in the daemon mode.

## Operation
To use the dhcp IPAM plugin, first launch the dhcp daemon:

```
# Make sure the unix socket has been removed
$ rm -f /run/cni/dhcp.sock
$ ./dhcp daemon
```

If given `-pidfile <path>` arguments after 'daemon', the dhcp plugin will write
its PID to the given file.
If given `-hostprefix <prefix>` arguments after 'daemon', the dhcp plugin will use this prefix for DHCP socket as `<prefix>/run/cni/dhcp.sock`. You can use this prefix for references to the host filesystem, e.g. to access netns and the unix socket.

Alternatively, you can use systemd socket activation protocol.
Be sure that the .socket file uses /run/cni/dhcp.sock as the socket path.

With the daemon running, containers using the dhcp plugin can be launched.

## Example configuration

```
{
	"ipam": {
		"type": "dhcp",
	}
}

## Network configuration reference

* `type` (string, required): "dhcp"
