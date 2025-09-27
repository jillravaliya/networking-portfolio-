# DHCP (Dynamic Host Configuration Protocol)  
 
---

## Where Do IPs Live?  

Imagine:  
- You just bought a new phone.  
- You connect to Wi-Fi at home.  
- Suddenly, you can watch YouTube, browse, WhatsApp.  

**Question:** Who gave your phone an IP address?  
Answer → The Wi-Fi router runs a **DHCP service**.  

---

### Without DHCP
You’d open network settings and manually type:  
```
IP:      192.168.1.25
Subnet:  255.255.255.0
Gateway: 192.168.1.1
DNS:     8.8.8.8
```
One wrong digit → internet doesn’t work.  

Every time you travel (café, office, airport) → manual nightmare.  

---

### With DHCP
DHCP automates this.  
It’s like a **welcome desk clerk** at every network:  

- **Device:** “I just arrived, give me details.”  
- **DHCP:** “Here’s your IP, gateway, DNS. Enjoy!”  

DHCP = **automation of IP management**.  

---

## Why DHCP?
- **Automation** → No manual IP entry.  
- **Scalability** → Works for 10 devices at home or 10,000 in enterprise/cloud.  
- **Avoid conflicts** → Ensures unique IPs.  
- **Centralized management** → Admins control all configs from one place.  
- **Cloud perspective** → DHCP = invisible backbone of IP assignment inside VPCs.  

---

## How DHCP Works (Deep Mechanics: DORA)  

DHCP is a **client–server protocol**.  
The exchange goes through **broadcasts** (at least at first).  

### Step 1: Discover  
- Client broadcasts: “I need an IP!”  
- Sent to `255.255.255.255` because client doesn’t know subnet.  
- IPv6 → sent as multicast to `ff02::1:2`.  

### Step 2: Offer  
- Server replies:  
  `192.168.1.50, mask 255.255.255.0, gateway 192.168.1.1, DNS 8.8.8.8`.  

### Step 3: Request  
- Client says: “Okay, I’ll take 192.168.1.50.”  

### Step 4: Acknowledge  
- Server confirms: “Done, that’s yours for 24 hours lease.”  

This is called the **DORA handshake**.  

---

## DHCP Lease Timers (Going Deep)  

Every IP = **lease (temporary ownership)**.  

- **Lease Time** → e.g., 24 hours.  

**Renewal Process:**  
- At 50% lease time → client unicasts `DHCPREQUEST` to server.  
- If confirmed → lease extended.  
- If no reply → retries at 87.5% lease time.  
- If still no reply → lease expires, IP released.  

Prevents IP starvation, keeps pool dynamic.  

---

## What DHCP Assigns (Options)  

DHCP gives not just IP but also:  
- IP address  
- Subnet mask  
- Default gateway  
- DNS servers  
- NTP server (time sync)  
- WINS server (legacy Windows)  
- Domain name  

DHCP = **centralized config hub**.  

---

## DHCP for IPv4 vs IPv6  

- **DHCPv4** → Full DORA handshake, assigns IP + config.  
- **DHCPv6** → Often only DNS + options, since IPv6 has **SLAAC**.  

Many networks = SLAAC (IP) + DHCPv6 (DNS).  

---

## Practical DHCP (Commands)  

```bash
# Show current IPs
ip addr show

# Check DHCP lease file
cat /var/lib/dhcp/dhclient.leases
```

---

## DHCP in Cloud Computing  

In cloud, DHCP is **hidden but always running**.  

Example: AWS EC2 in VPC subnet → AWS runs invisible DHCP:  
- Assigns private IP from subnet (e.g., `10.0.1.25/24`).  
- Default gateway → `10.0.1.1`.  
- DNS → Amazon Resolver `169.254.169.253`.  

---

## DHCP Limitations  

- **Single point of failure** → if server dies → no device gets IP.  
  - Solution: redundant DHCP servers, failover.  
- **Rogue DHCP servers** → can assign fake IPs/DNS (security risk).  
- **Not suitable for static servers** → DBs, routers need fixed IPs.  

---

## Summary Mental Model  

- DHCP = automated IP assignment system.  
- Works via **DORA process (Discover, Offer, Request, Ack)**.  
- Assigns IP + full network config.  
- Uses leases to recycle addresses.  
- Exists in every **cloud VPC subnet** (invisible to user).  
- Makes scaling networks **practical & reliable**.  

---

# NAT

---

## Why NAT Exists?  

Imagine you’re at home:  

- You have 4 devices: laptop, phone, smart TV, printer.  
- Your ISP gave you **one public IP** (e.g., `49.32.55.120`).  
- But each device needs an IP to reach the internet.  

**Problem:**  
- Public IPv4 addresses are limited.  
- Billions of devices can’t all have unique public IPs.  
- Private IPs cannot directly talk to the public internet.  

If you send a packet from `192.168.1.10` to Google (`142.250.x.x`), Google won’t know how to reply back — because `192.168.x.x` is **not globally unique**, it only exists inside your house.  

**Solution:** 

NAT (Network Address Translation)
- it allows many private IPs (e.g., `192.168.1.10`, `.20`, `.30`)  
- to share **one public IP** for internet access.  

---

## NAT in Simple Words  

A technique where a router/firewall modifies source/destination IPs in packet headers to allow communication between **private and public networks**. 
 
- NAT = a translator at the border.  
- Inside → You speak “private language” (`192.168.x.x`).  
- Outside → Internet only understands “public language” (`49.32.55.120`).
-  Defined in **RFC 1631**. 

NAT sits at your router (or cloud gateway) and **rewrites IP addresses in packet headers**.  

---

## Why NAT?  

-  Solved IPv4 exhaustion problem.  
- **Address Conservation** → Millions share few public IPs.  
- **Security by obscurity** → Internal IPs hidden.  
- **Flexibility** → Use `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` privately.  
- **Cloud Scaling** → VMs/containers in private subnets talk outside via NAT gateways.  

---

## NAT Types (Getting Deeper)  

### Static NAT  
- One private ↔ one public (fixed).  
- Example: Map `192.168.1.50` (internal web server) → `49.32.55.121`.  
- Outsiders can always reach it.  

### Dynamic NAT  
- Many private ↔ many public (from a pool).  
- Example: 100 users inside, router has 10 public IPs → assigns as needed.  

### PAT (Port Address Translation) = NAT Overload  
- Many private IPs share one public IP, NAT distinguishes by **port**.  
- This is what your **home Wi-Fi or AWS NAT Gateway** does.  

**PAT is most common** — because ISPs usually give only **1 public IP**.  

---

## The First Deep Step: How It Actually Works  

Let’s track a single packet.  

- **Laptop:**  
  IP = `192.168.1.10`  
  Wants Google (`142.250.180.78`)  

**Packet before NAT (from laptop):**  
```
Src IP: 192.168.1.10
Dst IP: 142.250.180.78
```

**Router rewrites:**  
```
Src IP: 49.32.55.120   (public IP)
Dst IP: 142.250.180.78
```

**NAT Table Entry:**  
```
192.168.1.10:54321 → 49.32.55.120:40001
```

**Reply from Google:**  
```
Src IP: 142.250.180.78
Dst IP: 49.32.55.120:40001
```

**NAT translates back:**  
```
Dst IP: 192.168.1.10:54321
```

Laptop receives reply → Success 

NAT = middleman rewriting addresses so communication works.  

---

## NAT vs. No NAT  

- **Without NAT:**  
  Every device needs its own public IP → impossible.  

- **With NAT:**  
  Thousands of devices share one IP.  
  Internet only sees your **router**, not individuals.  

---

## NAT in Cloud  

Example: **AWS VPC**  

- VPC CIDR = `10.0.0.0/16`  
- Public subnet = `10.0.1.0/24` (web servers with public IPs).  
- Private subnet = `10.0.2.0/24` (databases, apps).  

Private subnet machines cannot directly reach internet.  

**Solution → NAT Gateway**  

- Database (`10.0.2.15`) → wants updates.  
- NAT Gateway rewrites → `54.23.67.200` (public IP).  
- Internet replies → NAT maps back → `10.0.2.15`.  

NAT = only way private cloud subnets talk to outside.  

---

## NAT Commands / Config  

```bash
# Check NAT table (Linux)
sudo iptables -t nat -L -n -v
```

---

## NAT Limitations  

- Breaks **end-to-end principle** (IP no longer unique).  
- Some protocols fail (SIP, FTP, VoIP) unless helpers exist.  
- Performance overhead (maintaining tables).  
- Debugging harder (many users hidden behind 1 IP).  
- Inbound problem → NAT works outbound fine, but inbound needs mapping (Elastic IP).  

---

## Security Angle  

**Advantage:**  
- Internal network hidden, outsiders can’t directly see devices.  

**Reality:**  
- NAT ≠ Firewall.  
- It just translates addresses → need **firewall rules** for real security.  

---

## Real Enterprise Example  

- **Internal network** → `10.10.0.0/16`  
- 5,000 employees  
- Public IP block → `/28` (16 addresses)  

**Setup:**  
- Employees browsing → **NAT overload (PAT)**.  
- Web server inside → **Static NAT mapping** (1 public IP).  
- AWS workloads (private subnets) → **NAT Gateway** for outbound.  

---
