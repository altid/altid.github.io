# Using listen_address

A usual ubqt network exposes all services to all clients, at one IP address. This behavior can be changed, assigning an IP address per-service, or aggregating similar services to a single IP as a means of seperation of concerns. All your chat services could be accessed via an IP, that you set a router-level DNS entry for as `chat`; dialing that service with a ubqt client within your network is as simple as `myubqt-client chat`!
In the same spirit, you can aggregate services to your client (if supported): `myubqt-client docs web code` and be ready to go on a coding adventure.

## Network Stack

To allow this to happen, you first have to add IP addresses to your network stack. 

 * Run these commands for each entry, before starting your ubqt server(s)
 * Using a listen_address which already has a DHCP reservation will not work. Consider setting static IP reservations for your listen addresses.

### Linux/*BSD

#### With `ifconfig`

> This information is adapted from https://www.garron.me/en/linux/add-secondary-ip-linux.html, all credit goes to the author.

For each listen_address you wish to use, you issue `ifconfig [nic]:0 [IP-Address] netmask [mask] up`. For example, to add 192.168.0.2 to the network stack of eth0, you would type:
`ifconfig eth0:0 192.168.0.2 netmask 255.255.255.0 up`

#### With `ip`

For each listen_address you wish to use, you issue `ip address add [ip]/[mask-digits] dev [nic]`. For example, to add 192.168.0.2 to the network stack of eth0, you would type:
`ip address add 192.168.0.2/24 dev eth0`

### Plan9

> This information is adapted from http://doc.9gridchan.org/blog/180111.ip.stack.addresses

For each listen_address you wish to use, you issue `ip/ipconfig -g [gateway] ether [nic] add [IP-Address] [mask]`. For example, to add 192.160.2 to the network stack of ether0 when your router gateway ip is 192.168.0.1, you would type:
`ip/ipconfig -g 192.168.0.1 ether /net/ether0 add 192.168.0.2 255.255.255.0`

## Configuration

For each service, simply adding a line `listen_address=192.168.0.2` would suffice to set the listen address. For example:

```
service=irc server=irc.freenode.net port=6697
	[...]
	listen_address=192.168.0.2

service=docs
	[...]
	listen_address=192.168.0.3:4509
```

### Port Setting

In the above example, the docs service (docfs) uses an optional listen port, 4509. When unset, the default port will depend on which ubqt server you're connecting to. For example, 9p-server by default uses `:564`. In the above example, the actual address + port pair used would be `192.168.0.2:564`

## Wrapping Up

To ease starting up a ubqt network, it's strongly suggested to create a startup script.

For example on Linux/*BSD:

```
#!/bin/sh 
ip address add 192.168.1.141/24 dev eth0
ircfs -s freenode &
discordfs &
smsfs &

ip address add 192.168.1.142/24 dev eth0
docfs &
undofs &

9p-server &
ssh-server &
```

Simply chmod +x <scriptname> and set it somewhere in your path.
