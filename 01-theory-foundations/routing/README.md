# Routing Basics — Scratch to Deep

---

## Why Routing Exists?

Imagine this:

You’re sending a parcel from Mumbai → New York.  
Postal system doesn’t know your friend personally — it relies on **sorting centers, trucks, airplanes**.  

Each sorting center checks the destination → decides which path the parcel takes.

In networking:

- **Data packets** = parcels  
- **Routers** = sorting centers  
- **Destination IP** = address on parcel  
- **Routes / routing tables** = directions  

Without routing: packets get lost, browsers can’t reach Google or cloud servers.

---

## Scratch Definition

- **Routing** = process of determining the path a packet takes from source to destination across one or more networks.  
- **Router** = device that forwards packets based on IP + routing table.  
- Uses **destination IP + routing rules** → chooses **next hop**.  

---

## Why Routing is Important

- Connect networks → LAN to LAN, LAN to Internet, cloud networks  
- Efficient delivery → Best path chosen  
- Redundancy & failover → Traffic reroutes if one path fails  
- Cloud networking → VPCs, subnets, VPNs, hybrid clouds rely on routing  

---

## Routing Functions — Deeper Look

### Path Selection
Chooses best route based on:
- Hop count  
- Bandwidth  
- Latency  
- Cost (cloud hybrid networks)

### Traffic Segmentation
- Determines which subnet/VPC packet belongs to  
- Works with VPCs, NAT, VPNs

### Redundancy & Failover
- Dynamic routing detects failures → reroutes automatically  

### Integration with Security & NAT
- Ensures packets pass through **firewalls / NAT devices** as needed  

---


## Static vs Dynamic Routing — Deep Context

### Static Routing
- Manual config  
- Predictable, secure, low CPU  
- Not scalable  
- Good for small LANs / fixed cloud VPCs  

### Dynamic Routing
- Routers exchange info using **RIP, OSPF, BGP**  
- Auto adapts to changes  
- More complex  
- Needed for **enterprise / hybrid cloud**  

---

## Routing Process — Step by Step

---

### Step 0: Packet Creation (Source Device)
- Device generates a packet:
  - **Source IP** = your device  
  - **Destination IP** = target device/server  
  - **Transport Layer info** = TCP/UDP ports  

- Device checks if destination is in same subnet:
  - If yes → sends directly  
  - If no → sends to **default gateway (router)**  

---

### Step 1: Router Receives Packet
- Packet arrives at router’s interface.  
- Router inspects **destination IP** in header.  
- Router consults **routing table** to decide where to forward.  

---

### Step 2: Routing Table Lookup
- Router searches for **longest prefix match** (most specific route).  

**Example Table:**
```
10.0.0.0/8      → eth0
10.0.1.0/24     → eth1 (longest prefix match)
0.0.0.0/0       → eth2
```

- If no match → use **default route (0.0.0.0/0)**  
- Router determines:
  - Next Hop IP  
  - Outgoing Interface  

---

### Step 3: Determine Path (Static vs Dynamic)
- **Static Routing** → Forward via manually configured route  
- **Dynamic Routing** → Path chosen using protocol metrics:  
  - RIP → hop count  
  - OSPF → bandwidth / cost  
  - BGP → AS path / policy  

---

### Step 4: Forward Packet
- Packet sent via chosen interface → toward next hop.  
- **TTL (Time To Live)** decreases → prevents infinite loops.  

---

### Step 5: Repeat at Each Router
- Every intermediate router repeats:  
  1. Receive packet  
  2. Lookup destination in routing table  
  3. Determine next hop  
  4. Forward packet  

- **Dynamic routing** ensures updates if topology changes.  

---

### Step 6: Arrival at Destination Subnet
- Packet reaches final router in **destination subnet**.  
- Router forwards packet to target host.  
- **Switch** in subnet may forward by **MAC address (Layer 2)**.  

---

### Step 7: Transport Layer Handling
- **TCP** → Acknowledgment (ACK) confirms delivery.  
- **UDP** → No acknowledgment → fire-and-forget.  

---

### Step 8: Cloud & Hybrid Networking Integration
- **Cloud Networks**:  
  - Route tables define traffic paths.  
  - Public Subnet → Internet Gateway (IGW)  
  - Private Subnet → NAT Gateway (outbound)  
  - VPN → secure routing to on-prem  

- **Hybrid Cloud**:  
  - BGP / OSPF advertise routes between **cloud + on-prem**  
  - Automatic failover if primary link fails  

---

### Step 9: Routing Protocol Updates
- Routers exchange info:  
  - **OSPF** → Link state updates  
  - **BGP** → AS paths  
  - **RIP** → Distance-vector updates  

Ensures **future packets** follow the **best available path**.  

---


## Routing Table — Deep Anatomy

Every router maintains a **routing table**:

| Field        | Meaning                               |
|--------------|---------------------------------------|
| Destination  | Network or host IP to reach           |
| Netmask      | Defines network size                  |
| Gateway      | Next hop router IP                    |
| Metric/Cost  | Preference (lower = better)           |
| Interface    | Outgoing port                         |

**Example:**
```
Destination     Netmask         Gateway         Interface   Metric
10.0.1.0        255.255.255.0   0.0.0.0         eth0        1
10.0.2.0        255.255.255.0   10.0.1.1        eth1        2
0.0.0.0         0.0.0.0         192.168.1.1     eth2        10
```

- **Default route (0.0.0.0/0)** → used for unknown destinations  
- **Metric** → lower = preferred  

---

## Routing Protocols 

| Protocol | Type             | Key Use Case          | Extra Depth |
|----------|------------------|-----------------------|-------------|
| **RIP**  | Distance Vector  | Small networks        | Hop-count only, slow convergence |
| **OSPF** | Link State       | LAN + Data Centers    | Updates triggered by topology changes, faster convergence |
| **BGP**  | Path Vector      | Internet + Hybrid Cloud | Policies for multi-homed networks, can influence cost-based routing |
| **EIGRP**| Hybrid (Cisco)   | Cisco LAN/WAN         | Combines distance + metrics for optimized path |

---

## Cloud Scenario

- **Hybrid Cloud** → AWS VPC + On-Premises Data Center  
- **BGP** dynamically advertises **on-prem routes** to **AWS Virtual Private Gateway**  
- Provides **automatic failover** if the primary connection fails

---

## Routing in Cloud Networks — Real Depth

Cloud routing depends on:

- **VPCs** = isolated cloud networks  
- **Subnets** = public/private divisions  
- **Route Tables** = traffic paths  
- **VPN/Direct Connect** = hybrid connectivity  
- **NAT Gateway** = private → internet  

**Flow Example:**
- User → public web server → IGW route  
- Web server → external API → private subnet uses NAT Gateway  
- Employee → VPN → traffic routed to private subnet  

---

## Practical Commands / Setup

**Linux**
```bash
# Show routing table
route -n

# Modern command
ip route show
```

---

## Routing Limitations / Considerations

- Static → manual updates, no failover  
- Dynamic → complex, possible loops  
- Cloud routes → misconfig blocks internet  
- Redundant paths → need OSPF/BGP for loop prevention  

---

## Summary Mental Model

- **Routing = Postal System / GPS** for packets  
- **Router = Sorting Center**  
- **Routing Table = Directions / Map**  
- **Next hop = Next facility**  
- **Static vs Dynamic vs Default** = strategies for packet delivery  

Cloud: Route tables in VPCs = decide how traffic flows inside/outside.  

Think of routing as the **nervous system** of the internet:  
- Ensures packets find right path  
- Works with **NAT, VPN, firewall, DNS**  
- Adapts automatically in complex networks  
- Critical for **hybrid + cloud deployments**  





































