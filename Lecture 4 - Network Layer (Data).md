**Network layer**: Responsible for source-to-destination delivery of packets across multiple network links.

<ins>Best effort service</ins>: No guarantee on whether the data is delivered and the quality of the data when it is delivered.

**Forwarding (Data plane)**: Router-local action of transferring a packet from an input link interface to the appropriate output link interface.

<ins>Forwarding table</ins>: Router examines a packet header and its forwarding table to determine the outgoing link interface.

**Routing (Control plane)**: Network-wide process that determines the end-to-end paths that packets take from source to destination.

<ins>Traditional approach</ins>: A routing algorithm runs in each router and communicates with other routers to compute the forwarding table.

<ins>SDN approach</ins>: A remote controller computes and distributes the forwarding tables to every router. The controller may be implemented in a remote data center with high reliability.

**IPv4 datagram format**

Most datagrams do not contain "Options", so the header size is 20 bytes.

Version number specifies the IP protocol. Header length is used to determine where the payload begins. Datagram length is the total length (including header) in bytes. TTL is decremented each time a router processes the packet and dropped when zero is reached. Upper layer protocol is either TCP or UDP.

![](images/Pasted%20image%2020220201161147.png)

**Interface**: Boundary between the host and the physical link. A router has multiple interfaces. IP address is associated with an <ins>interface</ins> rather than the host or the router.

**Subnet**: Device interfaces that can reach each other without passing through a router. Devices in the same subnet have common high order bits.

**Classless interdomain routing (CIDR)**: Unique identifiers for networks and individual devices. An organization is typically assigned contiguous addresses with a common prefix.

**Private IP addresses**

![](images/Pasted%20image%2020220205163216.png)

**Dynamic host configuration protocol (DHCP)**

A host can be assigned a temporary address each time the host connects to the network.

Client-server protocol where the client is a new host that wants an IP address and each subnet has a DHCP server (Simple case). In home networks, the DHCP server is usually in the home router.

DHCP additionally stores the address of first hop router, IP address of DNS server, and the network mask.

**Four steps of DHCP**

1. Client sends DHCP discover message at port 67 to broadcast address 255.255.255.255 and source address as 0.0.0.0.
2. DHCP server offers a proposed IP address at the broadcast address.
3. The client respond with a DHCP request echoin back the same configurations.
4. DHCP server responds with DHCP ACK message.

![](images/Pasted%20image%2020220205164833.png)

**Network Address Translation**: NAT router behaves to the outside world as a single device with one IP address.

<ins>Decoupling</ins>: Can change host addresses in local network without notifying the outside world. Can change ISP without changing addresses of devices in the local network.

**NAT translation table**: Re-assigns the process in the local network with a new IP address and port number.

![](images/Pasted%20image%2020220205165707.png)