**Network edge**: Also called hosts. End systems that run applications.

**Access network**: Also known as the edge router. Physically connects a host to the first router.

**Internet**: Interconnected Internet Service Providers (ISP).

**Physical media**: Connects transmitter and receiver. In guided media, signals propagate through solids. In unguided media, signals propagate through air or water.

**Network core**: Interconnected routers.

**Packet switching**: Hosts break messages into smaller bits (packets). Packets do not have to take the same path. 

Host breaks down into packets of length L (bits). Transmits packet into access network at transmission rate R (bits/sec).

```
Packet transmission delay
= Time needed to transmit L-bit packet into link
= L (bits) / R (bits/s)
```

**Store and forward transmission**: Entire packet must be received by the router before it transmits onto the outbound link.

**Message switching**: Sending the entire message as a packet will cause a higher delay than packet switching. Sending and receiving of a packet cannot be done in parallel.

**Packet-switched networks**: As a packet travels from one node to another, there are several types of delay along its path.

![](images/Pasted%20image%2020220409222619.png)

**Types of delay components**

Given a fixed route, only the queuing delay is variable, the rest are constant.

<ins>Processing delay</ins>: Time that a node spends examining a packet's header to direct the packet. This can include the time it takes to check for errors in the packet. (Order of microseconds)

<ins>Queuing delay</ins>: Time for a packet to wait for its turn to be transmitted on to the link. (Order of microseconds to milliseconds)

<ins>Transmission delay</ins>: Time required to push out all the packet's bits onto the link. (Order of microseconds to milliseconds)

<ins>Propagation delay</ins>: Time for a packet to reach destination. Distance between the two routers divided by the propagation speed. It is not affected by the packet length or transmission rate.

**Nodal delay**: Combination of the four delays.

**Packet loss at queues**: Each packet switch has multiple links attached. For each link, there is an output buffer which stores packets that the router is about to transmit. If emission rate is higher than transmission rate, a queue overflow occurs and packets are dropped (Based on an algorithm).

**Throughput**: Rate at which data flows. Similar to how water taps can have 1L/s or 2L/s even when water flows the same rate (Propagation speed).

**Bottleneck link**: Link that constrains the end-to-end throughput. Usually bottleneck links are at the network edges instead of the network core.

**Layered architecture**

Layer | Examples
--- | ---
Application | HTTP, DNS
Transport | TCP, UDP
Network | Internet protocol, Routing protocol
Link | Ethernet, Wifi
Physical | Bits on "wire"

**Encapsulation**

Data goes down a sending host's protocol stack, up and down the protocol stacks of intermediate routers, then up the protocol stack at the receiving host. Hosts implement all 5 layers. Routers typically implement only layers 1-3.

![](images/Pasted%20image%2020220111181930.png)

![](images/Pasted%20image%2020220122133203.png)

_Data at Each Layer_

**Kb != KB**
1 Kb = 1 kilobit = 103 bits
1 KB = 1 kilobyte = 103 bytes