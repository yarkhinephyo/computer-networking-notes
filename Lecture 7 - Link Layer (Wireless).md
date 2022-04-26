**Wireless internet access technologies**

<ins>WLAN (Wireless local area network)</ins>: Locally placed radio transmitters (base stations) to provide internet within a few tens of meters. The base stations are connected to the internet via wires.

<ins>WWAN (Wide-area wireless access network)</ins>: Cellular telephone networks to provide internet access within a radius of a few kilometers.

**Wireless hosts**: Hosts but wireless.

**Wireless links**: Connects wireless hosts at the network edge into the larger network infrastructure.

Signal strength decreases more significantly over a distance compared to wired links. There are more interferences from other sources. If two devices transmit to a third device, their transmissions will interfere. When waves reflects of objects ground, they arrive at destination at different times.

**Base station**: Responsibile for sending and receiving data to and from a wireless host that is associated with the base station. Also known as Access Point (AP).

Has SSID (Service Set Identifier). AP periodically send <ins>beacon frames</ins> which includes SSID and MAC address.

A device selects the AP with highest signal strength and associates with it. Once associated, the device will want to join the subnet of AP. DHCP may be required to obtain an IP address. The wireless home routers come with DHCP.

**802.11 Wireless LAN**: Basic Service Set (BSS) is a fundamental building block. It consists of one or more wireless host and one base station (AP). The AP connects to an interconnection device such as switch or a router which leads to the internet.

**802.11 frame**

![](images/Pasted%20image%2020220308164502.png)

Typically, three addresses are in each frame when the network-layer datagram is moved from a wireless host to a router interface through AP. Address 1 is the MAC address of the wireless station receiving the frame (Wireless host or AP). Address 2 is the MAC address of the station transmitting the frame (Wireless host or AP). Address 3 is the MAC address of the router interface that can connect to other subnets.

This is <ins>different</ins> from 802.3 frame with only destination and source addresses.

The router is not aware of the AP's presence. To forward datagram to a wireless host, the router encapsulates a datagram with an ethernet frame, indicating the MAC address of the wireless host as the destination MAC address. AP converts the 802.3 frame into 802.11 frame before transmitting it to the wireless channel.

**802.11 MAC protocol**: Carrier Sense Multiple Access with Collision Avoidance (CSMA/CA). This is different from ethernet's CSMA/CD. Collision detection cannot be effectively done on wireless channels.

If a station senses the channel idle, waits for DIFS (Distributed inter-frame space) before transmitting frame. If not, random backoff with binary exponential backoff is done. Then the station transmits the <ins>entire frame</ins> and sets a timer. If an acknowledgement is received within the time, it marks a successful transmission. If not, it carries out the random exponential backoff.

For collision avoidance, the host sends a Request to Send (RTS) frame to AP with the total time required for transmission. Clear to send (CTS) frame is broadcasted by the AP to ensure other stations do not send during reserved duration. It also acts as an explicit permission for the would-be sender. <ins>However</ins>, this sequence is skipped for frames with less than a threshold size.

![](images/Pasted%20image%2020220409220137.png)

**Mobility is same subnet**: If a host moves from AP1 to AP2 which are in the same subnet, the host associates with AP2 which keeping its IP address and maintaing the ongoing TCP sessions.

**4G LTE cellular networks**

**Cell**: Partition of a cellular network. There is one base station that transmits signals to and receives signals from the mobile devices inside.

**Mobile devices**: IP addresse is obtained through NAT. Possesses <ins>International Mobile Subscriber Identity</ins> (IMSI) which is a globally unique identifier stored on the SIM card.

**Base station**: Serves as a central connection point for wireless devices to communicate within its coverage area. Handles authentication and channel access.

**Home subscriber server (HSS)**: Database with information about the mobile devices for which the network is their home network. (Example: StarHub, Singtel)

**Packet data network gateway (P-GW)**: Provides NAT IP addresses to mobile devices. To the outside world, P-GW looks like any other router.

**Mobility management entity: (MME)**: Tracks a mobile device's location and works with HSS to perform mutual authentication.

![](images/Pasted%20image%2020220409212744.png)

**Device mobility**: There are different levels.

<ins>Handover</ins>: The process of transferring an ongoing call or session from one channel connected to the core network to another channel.

![](images/Pasted%20image%2020220308163215.png)

**Roaming**: If a mobile device is connected to a cellular network where the HSS does not recognize it as a subscriber, it is said to be roaming on a visited network. The home network provides a single location where the information about the device can be found.

**Direct routing to/from mobile**: When a mobile device is roaming, the visited network provides an IP address to the mobile device and informs the home network. When a host (correspondent) wants to communicate with the device. Correspondent contacts home HSS, gets the device's visited network. Correspondent addresses datagram to visited network address. Visited gateway router forwards the data to the mobile device. The HSS must <ins>proactively</ins> update the correspondent every time the mobile device moves.