# 🧠 **Azure Virtual Network (VNet) — Short Notes**

---

### 🔷 **1. Virtual Network (VNet)**  
**Definition**:  
An Azure VNet is a **private, isolated network** in the Azure cloud, similar to a traditional on-premises network.
**It is a fundamental building block for creating a private network in Azure, providing isolation and control over your cloud resources**

**Application**:  
Used to host and securely connect Azure resources like VMs, databases, and app services.

**Use Case**:  
Create a VNet to group your application components and control their communication within Azure — e.g., a VNet for a 3-tier web app (frontend, backend, DB).

---

### 🔷 **2. Subnet**  
**Definition**:  
A **subdivision of a VNet** used to organize and isolate resources.

**Application in VNet**:  
Split your VNet into smaller networks — like a public subnet for internet-facing VMs and a private subnet for internal systems.

**Use Case**:  
- **Public Subnet**: Host a Bastion or public-facing web server  
- **Private Subnet**: Host database servers or internal services not exposed to the internet

---

### 🔷 **3. Network Security Group (NSG)**  
**Definition**:  
An NSG is like a **firewall** that controls inbound and outbound traffic at the subnet or NIC (network interface) level.

**Application in VNet**:  
Apply NSG to subnets or VMs to define rules (e.g., allow SSH from specific IPs, block all others).

**Use Case**:  
Allow port 22 (SSH) only from your laptop to the public VM; deny all traffic to the private VM except from internal IPs.

---

### 🔷 **4. Public IP Address**  
**Definition**:  
An IP address assigned by Azure to expose a resource (like a VM) to the **public internet**.

**Application in VNet**:  
Attach to a VM in the public subnet to allow remote access (e.g., SSH or RDP).

**Use Case**:  
Use a public IP for your Bastion or jump-box VM to access your private VMs through it.

---

### 🔷 **5. Private IP Address**  
**Definition**:  
An IP address used for **internal communication** within a VNet.

**Application in VNet**:  
Every VM or service in a subnet gets a private IP to communicate securely inside the VNet.

**Use Case**:  
Private VM in backend subnet talks to the database or app server securely using private IP.

---

### 🔷 **6. NAT Gateway (Outbound Internet Access)**  
**Definition**:  
A NAT (Network Address Translation) Gateway allows **private subnet VMs to access the internet** for outbound traffic, without having a public IP.

**Application in VNet**:  
Attach a NAT gateway to your private subnet to enable internet access for patching, downloads, or API calls.

**Use Case**:  
A private VM downloads software updates or pulls packages securely — it can go out to internet, but the internet can't come in.

---

### 🔷 **7. Service Endpoint**  
**Definition**:  
A feature that allows secure access to Azure PaaS services (like Storage or SQL) **over the Azure backbone network**, bypassing the internet.

**Application in VNet**:  
Enable service endpoint in your subnet, then restrict access to the Azure service (like Storage Account) only to that subnet.

**Use Case**:  
Private VM in your VNet securely accesses a Storage Account without using public IP or exposing the storage to the internet.

---

## **How It All Works Together – Example Scenario**

Imagine you're deploying a **secure web application** in Azure:

| Component | Purpose | Example |
|----------|---------|---------|
| **VNet** | The overall private network | `my-vnet` with address space `10.0.0.0/16` |
| **Subnets** | Divide network by function | `public-subnet` for jump-box, `private-subnet` for app/database |
| **NSG** | Define who can talk to what | Allow SSH to jump-box; deny all others to private VM |
| **Public IP** | Internet access to entry-point | Jump-box VM gets a public IP |
| **Private IP** | Internal secure communication | Private VM uses `10.0.2.4` to talk to DB |
| **NAT Gateway** | Allow private VM to reach internet | Private VM downloads updates via NAT |
| **Service Endpoint** | Secure access to Azure services | Private VM reads/writes to Azure Storage privately |

