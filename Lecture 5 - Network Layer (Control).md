**Good routing path**: Path with the least cost. In reality, other policy issues come into play.

**Network as a graph**: Nodes represent routers in the context of network-layer routing. Nodes represent networks in the context of BGP inter-domain routing protocol. Edge cost may represent speed or monetary cost associated with a link.

**Link-state routing (Dijkstra)**

Network topology and link costs are presumed to be known by every node. All nodes have an identical view of the network. Each node broadcast identities and costs of attached links through _link-state packets_.

```
N’ = {u}
for v in all nodes:
 if v is a neighbor of u:
  D(v) = c(u,v)
 else:
  D(v) = ∞

Loop:
 find w not in N’ such that D(w) is a minimum
 add w to N’
 for each neighbor v of w and not in N’:
  update D(v) = min( D(v), D(w)+ c(w,v) )

until N’= N
```

**Distance vector routing (Bellman Ford)**

Each node only knows the cost-paths to reach the directly connected neighbors. Each node broadcasts _distance vector_ (least-cost estimates to all nodes) to its neighbors.

<ins>Iterative</ins>: Process continues until no more exchange of information between neighbors.

<ins>Asynchronous</ins>: The nodes do not operate in lockstep with one another.

<ins>Distributed</ins>: Each node receives information from neighbors and distributes the calculated results.

![](images/Pasted%20image%2020220412111056.png)

```
loop:
 wait for a distance vector [Dv(y): y in N]
 for each y in N:
  Dx(y) = minv{c(x,v) + Dv(y)} # minimum of all neighbors
 if Dx(y) changed for any destination y:
  send distance vector Dx = [Dx(y): y in N] to all neighbors
forever
```

**Intra-AS (autonomous system) routing: OSPF**

Flat networks with simple routers only would not work at large scale. Each organization would also like to control its own network of routers.

Routers can be organized into autonomous systems (ASs). Routers within the same AS run the same routing algorithm.

Gateway routers at the edge of the ASs have links to routers in other ASs.

Open Shortest Path First (OSPF) is a link-state protocol that uses flooding of link-state information and Dijkstra's least-cost algorithm. The flooding only occurs in backbone or the router's own "area". Each router runs the algorithm with itself as the root node.

![](images/Pasted%20image%2020220208162323.png)

Individual link costs are configured by network administrator. Equal link costs mean minimum-hop routing. They can also be configured inversely proportional to link capacity.

**Routing among ISPs: BGP**

All ASs run the same routing protocol named Border Gateway Protocol. Allows subnet to advertise its existence to the rest of internet. Local AS policies will determine whether to advertise the paths to other ASs or choose to never route through certain ASs.

eBGP obtains subnet reachability information from neighboring ASs. iBGP propagates reachability information to all AS-internal routers. OSPF advertises the link-state information within the AS and works alongside iBGP.

AS2 router 2c receives path advertisement {AS3, X} via eBGP -> AS2 router 2c propagates the path advertisement to all internal routers via iBGP -> AS2 router 2a advertises path {AS2, AS3, X} to AS1 router 1c.

![](images/Pasted%20image%2020220206162221.png)

<ins>ISPs</ins>: When a company contracts with a local ISP and gets assigned a prefix (address range), the ISP uses BGP to advertise the prefix. This is how the datagrams will be able to find the company's servers.

**BGP vs OSPF**

BGP | OSPF
------ | -------
Mainly used for inter-AS | Intra-AS
iBGP + eBGP | Used with iBGP
AS as a single unit | Router as a single unit
Pick links with lowest AS cost | Pick links with lowest router cost
Only exchange information to peer | Information is flooded to all routers
Does not support ECMP | Supports ECMP

**Routing policy**: ISP only wants to route traffic to/from its customer networks.

In the diagram, A advertises {A, w} to B and C. B does not advertise {B, A, w} to C, so C does not learn about {C, B, A, w} path. C will route {C, A, w} from y to w.

![](images/Pasted%20image%2020220208164745.png)
