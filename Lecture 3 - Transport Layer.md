**Transport layer**: Logical communication between application processes (sockets) between different hosts over a network.

Breaks application messages into segments at the sender and reassembles them back at the receiver. Routers do not implement this layer.

**Multiplexing**: Data from different sockets are gathered, encapsulated into segments and passed to the network layer.

**Demultiplexing**: Transport layer looks at the header information and directs the segment to the correct socket.

**Socket**: Identified by tuple `(local-IP, local-port, remote-IP, remote-port, protocol)`. Different sockets for multiple established connections.

**UDP**

Connectionless, unordered and unreliable. Headers (8-byte) are source port, destination port, length and checksum.

![](images/Pasted%20image%2020220410172400.png)

**Advantages of UDP over TCP**

1. Finer application-level control over the data. Application data is packaged into a segment and immediately sent to the network layer. This is unlike TCP protocol which grabs arbitrary bytes and does not send the data immediately.
2. No connection establishment means lower delays.
3. No connection state means less memory needed.
4. Smaller header overhead (8 bytes < 20 bytes).

However, UDP traffic is usually blocked by firewalls because it can be unsolicited traffic. So applications that will benefit from the protocol may use TCP as a backup.

**Maximum transmission unit (MTU)**: Maximum length of a link-layer frame. Ethernet has MTU of 1500 bytes.

**Maximum segment size (MSS)**: Maximum amount of application-layer data in the segment. Typically 1460 bytes as IP + TCP headers add up to 40 bytes.

**TCP**

Point-to-point, reliable, in-order byte stream.

Sender and receiver must "handshake" to establish connection and prepare for data transfer. Both sides will initialize some state variables associated with the connection.

After the client process passes the data through the socket, it is sent to a "send buffer" managed by TCP. From time to time, TCP grabs chunks of data from the buffer and send to the network layer.

At the receiver side, the segments' data are placed in the TCP connection's receive buffer. The application reads the stream of data from the buffer.

**Byte-stream of TCP over UDP's message boundaries**

<ins>Advantage</ins>: Application that reads/writes byte streams (ssh, telnet) have no notion of message boundaries.

<ins>Disadvantage</ins>: Application protocols need to create mechanisms for identifying message boundaries.

**TCP segment structure**: 20-byte headers.

![](images/Pasted%20image%2020220122164024.png)

<ins>ACK field</ins>: Indicates if the acknowledgement field is valid. If acknowledgment number is 550, it means "send me from byte 550 onwards".

<ins>Sequence number</ins>: Counter tracking every byte sent outwards by the sender.

**Reliable data transfer**: TCP's reliable data transfer on top of IP's unreliable service.

Receiver maintains a buffer of bytes that have been received. The sender tracks which bytes are already sent. ACK numbers indicate what byte a receiver expects. SEQ numbers indicate what byte the sender is sending. If the sender does not receive ACK in time, it re-sends the oldest unacknowledged segment.

In the example, sender has sent until byte 11, but the receiver has only acknowledged until byte 8. The sender can send until byte 16 if it has more data from the application layer as the TCP window size is not reached yet. The window moves forward as more segments are acknowledged.

![](images/Pasted%20image%2020220125161921.png)

**TCP window size**: Maximum data that TCP can send at one time - `min(cwnd, rwnd)`.

<ins>Congestion window</ins>: How many unacknowledged bytes can be sent to the receiver without causing congestion in the network.

<ins>Receive window</ins>: How many unknowledged bytes can be sent to the receiver without overwhelming the receiver.

**Timer**: Set for the oldest unacknowledged segment. If timeout, retransmit the <ins>oldest</ins> unacknowledged segment and restart timer.

**SendBase**: TCP state variable which is the oldest unacknowledged sequence number. If the ACK value from the other party is more than this variable, it means one or more previously unknowledged segments are being acknowledged. Timer can be restarted if there currently are more unacknowledged segments.

**Scenario 1: Segment 100 not retransmitted**

![](images/Pasted%20image%2020220125171542.png)

**Scenario 2: A cumulative ACK**

![](images/Pasted%20image%2020220125171613.png)

**TCP Flow control**: Receiver controls the sender so that the receive buffer does not overflow. Flow control matches the rate at which the data is sent to the rate that it is read. The "receive window" TCP header communicates spare buffer left. This allows the sender to self-limit the amount of unacknowledged data.

```
rwnd = RcvBuffer - (LastByteRcvd – LastByteRead)
```

If `rwnd` is zero, the sender will send segments will one byte until the acknowledgement segments contain a nonzero `rwnd` value.

**TCP three-way handshake**: SYN -> ACK/SYN -> ACK. Sequence numbers from each host are randomly chosen. The last ACK segment may contain client-to-server data in the payload.

**TCP close connection**: FIN -> ACK ... FIN -> ACK. Two pairs of the same set of messages. This gives the responding application time to prepare for closing. Less graceful method is sending a single RST, occurs during error.

**TCP congestion control**: Traffic is controlled entering into the network. This is different from flow control which is between the sender and the receiver. `cwnd` is measured in terms of <ins>MSS</ins> instead of bytes. 

A TCP sender perceives congestion in the network through loss events. There are two types of loss events - 3 duplicate ACKs or a timeout (Worse). These are caused when datagrams are dropped in the routers during buffer overflows.

If transmissions succeed, the congestion window ramps up first exponentially than linearly. TCP strategy for adjusting its transmission rate is to increase its sending rate until a loss event occurs. There are three states - 1) slow start 2) congestion avoidance 3) fast recovery.

![](images/Pasted%20image%2020220125180832.png)

**Handling loss events**

<ins>Timeout</ins>: No matter the state, the TCP sender sets `ssthresh` to `cwnd/2` and restarts the slow start process at 1 MSS. 

**1. Slow start**

The `cwnd` is typically initialized to 1 MSS, with the rate of MSS/RTT. Since every acknowledgement increases `cwnd` by 1 MSS, the `cwnd` effectively doubles every RTT. When `cwnd` is equal to `ssthresh`, TCP transition to congestion avoidance mode.

<ins>Triple duplicate ACK</ins>: TCP performs a retransmit, sets `cwnd` to `ssthresh + 3MSS` and enters the fast recovery state.

**2. Congestion avoidance**

TCP increases `cwnd` by a single MSS every RTT. This means every acknowledgement increases `cwnd` by `MSS/cwnd`.

<ins>Triple duplicate ACK</ins>: TCP performs a retransmit, sets `cwnd` to `ssthresh + 3MSS` and enters the fast recovery state.

**State transition for TCP congestion control**

**3. Fast recovery**

The `cwnd` increases by 1 MSS for every duplicate ACK (the particular missing segment). When the ACK finally arrives, TCP enters the congestion avoidance state after deflating `cwnd` to `ssthresh`.

![](images/Pasted%20image%2020220410225659.png)

![](images/Pasted%20image%2020220410233539.png)

_Diagram on Fast Recovery Phase_