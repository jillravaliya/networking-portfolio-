# Addressing  

Every device in a network needs a unique identity to communicate.  
That identity is the **IP address** — the foundation of routing, cloud networks, and the Internet itself.  

This section covers the **core concepts of addressing**: from the basics of IPv4 and IPv6 to how networks are logically divided using **subnetting and CIDR**.  

---

## Why Addressing Matters  
- Without addressing, data packets would be lost — like letters with no recipient address.  
- Subnetting ensures networks are efficient, secure, and scalable.  
- CIDR (Classless Inter-Domain Routing) revolutionized IP allocation, enabling modern Internet growth.  
- In **cloud networking (AWS VPCs, Azure VNets, GCP VPCs)** → designing IP ranges correctly is a critical first step.  

---

## What’s Inside  

### [IP Fundamentals](./ip-fundamentals)  
- Basics of **IPv4** structure (network + host portions).  
- **Private vs Public IPs**, static vs dynamic assignment.  
- Short intro to **IPv6** and why it exists.  
- Real-world examples: home networks, enterprise IP allocation.  

---

### [Subnetting & CIDR](./subnetting-and-cidr)  
- Understanding **subnet masks** and how they divide networks.  
- Calculating usable IPs in a subnet.  
- **CIDR notation (/24, /16, /12, etc.)** and why it replaced classful addressing.  
- Role in **VPC design**: avoiding overlapping ranges, planning for growth.  
- Troubleshooting scenarios: “Why can’t two subnets talk?”  

---

## Cloud & DevOps Relevance  
- **VPCs and Subnets:** Designing AWS VPCs or Kubernetes clusters always starts with IP planning.  
- **Security:** Smaller subnets = controlled access zones.  
- **Scalability:** CIDR allows carving networks without wasting IPs.  
- **Troubleshooting:** Understanding masks/CIDR is key when fixing connectivity issues.  

---

### Choose which model you want to dive into:

- [IP Fundamentals](./ip-fundamentals) 
- [Subnetting & CIDR](./subnetting-and-cidr)  

