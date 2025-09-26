# Subnet 
---

## 1. What is a Subnet?
**Subnet = Sub-network** → A smaller portion of a larger IP network.

---

An IP address is split into two parts:

- Network ID → tells you which network you’re on.
- Host ID → tells you which device inside that network.

But the IP itself (e.g., 192.168.1.15) doesn’t tell the computer where to cut between Network ID and Host ID.
That’s where Subnet Mask comes in — it’s like a mask/template that says:
- “These first X bits are the Network ID, and the rest are Host ID.”

Subnet mask looks like an IP: e.g., 255.255.255.0.
- In binary, 255.255.255.0 looks like:

```
11111111.11111111.11111111.00000000
```

- The 1s = Network bits
- The 0s = Host bits

So in 255.255.255.0:

- First 24 bits (1s) = Network ID
- Last 8 bits (0s) = Host ID

That’s why we also write it as /24 in CIDR notation.

---

### Purpose:
- Efficient IP address usage  
- Better security  
- Controlled traffic flow  

In both **IPv4 & IPv6**, subnetting = splitting a big network into smaller logical parts.  

### Example:
You own `192.168.0.0/16` (65,534 hosts).  
But you don’t want one giant flat network.  
So, you break it into multiple `/24` subnets (each with 254 hosts).  

---

## 2. Why Subnet?
- **Efficient IP usage** → no waste of huge ranges for small groups  
- **Security** → isolate departments, servers, or apps  
- **Performance** → smaller broadcast domains (IPv4)  
- **Control** → apply routing/firewall rules per subnet  
- **Cloud networking** → VPCs use subnets to separate public/private resources  

---

## 3. Subnet Mask (IPv4)
A **subnet mask** defines which part of the IP is **network** and which part is **host**.  

- Dotted decimal → `255.255.255.0`  
- CIDR → `/24`  

**IPv4:**  
- 32 bits → 4 octets → Example: `192.168.10.25`  
- Binary:  
  ```
  11000000.10101000.00001010.00011001
  ```

**Subnet mask:**  
- Determines **network portion vs host portion**  
- `/24` → first 24 bits = network, last 8 = hosts  

---

**IPv6:**  
- 128 bits → 8 groups of 16 bits  
- Example: `2001:db8:85a3::8a2e:370:7334`  
- Standard subnet size = `/64` → 64 bits network, 64 bits host  

---

## 4. How Subnetting Works

### Step 1: Decide number of subnets or hosts
- If you know how many subnets you need → **calculate borrowed bits**  
- If you know how many hosts per subnet → **calculate host bits needed**  

### Formulas (IPv4):
- **Total hosts** = 2^(host bits) − 2  
- **Number of subnets** = 2^(borrowed bits)  
- **New subnet mask** = default mask + borrowed bits  

---

### Example 1: Number of Subnets
- Network: `192.168.1.0/24`  
- Requirement: 4 subnets  

Default `/24` → 8 host bits  

Number of subnets = 2^(borrowed bits) → 2^2 = 4 → borrow 2 bits  

New subnet mask = `/24 + 2 = /26`  

**Subnet ranges:**  
- `192.168.1.0/26` → hosts: 1–62  
- `192.168.1.64/26` → hosts: 65–126  
- `192.168.1.128/26` → hosts: 129–190  
- `192.168.1.192/26` → hosts: 193–254  

---

### Example 2: Number of Hosts
- Requirement: **50 hosts per subnet**  

Hosts bits = smallest n such that `2^n − 2 ≥ 50`  
- 2^6 − 2 = 62 → enough  

Subnet mask = 32 − 6 = `/26`  

Same ranges as above  

---

## 5. Commands for Subnetting

On **Linux**:
```bash
# Calculate IPv4 subnet
ipcalc 192.168.1.0/26

# Show IPv6 details
sipcalc 2001:db8::/48
```

---

## 6. IPv4 vs IPv6 Subnetting Recap

| Feature         | IPv4                      | IPv6                       |
|-----------------|---------------------------|----------------------------|
| Address size    | 32-bit                    | 128-bit                    |
| Host limit      | 2^32                      | 2^128                      |
| Broadcast       | Yes                       | No (multicast instead)     |
| NAT             | Needed                    | Not needed                 |
| Standard subnet | Variable (/24, /26 …)     | /64                        |
| Complexity      | Medium → need math        | Simple → almost always /64 |
| Cloud use       | Mainstay                  | Dual-stack optional, growing |

---

Subnetting = math + logic to break large IP blocks into smaller ones.  
Cloud VPCs, Kubernetes, and enterprise networks all rely on it for **efficiency, isolation, and security**.  

---

# CIDR

---

**CIDR = Classless Inter-Domain Routing**  

- Introduced in **1993** to replace the old **classful IP system (Class A, B, C)**.  
ables on the internet.  

### CIDR Format
```
IP_address / Prefix_length
```

Example:  
```
192.168.1.0/24
```

- `/24` = prefix length → first 24 bits = network  
- Last 8 bits = host portion  

---

## Conceptual Foundation: Why CIDR Exists

### Before CIDR → Classful addressing:
- **Class A → /8 → 16 million hosts**  
- **Class B → /16 → 65k hosts**  
- **Class C → /24 → 254 hosts**

### Problems:
- Wasted addresses → Class B often too big, Class C too small  
- Routing table explosion → routers had to track millions of small networks  

### CIDR solution:
- Remove rigid classes → “classless”  
- Represent networks as **IP/prefix** → flexible subnetting & supernetting  
- Aggregates multiple small networks into one routing entry  

---

## CIDR Notation Explained
### IPv4:
- `/8` → 255.0.0.0 → first 8 bits = network  
- `/16` → 255.255.0.0 → first 16 bits = network  
- `/24` → 255.255.255.0 → first 24 bits = network  

### IPv6:
- `/64` standard subnet → first 64 bits network, last 64 bits host  

**Rule of Thumb:**  
- Bigger prefix (like `/30`) → fewer hosts  
- Smaller prefix (like `/16`) → more hosts  

---

## CIDR Calculations
### Formulas:
- **Number of hosts per subnet:**  
  ```
  Hosts = 2^(32 - prefix length) - 2
  ```
- **Number of subnets possible:**  
  ```
  Subnets = 2^(new prefix - old prefix)
  ```

---

### Example 1: IPv4
- **Network:** 192.168.1.0/24  
- **Want:** /26 subnets  

**Step 1: How many subnets?**  
- Borrowed bits = 26 − 24 = 2 bits  
- Subnets = 2^2 = 4  

**Step 2: Hosts per subnet**  
- Host bits = 32 − 26 = 6  
- Hosts = 2^6 − 2 = 62  

**Subnet Ranges:**  
- 192.168.1.0/26 → 1–62  
- 192.168.1.64/26 → 65–126  
- 192.168.1.128/26 → 129–190  
- 192.168.1.192/26 → 193–254
  
---

## CIDR Tips & Tricks
- **Subnet increment = block size**  
  ```
  2^(32 − prefix length)
  ```
- CIDR calculators → essential for large networks (`ipcalc`, online tools)  
- Remember binary conversion → helps visualize borrowed bits & host bits  
- **IPv6 is simpler** → almost always `/64`, only network planning matters  

---

## IPv4 vs IPv6 CIDR Comparison

| Feature          | IPv4                          | IPv6                          |
|------------------|-------------------------------|-------------------------------|
| Address space    | 32-bit                        | 128-bit                       |
| Hosts per subnet | 2^(32−prefix) − 2             | 2^(128−prefix)                |
| Typical subnet   | /24, /26                      | /64                           |
| Supernetting     | Yes                           | Yes                           |
| Routing efficiency | Needed                      | Better + simple               |

---

## Advanced CIDR Concepts

---

### 4.1 Supernetting
- Combine multiple small subnets → one bigger route  
- Reduces router table entries  

**Example:**
```
192.168.0.0/24
192.168.1.0/24
192.168.2.0/24
192.168.3.0/24
```
Can be advertised as `/22` → covers `192.168.0.0 – 192.168.3.255`

---

### 4.2 VLSM (Variable Length Subnet Mask)
- Allocate subnets of different sizes from the same network  

**Example:**
```
192.168.1.0/24
- HR: /26 → 62 hosts
- IT: /27 → 30 hosts
- Finance: /28 → 14 hosts
```
Avoids wasting IPs  
Critical for enterprise & cloud design  

---

### 4.3 Hierarchical Subnetting
- Plan IPs **top-down** → region → department → subnet → host  

**In cloud, map:**
- `/16` → VPC  
- `/24` → public/private subnet per AZ  
- `/28` → app cluster subnet  

