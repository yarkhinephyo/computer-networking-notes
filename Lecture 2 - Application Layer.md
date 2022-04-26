**Application layer**: Responsible for communication between applications running on two different end systems.

**Network applications**: Apps that need to communicate over the internet and runs on separate devices. These run at the network edge only (Not at network core).

The process that initiates the communication is the client. The process that waits to be contacted is the server.

**Client-server architecture**: Servers are hosted by a company. Clients do not directly communicate with one another. Requires high availability for the servers.

**Peer-to-peer architecture**: Everyone can be a client or a server. (E.g: BitTorrent) Increase in users are offset by increase in peers which means self-scalability.

**Socket interface**: Interface between the application layer and the transport layer within the host. The application developer has control of everything on the application-layer side but has little control of the transport-layer side of the socket.

**Application layer protocol**

1. Type of message such as request and response.
2. Syntax of message such as fields and how they are delineated.
3. Semantics of the fields such as the meanings in the fields.
4. Rules for determining how a process sends and receives messages.

**HTTP**

Client initiates TCP connection to server (Creates socket). Server accepts TCP connection from client. Messages are exchanged. HTTP does not need to worry about missing or unordered data.

```
# No persistent connection required.

GET /somedir/page.html HTTP/1.1\r\n
Host: www.someschool.edu\r\n
Connection: close\r\n
User-agent: Mozilla/5.0\r\n
Accept-language: fr\r\n
\r\n
```

```
# Connection has been closed. Content-length is 6821 bytes.

HTTP/1.1 200 OK\r\n
Connection: close\r\n
Date: Tue, 18 Aug 2015 15:44:04 GMT\r\n
Server: Apache/2.2.3 (CentOS)\r\n
Last-Modified: Tue, 18 Aug 2015 15:11:03 GMT\r\n
Content-Length: 6821\r\n
Content-Type: text/html\r\n
(data data data data data ...)\r\n
```

**Cookie**: Applications can choose to store data on the client browser to maintain some states between transactions.

**DNS**

Port 53 - UDP. DNS servers communicate in the application layer to resolve names. DNS servers cache mappings at every level of the hierarchy and local DNS servers.

```
# Three classes of DNS servers
-> Root DNS servers
-> TLD DNS servers (.com .edu)
-> Authoritative DNS (facebook.com amazon.com)
```

Queries made by the requesting host are sent to Local DNS servers (Also known as DNS Resolvers) which are not in the hierarchy. They are managed by ISPs such as cable internet provider or a corporate network.

Most home routers also have local DNS cache. Only if there is no record, the query is forwarded to IPS's more capable servers.

**DNS resolution**

In practice, DNS lookup process is iterative as a recursive query results in heavy load at the upper levels of the hierarchy.

```
# Iterative
1. Ask local DNS server
2. (If uncached) Local DNS server asks the root DNS server which provides TLD DNS server
3. TLD DNS server provides the authoritative DNS server
4. The authoritative DNS server provides the IP address

# Recursive
1. Ask local DNS server
2. (If uncached) Local name server asks the root DNS server
3. The root DNS server asks TLD DNS server
4. TLD DNS server asks the authoritative DNS server
5. The authoritative DNS server provides the IP address
```

**DNS caching**: The local DNS server caches information of any reply received from DNS servers. If TLD DNS servers are cached, root servers can be bypassed.

**Video streaming**: Bit rate is the most important performance measure for streaming video. Network must provide throughput >= bit rate of video. (e.g: 100kbps for low-quality, 10Mbps for 4K)

**Dynamic adaptive streaming over HTTP (DASH)**: Video is encoded into several quality versions. Client dynamically requests chunks of videos based on the bandwidth available.

**Content distribution network**

Put the exact content into multiple servers. Authoritative DNS server (CDN) does a geographic lookup based on the DNS resolver's IP address and return an IP address of an edge server closest to the area.

![](images/Pasted%20image%2020220118180651.png)

**Enter deep**: Deploy the servers in ISPs all over the world so that users can access with lower delay and higher throughput. Decreases the number of links and routers between the end users and the content.

**Bring home**:. Build large clusters but at a smaller number of physical sites. There is lower maintenance required but possibly at the expense of higher delay and throughput.