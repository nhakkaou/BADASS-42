---

# BADASS-42

## Bgp At Doors of Autonomous Systems is Simple

The purpose of this project is to expand on the knowledge you have gained through NetPractice. You will have to simulate a network and configure it using GNS3 with docker
images .

BGP EVPN is based on BGP (RFC 4271) and its extensions MP-BGP (RFC4760).
BGP is the routing protocol that drives the Internet. Through MP-BGP extensions, it
can be used to carry reachability information (NLRI) for various protocols (IPv4, IPv6,
L3 VPN and in this case, EVPN). EVPN is a special family for publishing information
about MAC addresses and the end devices that access them..

<img width="640" alt="Screen Shot 2024-04-22 at 12 40 32 AM" src="https://github.com/iltafah/BGP-GNS3/assets/56886719/b0e35d62-c022-43cd-8d46-a0e6348aaa72">


### HelpFul Notes, well at least for me :

#### To Ping between hosts:

```bash
# You will have just to create a bridge interface in every available routers just like this :
ip link add br0 type bridge
# or
brctl addbr br0
# Then set it to UP, default state is Down, run `ip a` to show its state
ip link set dev br0 up
# now connect all the router interfaces with the bridge interface, in order for the packets to traverse between them
brctl addif br0 eth0
brctl addif br0 eth1
brctl addif br0 eth2
# now you can ping from host `tofa7a-host-1` to `tofa7a-host-2` and vice-verca, do the same thing for the other router if you want to ping the other host
# don't forget to assign ip addresses for your host
```
<img width="1068" alt="Screen Shot 2024-04-21 at 3 50 31 AM" src="https://github.com/iltafah/BGP-GNS3/assets/56886719/6725c08f-9304-4e15-a0ef-a71dd0b61875">

####  ARP Packet Flow:

  ![ezgif com-animated-gif-maker](https://github.com/iltafah/BGP-GNS3/assets/56886719/b4ef070c-d37b-46cc-8b24-477ca2007874)


#### To Debug :

```bash
# in order to view the Address Resoloution Table Cash, which list the ip addresses with their own mac addresses
arp
# or
ip neigh show
# hence neigh stands for neighbor
```

```bash
# this will convert "hello world" to hexadecimal, in order to send it as a packet using ping
v=$(echo -n "hello world" | xxd -p)
```

```bash
# this will ping 30.1.1.3 and send the content of $v which is the hexadecimal of "hello world" just one time hence '-c 1' and only 11 bytes
ping -p $v -s 11 -c 1 30.1.1.3
```

```bash
# to see the requests that you received in destination sepecified earlier in the ping command, you can use within the destination host the following command:
packet_data=$(tcpdump -n -i eth1 -X -c 1)
# or just
tcpdump -n -i eth1
```

```bash
# If you want to add another host, just like the following image, you will have to add a new interface within the router
# then you will have to bridge the Vxlan interface with your new host interface
```

<img width="2560" alt="Screen Shot 2024-04-21 at 2 09 08 AM" src="https://github.com/iltafah/BGP-GNS3/assets/56886719/dbf2ec65-1d9a-4916-ad13-13214ab10b0b">

```bash
# If you want to play around and go further away with debugging, you can flush arp cash table
ip neigh flush 30.1.1.2 dev eth1
# you removed just one ip from the arp cash
# but what if You want to remove all the arp cash
ip neigh flush dev eth1
```

```bash
# Wanna see your beloved interfaces
ip addr show
# a specific one ?
ip addr show eth1
```

```bash
# How about changing Mac Address of a certain interface
ip link set dev eth1 address 00:13:37:42:fa:7a
```
