# DNS 
---

## Why DNS Exists?  

Imagine this scenario:  

- You want to call your best friend.  
- You could memorize their phone number `+91-98765-43210`, but it’s hard to remember hundreds of numbers.  
- Instead, you just save **“Nidhi”** in your contacts and your phone automatically connects to the right number.  

**DNS is exactly like that for the internet:**  

- Computers/servers communicate using IPs (e.g., `142.250.180.78` → Google).  
- Humans prefer names (e.g., `google.com`).  

**DNS = the phonebook of the internet** → translates human-friendly names into machine-friendly IPs.  

**Without DNS:**  
- You’d need to remember every IP.  
- Browsing would be impractical → internet becomes a nightmare.  

---

## Scratch Definition  

- **DNS = Domain Name System**  
- A **hierarchical, distributed naming system** that translates **domain names → IP addresses**.  
- Defined in **RFC 1034 & 1035**.  
- Supports both **IPv4 (A records)** and **IPv6 (AAAA records)**.  

---

## Why DNS?  

- **Memorability** → Humans remember names better than numbers.  
- **Flexibility** → Websites can move servers/IPs without changing domain.  
- **Scalability** → Supports billions of devices globally.  
- **Cloud-native architecture** → AWS, Azure, GCP services rely on DNS for service discovery & load balancing.  

---

## How DNS Works (Deep Mechanics)  

DNS = multi-step lookup:  

**Step 1:** You type a URL → `https://www.google.com`  
**Step 2:** Browser asks local resolver → “What’s the IP of www.google.com?”  
**Step 3:** Resolver queries **root servers** → “Who handles `.com` domains?”  
**Step 4:** Resolver queries **TLD servers** → gets `.com` name servers.  
**Step 5:** Resolver queries **authoritative server** → gets actual IP.  
**Step 6:** Resolver responds to client → Browser connects to `142.250.180.78`.  

⚡ All this in **milliseconds**.  

---

## DNS Records (Going Deep)  

| Record Type | Purpose |
|-------------|---------|
| **A**       | IPv4 address for domain |
| **AAAA**    | IPv6 address for domain |
| **CNAME**   | Alias for another domain (e.g., www → root domain) |
| **MX**      | Mail server routing |
| **NS**      | Name server for the domain |
| **PTR**     | Reverse DNS lookup (IP → name) |
| **TXT**     | Text info (SPF, DKIM, verification) |
| **SRV**     | Service records (SIP, LDAP, cloud) |

---

## DNS in Cloud  

- **AWS** → Route 53 (managed DNS)  
- **Azure** → Azure DNS  
- **GCP** → Cloud DNS  

**Use cases in cloud:**  
- **Service discovery** → Kubernetes services use DNS.  
- **Load balancing** → DNS maps to multiple IPs.  
- **Failover** → DNS points to healthy servers automatically.  

If cloud server IP changes → update DNS once → users still connect.  

---

## Practical Commands  

**Linux:**  
```bash
nslookup google.com
dig google.com
dig google.com A
dig google.com AAAA
```
---

## DNS Security Considerations  

- **DNS Spoofing / Poisoning** → attacker returns wrong IP.  
  - Mitigation → **DNSSEC** (cryptographic signing).  
- **DDoS attacks on DNS** → domains unreachable.  
  - Mitigation → **Anycast + distributed DNS servers**.  
- **Internal vs External DNS** →  
  - VPCs use private DNS for internal comms.  

---

## Real-World Example (Enterprise + Cloud)  

**Company Example:**  
- Internal apps → `app1.internal.company.com`  
- Public website → `www.company.com`  

**DNS Setup:**  
- Internal resolver → only resolves `internal.company.com`.  
- Public resolver → resolves `www.company.com`.  
- Cloud load balancer → `app1.company.com` → maps to multiple private IPs behind NAT.  

---

## Summary  

- **DNS = Internet’s phonebook**  
- Domain name → Resolver → Root → TLD → Authoritative → IP  
- **Record types**: A, AAAA, CNAME, MX, etc.  
- **Caching & TTL** = efficiency.  
- **Cloud** = DNS powers service discovery, load balancing, failover.  

---


