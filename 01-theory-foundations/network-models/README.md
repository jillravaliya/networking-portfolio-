# Network Models  

Networking is complex — so engineers created **models** to break it into structured layers.  
These models help us **understand, design, and troubleshoot networks** from theory to real-world implementation.  

---

## Why Models?  
- Provide a **common language** → engineers across companies use the same terms.  
- Break networking into **layers**, so problems can be isolated.  
  - Example: “Is it a cabling issue (Layer 1), or DNS (Layer 7)?”  
- Link **theory (OSI)** with **reality (TCP/IP)**.  

---

## Contents  

### [OSI Seven-Layer Model](./osi-seven-layers)  
- Classic **7-layer reference model**.  
- More **theoretical**.  
- Used in interviews, study, and troubleshooting.  
- Layers: Physical → Data Link → Network → Transport → Session → Presentation → Application.  

---

### [TCP/IP Five-Layer Model](./tcpip-five-layers)   
- The **real-world Internet model**.  
- Practical, used in all actual networking.  
- Layers: Physical → Data Link → Internet → Transport → Application.  
- Simpler, but maps well to OSI.  

---

### OSI vs TCP/IP  
- **OSI** → Ideal for **learning, teaching, troubleshooting**.  
- **TCP/IP** → Real-world **implementation**, the Internet runs on it.  
- Together → Complete picture: *theory + practice*.  

---

## Why It Matters for Cloud & DevOps  
- Debugging: knowing which layer fails saves hours (e.g., DNS vs routing vs app bug).  
- Security: firewalls, proxies, and TLS all live at specific layers.  
- Cloud networking (VPCs, load balancers, service meshes) all map back to these models.  

---

Choose which model you want to dive into: 
- [OSI Seven Layers](./osi-seven-layers)  
- [TCP/IP Five Layers](./tcpip-five-layers)  

