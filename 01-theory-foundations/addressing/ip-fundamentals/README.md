# Introduction to IP Addressing

---

### Imagine you just opened your laptop and typed google.com
In less than a second, a page loads with billions of links.  

But pause - how did your laptop know where Google lives?  
It doesn’t know Google personally.  
It only knows how to shout into the digital void: **“Who has this address?”**  

The magic that makes that shout land on the exact right server, across oceans of routers, is **IP addressing**.  

**IP address (Internet Protocol address)** → It’s the language of the Internet.  
No IP, no communication.  

---

## Why IP is the Backbone of the Internet
Imagine **3 PCs connected to a switch**. MAC addresses are enough for them to chat.  
But what if one PC wants to talk to **Google’s server, sitting in another country?**  

You need a globally unique, hierarchical addressing system → **that’s IP.**  

- **MAC = flat system** (random 48-bit numbers).  
- **IP = hierarchical system** (organized by network → host).  

IP introduces hierarchy:  
- **Network part** → where you live  
- **Host part** → who you are inside that network  

This hierarchy allows routers to forward data across continents **without getting lost**.  

**Analogy:**  
- MAC = person’s name ("John") → many Johns exist  
- IP = full mailing address (Country, State, City, Street, House #) → only one John at that address  

Without IP, the Internet would collapse.  

---

## MAC vs IP – Why Two Identities?

Here’s the trick: one identity isn’t enough.  

### MAC Address
- Burned into your network card (hard to change).  
- Works like your **fingerprint** → unique but not location-aware.  
- Lives at **Layer 2 (Data Link)**.  

### IP Address
- Assigned logically by software (can change anytime).  
- Works like your **home address** → tells the world where to deliver.  
- Lives at **Layer 3 (Network/Internet)**.  

**Analogy:**  
Imagine you’re sending me a letter:  
- Your **name** = MAC (always you)  
- Your **address** = IP (where you are right now)  

That’s why we need both.  

---

## The Two Generations of IP
- **IPv4 (1980s):** 32-bit, ~4.3 billion addresses. We squeezed it dry.  
- **IPv6 (1990s):** 128-bit, ~340 undecillion addresses. Future-proof, built for the cloud.  

Today both run together (dual stack).  
Cloud providers (AWS, Azure, GCP) push IPv6 hard because **scale is everything**.  

---

## Flavors of IP Addresses
- **Private:** Stay inside your LAN/VPC → Example: `192.168.1.10`  
- **Public:** Reachable from the Internet → Example: `8.8.8.8` (Google DNS)  
- **Loopback:** Test yourself → `127.0.0.1`  
- **Link-local:** Emergency fallback if DHCP fails → `169.254.x.x`  

---

## Flow Example
IP sits in **OSI Layer 3 (Network Layer)** and **TCP/IP Internet Layer**.  

It encapsulates **Transport Layer segments (TCP/UDP)** into packets.  
Then hands them to **Layer 2 (Ethernet, Wi-Fi)** → which uses **MAC addresses** for local delivery.  

1. Your app makes an HTTP request  
2. **Transport (TCP)** → segment  
3. **IP (Layer 3)** → wraps in packet (source IP, destination IP)  
4. **Ethernet (Layer 2)** → adds MAC headers  
5. **NIC sends signals (Layer 1)**  

IP = the bridge between apps and the physical network.  
Every **VM you launch** has a private IP for inside traffic, and maybe a public IP for the Internet.  
**NAT glues the two worlds.**  

---

## Tiny Hands-on (CLI Proofs)


See all IPs on your machine
```bash
ip addr show
```

Only IPv4
```bash
ip -4 addr
```

Only IPv6
```bash
ip -6 addr
```

You’ll instantly see: your device isn’t just a computer — it’s a **node in a global web**.  


---

# IPv4 
---

## 1. What is IPv4?
IPv4 = Internet Protocol version 4.  
Think of it as the **address system of the internet**.  

Just like your house needs an address so mail can find you, every device on a network (computer, server, phone, IoT device) needs an IP address so **data packets know where to go**.  

### Examples of IPv4 addresses:
```
- 192.168.0.1
- 8.8.8.8
- 172.16.254.3
```

- Each IPv4 address is **32 bits long (4 bytes)**.  
- Written in **dotted decimal notation** → 4 numbers (each 0–255), separated by dots.  

Example: `192.168.1.10`  
```
192 → 11000000 in binary
168 → 10101000 in binary
1   → 00000001 in binary
10  → 00001010 in binary
Together → 11000000.10101000.00000001.00001010
```

---

### Commands

```bash
ifconfig        # older tool
ip addr show    # modern replacement
```

These will show your device’s IPv4 (both private and possibly public via NAT).  

---

## 2. Structure of IPv4 Address
IPv4 = **Network portion + Host portion**  

Example: `192.168.1.10/24`  
- **Network**: `192.168.1.0`  
- **Host**: `.10`  

Subnet mask: `/24 = 255.255.255.0` → means first 24 bits = network, last 8 bits = host.  

### Command 
```bash
ipcalc 192.168.1.10/24
```

Output:
```
Network:   192.168.1.0
Broadcast: 192.168.1.255
Host range: 192.168.1.1 - 192.168.1.254
Total hosts: 254
```

---

## 3. Classes of IPv4

| Class | Range                        | Default Mask      | Hosts per Network |
|-------|------------------------------|------------------|------------------|
| A     | 1.0.0.0 – 126.255.255.255    | /8 → 255.0.0.0   | ~16 million      |
| B     | 128.0.0.0 – 191.255.255.255  | /16 → 255.255.0.0| ~65k             |
| C     | 192.0.0.0 – 223.255.255.255  | /24 → 255.255.255.0 | 254          |
| D     | 224.0.0.0 – 239.255.255.255  | Multicast        | N/A              |
| E     | 240.0.0.0 – 255.255.255.255  | Experimental     | N/A              |

Today, we mostly use **CIDR (Classless)** for flexibility.  

---

## 4. Subnetting & CIDR (with Calculations)

**Formula:**  
- Total addresses = 2^(host bits)  
- Usable hosts = Total − 2 (network + broadcast)  

Example: `192.168.1.0/24`  
- Subnet mask: `255.255.255.0` → 8 host bits  
- Total = 2^8 = 256  
- Usable = 256 − 2 = 254 hosts  

Another: `10.0.0.0/16`  
- Subnet mask: `255.255.0.0` → 16 host bits  
- Total = 2^16 = 65,536  
- Usable = 65,534 hosts  

### Command
```bash
ipcalc 10.0.0.0/16
```

---

## 5. Private vs Public IPv4
**Private ranges (not routed on internet):**  
- `10.0.0.0 – 10.255.255.255 (/8)`  
- `172.16.0.0 – 172.31.255.255 (/12)`  
- `192.168.0.0 – 192.168.255.255 (/16)`  

**Public IPs:** routable on the internet.  

Your router does **NAT (Network Address Translation):**  
- Inside home: `192.168.1.10`  
- Internet sees: `49.36.xxx.xxx` (your ISP’s public IP).  

### Commands
```bash
curl ifconfig.me
# or
curl ipinfo.io/ip
```

---

## 6. IPv4 Packet Structure
An IPv4 packet has:  
- **Header (20–60 bytes):**  
  - Source IP  
  - Destination IP  
  - TTL (Time to Live)  
  - Protocol (TCP/UDP/ICMP)  
  - Checksum  
- **Payload:** actual data (website request, video chunk, etc.)  

Use **Wireshark** to capture and analyze IPv4 headers in real-time.  

---

## 7. Limitations of IPv4
- Only **4.3 billion addresses (2^32)** → not enough for billions of devices  
- Requires **NAT** and subnetting tricks  
- No built-in strong encryption or mobility  

---

## 8. IPv4 in Cloud Computing 
- Every **VM, container, load balancer, service** = needs an IP  
- **Private IPv4** → inside VPC  
- **Public IPv4** → internet-facing  
- **Elastic/Reserved IPs** → permanent IPv4s  
- **NAT Gateway** → private to public translation  


---

# IPv6 

---

## 1. What is IPv6?
- IPv6 = Internet Protocol version 6  
- Successor to IPv4 (because addresses ran out)  
- **128-bit address space (vs IPv4’s 32-bit)**  
- ~3.4 × 10³⁸ possible addresses  

Format: **hexadecimal + colon notation**  
Example:
```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```
Shortened:
```
2001:db8:85a3::8a2e:370:7334
```

---

## 2. Structure of IPv6 Address
IPv6 = **Network prefix + Interface ID**  
- First 64 bits = Network prefix  
- Last 64 bits = Host (Interface ID)  

Example: `2001:db8:abcd:0012::1/64`  
- Network: `2001:db8:abcd:0012::/64`  
- Host: `::1`  

### Command 
```bash
ip -6 addr show
```


---

## 3. Types of IPv6 Addresses
- **Unicast (1-to-1):**  
  - Global Unicast → public internet (2000::/3)  
  - Link-Local → always assigned (fe80::/10)  
  - Unique Local (ULA) → like private IPv4 (fc00::/7)  

- **Multicast (1-to-many):** `ff00::/8`  
- **Anycast (1-to-nearest):** same IP on multiple servers, routes to nearest  

No **broadcast** in IPv6 (multicast replaces it).  

---

## 4. Subnetting in IPv6
- Standard subnet = **/64**  
- 64 bits for network + 64 bits for host 
- Host space = 2^64 ≈ 18 quintillion hosts per subnet 

No NAT needed.  

---

## 5. IPv6 Packet Structure
- Fixed header size = **40 bytes**  
- Fields: Source, Destination, Flow label, Next header, Hop limit  
- No checksum (TCP/UDP handle it)  
- No router fragmentation (done by source)  

---

## 6. Features of IPv6
- Massive address space  
- **Auto-configuration (SLAAC)**  
- **No NAT needed** → global IP for each device  
- Built-in **IPsec**  
- Multicast instead of broadcast  

---

## 7. Commands (IPv6 Practice)
```bash
# Show IPv6
ip -6 addr show

# Ping IPv6 host
ping6 google.com

# Trace IPv6 route
traceroute6 google.com
```

---

# IPv4 vs IPv6 (Side by Side)

| Feature             | IPv4                      | IPv6                            |
|---------------------|--------------------------|---------------------------------|
| Address size        | 32-bit (~4.3 billion)    | 128-bit (~3.4 × 10³⁸)           |
| Notation            | Decimal, dotted (192.168.1.1) | Hex, colon (::1)             |
| Address exhaustion  | Yes (NAT required)        | No                              |
| Config              | Manual, DHCP              | SLAAC + DHCPv6                  |
| Private/Public      | Yes                       | ULA (fc00::/7)                  |
| NAT                 | Common                    | Not needed                      |
| Broadcast           | Yes                       | No (multicast instead)          |
| Header size         | 20–60 bytes               | Fixed 40 bytes                  |
| Security            | Optional (IPsec)          | Built-in IPsec                  |
| Cloud use           | Default (mainstay)        | Growing, dual-stack adoption    |

