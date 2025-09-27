# Cloud Networking

---

## Why Cloud Networking Exists?  

Imagine this:  

You’re organizing a **music festival**.  

- Thousands of people  
- Multiple stages  
- Food stalls  
- VIP areas  

Everyone needs access to the **right place at the right time**:  

- **Stage A** → performers only  
- **VIP area** → ticketed guests only  
- **Public zone** → everyone  

In a cloud environment, your **applications, servers, databases, and users** are like these people.  

### Cloud Networking = Roads, Bridges, and Security Guards  

- **Roads** → how traffic flows between servers  
- **Bridges** → connect public internet to private cloud  
- **Security guards** → firewalls, ACLs, NAT to control access  

Without proper cloud networking:  

- Users could get lost → requests fail  
- Traffic bottlenecks → slow applications  
- Security gaps → breaches  

---

## Scratch Definition  

**Cloud Networking =** the combination of networking components and services that connect, secure, and manage communication between cloud resources and the internet.  

---

## Why Cloud Networking?  

- **Segmentation & Isolation** → keep apps, databases, and public-facing services separate  
- **Security** → control who accesses what (firewalls, ACLs, NAT)  
- **High Availability** → route traffic intelligently, multi-AZ or multi-region  
- **Scalability** → add more servers, subnets, and regions without breaking network  
- **Hybrid Cloud / Multi-Cloud** → connect private datacenter to cloud securely  

---

## Components — Layer by Layer  

### 1. VPC (Virtual Private Cloud)  
- Your **isolated cloud network**  
- You choose **IP range** (CIDR block, e.g., `10.0.0.0/16`)  
- Can create **multiple subnets** within it  
- Acts as the **foundation** for all networking  

---

### 2. Subnets  
- Divide VPC into **smaller logical networks**  

### Types:  
- **Public Subnet** → Direct internet access via **Internet Gateway (IGW)**  
- **Private Subnet** → No direct internet access → uses **NAT Gateway** for outbound  

---

### 3. Route Tables  
- Decide **where traffic goes**  

### Example:  
- `0.0.0.0/0 → IGW` → all internet traffic goes via Internet Gateway  
- `10.0.2.0/24 → Local` → internal subnet traffic stays within VPC  

---

### 4. Internet Gateway (IGW)  
- Connects **VPC to the internet**  
- Required for **public subnets** to reach outside world  

---

### 5. NAT Gateway  
- Allows **private subnets** to access internet outbound only  
- Doesn’t allow inbound traffic → keeps private resources **secure**  

---

### 6. Security Groups  
- **Instance-level firewall**  
- **Stateful** → return traffic is automatically allowed  

### Example Rules:  
- Allow inbound **TCP 22 (SSH)** only from **office IP**  
- Allow inbound **TCP 443 (HTTPS)** from **anywhere**  

---

### 7. Network ACLs (NACLs)  
- **Subnet-level firewall**  
- **Stateless** → rules must explicitly allow both **inbound and outbound**  
- Acts as an **extra security layer** outside Security Groups  

---

### 8. VPN / Direct Connect  
- Securely connect **on-premises network** or **remote users**  
#### Types:  
- **Site-to-Site VPN** → office ↔ VPC  
- **Client VPN** → remote employee ↔ VPC  
- **Direct Connect** → private **dedicated high-speed link** to cloud  



---

## How Cloud Networking Works 

---

### Creating a Virtual Network
- When you create a **VPC (AWS)** or **VNet (Azure)**, the cloud provider carves out an **isolated IP space** from its massive global network backbone.  
- **Example:** `10.0.0.0/16`  
This is your **private network playground**.  

---

### Subnets: Dividing the Network
- You split the VPC into **subnets**.  
  - **Public Subnet:** connected to the internet (via Internet Gateway).  
  - **Private Subnet:** isolated, no direct internet.  
- Each subnet sits inside a **datacenter (AZ)**.  

Ensures **organization + fault tolerance**.  

---

### Routing Traffic
- **Route Tables** define where packets go:  
  - To the **internet** → via **Internet Gateway (IGW)**.  
  - From **private subnet** → via **NAT Gateway** (outbound only).  
  - To another **VPC** → via **Peering/Transit Gateway**.  

Routes = **road maps for packets**.  

---

### Security Filtering
- **Security Groups (SGs):** Instance-level firewalls (**stateful**).  
- **Network ACLs (NACLs):** Subnet-level firewalls (**stateless**).  
- Together, they decide **who can talk to whom, on which port, inbound/outbound**.  

Like having **door locks (SGs) + colony gates (NACLs)**.  

---

### Handling Incoming User Traffic
- A user request (`https://myapp.com`) enters through:  
  - **Load Balancer (L4 or L7)** → distributes traffic to multiple servers.  
  - Load balancer runs **health checks** → ensures only healthy servers get traffic.  
  - If demand increases → **Auto-Scaling** launches more servers, LB auto-adds them.  

Ensures **reliability and performance**.  

---

### Outbound & Hybrid Connectivity
- **Private subnet instances** (needing internet, e.g., for updates): use **NAT Gateway**.  
- **Hybrid cloud/on-premises:**  
  - **VPN Gateway** → encrypted tunnel.  
  - **Direct Connect** → private high-speed link.  

Connects **cloud + outside world securely**.  

---

### Behind the Scenes: SDN
- Powered by **Software-Defined Networking (SDN)**.  

**Control Plane (cloud provider’s brain):**  
- Decides:  
  - Which IPs belong where  
  - Which packets follow which routes  
  - How isolation is enforced  

**Data Plane (physical backbone):**  
- Simply forwards packets at high speed.  

**SDN = invisible magic** enabling millions of isolated virtual networks on one global backbone.  


---

## Cloud Networking Services

### AWS  
- VPC → isolated cloud network  
- Subnets → public/private  
- Internet Gateway → allow internet access  
- NAT Gateway → outbound for private subnets  
- Route Tables → define traffic paths  
- Security Groups → instance-level firewall  
- Network ACLs → subnet-level firewall  
- Direct Connect → private dedicated connection to on-prem  

### Azure  
- Virtual Network (VNet) → same as VPC  
- Subnets, NSGs, Route Tables → similar structure  
- VPN Gateway / ExpressRoute → connect on-premise network  

### GCP  
- VPC Network → global scope  
- Subnets → regional  
- Cloud NAT → outbound access for private instances  
- Firewall rules → control traffic  

---

## Real-World Example

- **VPC** → 10.0.0.0/16  
- **Public Subnet** → Web servers → IGW → Internet  
- **Private Subnet** → DB servers → NAT → Updates/patches  
- **Security Groups** → allow HTTPS to web, block all else  
- **Network ACLs** → block unwanted ports/subnets  
- **VPN** → employees access private subnet securely  
- **Load Balancer** → distributes traffic across web servers  
- **Cloud DNS** → internal services resolve private names  

Everything is connected and secure, with **defined rules for who can talk to whom, and how traffic flows**.  

---

## Summary  

Think of Cloud Networking as:  

- **City layout** → VPC & subnets  
- **Roads & bridges** → IGW, NAT, route tables  
- **Security guards** → Security Groups, NACLs, Firewalls  
- **Tunnels** → VPN, Direct Connect  
- **Street signs** → Route tables & DNS  
- **Traffic management** → Load balancers, monitoring  

Once you see it like a **living city**, all the individual pieces (IP → DHCP → NAT → DNS → VPN → Firewall) suddenly make sense in **cloud context**.  
          

