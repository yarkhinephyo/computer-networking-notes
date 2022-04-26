**Link layer**: Responsible for moving frames from one hop to the next.

**Node**: Any device that runs a link-layer protocol. (Hosts, routers, switches, access points) A transmitting node encapsulates a datagram in a link-layer frame and transmits onto the linkk.

**Services of link layer**: Varies from one protocol to the next.

<ins>Framing</ins>: Almost all link layer protocols encapsulate the network-layer datagram in a link-layer frame.

<ins>Link access</ins>: Medium Access Control (MAC) protocol specifies the rules by which a frame is transmitted onto the link. It coordinates the frame transmissions of many nodes that share a single broadcast link.

<ins>Reliable delivery</ins>: Some link layer protocols ensure that datagrams are moved without error. This is more relevant in links with high error rates such as wireless transfer compared to links such as fiber or copper links. The reliability in link-layer (point-to-point nodes) does not translate to reliability in the transport layer (end-to-end hosts).

<ins>Error detection</ins>: Receiver can detect errors and ask to retransmit or drop frames.

<ins>Error correction</ins>: Receiver can correct bit errors without retransmission.

**Link-layer implementation**: Network Interface Controller (NIC). The controller takes a datagram, encapsulates in a frame and sends over a communication link. The receiving controller extracts the datagram. (Optionally performs error detection)

**Half-duplex and full-duplex**: Half duplex means both can transmit but not at the same time.

**Point-to-point vs broadcast**: Two types of links. Single sender and single receiver VS multiple senders and receivers. Ethernet and wireless LANs are broadcast links.

**Multiple access problem**: Coordinate the access of multiple sending and receiving nodes to a shared broadcast channel.

When more than one node transmit frames at the same time, the frames collide at the receivers and no one can make any sense.

**Multiple access protocol**: Distributed algorithm that determines how nodes share a broadcast channel.

<ins>Time division multiple access (TDMA)</ins>: Each station gets a fixed length slot of time. Each transmits packet bits during the assigned time slot. Unused slots go idle. Time slots are typically chosen so that a single packet can be transmitted.

<ins>Frequency division multiple access (FDMA)</ins>: Each station gets a fixed frequency band with corresponding equal (divided) bandwidths. Unused frequency bands go idle.

<ins>Code division multiple access (CDMA)</ins>: Unique "code" for each user. Users share frequency but each user has own "chipping" sequence to encode data.

<ins>Simple CSMA (Carrier Sense Multiple Access)</ins>: A node only transmits if it senses that the channel is idle. It is a random access protocol which means a node transmit the full rate of the channel. Collisions can still occur.

**CSMA/CD**: A node only transmits if it senses that the channel is idle. If another node transmits at the same time, it stops transmitting and waits for a random amount of time (backoff time). The random amount of time is chosen from an integer interval which gets wider with more failures.

![](images/Pasted%20image%2020220409220230.png)

**Link-layer addressing**: MAC addresses are uniquely associated with each network adapter. A host or router with multiple NICs with have multiple MAC addresses. If there are multiple network interfaces, there are multiple link-layer addresses (And IP addresses). MAC addresses are used in the same subnet to get frame from one interface to another physical interface.

![](images/Pasted%20image%2020220409220711.png)

When an adapter wants to send frame to a destination adapter, it inserts the destination MAC address into the frame and sends it into the LAN. Each adapter checks if the destination MAC address matches its own.

**ARP**

ARP table stores mappings of IP address to MAC address, stored in each machine. The sending host checks the table to find out what the corresponding MAC address is. To send a datagram, the source must give its adapter both MAC and IP address of the destination.

A wants to send datagram to B. -> If no ARP entry, A sends an _ARP packet_ containing B's IP address and its own to the <ins>broadcast address</ins>. -> B replies with its MAC address. -> A updates ARP table (TTL).

ARP broadcast is not communicated across subnets by the router.

![](images/Pasted%20image%2020220222163153.png)

**Routing to another subnet**

A wants to send B which is in another subnet separated by router R. The router has an IP address and MAC address for each interface.

A creates IP datagram with IP destination B and link layer frame with R's MAC address as destination -> R extracts the datagram and determines the outgoing interface through forwarding tables. -> R creates link layer frame with B's MAC address as destination.

A cannot put the MAC address of B because then none of the adapters in Subnet 1 would pass the datagram up to network layer.

![](images/Pasted%20image%2020220222163707.png)

**Switch**: Link-layer device. Hosts are unaware of their presence. No need configuration.

```
if entry found for destination then
	if destination from which frame arrived then
		drop frame
	else
		forward frame on the indicated interface
else
	flood on all interfaces except the arriving interface
```

Switches are layer-2 packet switch. Routers are layer-3 packet switch. Switches have forwarding table, not ARP table (No concept of IP address).

<ins>Self-learning</ins>: Switch stores the MAC address from the incoming frame's source address field. There is no intervention from a network administrator or protocol. For example, if router A sends an ARP request through a switch, the switch will update its forwarding table to associate A with the interface.

**When to use switch vs routers**

Switches create networks and routers allow for connections between networks. Routers use IP addresses for data transmissions, switches do not.
