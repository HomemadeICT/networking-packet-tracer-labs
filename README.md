# 📡 Networking Packet Tracer Labs & Enterprise Network Project

<div align="center">

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer%20v8.1%2B-blue?style=for-the-badge&logo=cisco&logoColor=white)
![NVQ Level](https://img.shields.io/badge/NDICT-NVQ%20Level%205-green?style=for-the-badge)
![Labs](https://img.shields.io/badge/Labs-15%2B-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)
[![GitHub](https://img.shields.io/badge/GitHub-HomemadeICT-black?style=for-the-badge&logo=github)](https://github.com/HomemadeICT)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Theekshana%20Prabodha-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/theekshana-prabodha/)

**A comprehensive, hands-on Networking Practical Book built in Cisco Packet Tracer**  
*From basic LAN connectivity to a full 80-Workstation Enterprise Network Design*

**Author**: D.M. Theekshana Prabodha | **Brand**: TheekMind | **Qualification**: NDICT NVQ Level 5

</div>

---

## 📋 Table of Contents

| # | Section | Description | Labs |
|---|---------|-------------|------|
| [01](#-section-01--basic-connectivity) | 🔌 Basic Connectivity | LAN topology, routing fundamentals | 1 lab |
| [02](#-section-02--network-services) | 🌐 Network Services | DHCP, DNS, Web, Mail servers | 5 labs |
| [03](#-section-03--vlan--switching) | 🔀 VLAN & Switching | VLANs, Inter-VLAN routing | 4 labs |
| [04](#-section-04--security--remote-access) | 🔒 Security & Remote Access | Port Security, Telnet | 2 labs |
| [05](#-section-05--wireless--iot) | 📶 Wireless & IoT | Wireless networking, IoT integration | 2 labs |
| [06](#-section-06--final-project) | 🏢 Final Project | Enterprise network, 80-workstation proposal | 2 projects |

---

## 🛠️ Technologies & Tools

| Tool/Technology | Purpose |
|-----------------|---------|
| **Cisco Packet Tracer** | Network simulation and lab design |
| **VLANs & VTP** | Network segmentation and management |
| **DHCP / DNS / Web / Mail** | Core network services |
| **Port Security / ACLs** | Network security |
| **Router-on-a-Stick** | Inter-VLAN routing technique |
| **Wireless (WRT300N)** | Wireless access point configuration |
| **IoT Devices** | Smart device network integration |
| **Cat6 Cabling** | Physical network infrastructure |

---

## 🧠 Skills Demonstrated

- ✅ Network topology design and implementation
- ✅ VLAN configuration and inter-VLAN routing
- ✅ Network services deployment (DHCP, DNS, HTTP, SMTP)
- ✅ Network security (port security, ACLs, Telnet)
- ✅ Wireless network configuration
- ✅ IoT device integration
- ✅ Enterprise infrastructure planning and cost estimation
- ✅ Professional technical documentation

---

## 🚀 Getting Started — Open & Experiment with the Labs

All labs include a **Cisco Packet Tracer `.pkt` file** — a fully built and configured network you can open, inspect, modify, and break (then fix). This is the best way to learn.

---

### Step 1 — Get Cisco Packet Tracer (Free)

Packet Tracer is free via Cisco Networking Academy. No prior enrolment is needed.

1. Go to **[netacad.com](https://www.netacad.com/courses/packet-tracer)**
2. Click **"Get Packet Tracer"** → Create a free Cisco NetAcad account
3. Download and install the version for your OS (Windows / macOS / Linux)

> [!IMPORTANT]
> These labs were built on **Cisco Packet Tracer 8.1.0.0722**. Use version **8.1 or newer** to guarantee all features work correctly. Older versions (e.g., 7.x) may not support some device models or IoT components used in these labs.

---

### Step 2 — Get the Repository

**Option A — Clone with Git:**
```bash
git clone https://github.com/HomemadeICT/networking-packet-tracer-labs.git
```

**Option B — Download as ZIP:**
> GitHub → **Code** → **Download ZIP** → Extract to a folder of your choice

---

### Step 3 — Open a Lab

1. Navigate to any lab folder (e.g., `03_VLAN_and_Switching/4.router-on-a-stick/`)
2. Double-click the `.pkt` file — it opens directly in Packet Tracer
3. Read the `README.md` in the same folder for the full configuration guide and verification steps

---

### 💡 Tips for Experimenting

| Tip | Detail |
|-----|--------|
| **Use Simulation Mode** | Switch from Realtime to Simulation (bottom-right clock icon) to watch packets travel hop-by-hop through the network |
| **Break and fix it** | Delete a trunk link, remove a VLAN, or change an IP — then use the verification commands in the README to diagnose and fix the problem |
| **Check the CLI** | Click any router or switch → CLI tab to run `show` commands and explore the running configuration |
| **Save your changes** | Use **File → Save As** with a new name so the original lab file stays intact |
| **Start from scratch** | Delete all configs on a device with `erase startup-config` + `reload` and reconfigure it following the README step-by-step |

---

# 🔌 Section 01 — Basic Connectivity

> **Foundation of networking** — building your first LAN with routers, switches, and PCs

---

## Lab 1.1 — One Router, 4 Switches, 8 PCs

📁 **Path**: `01_Basic_Connectivity/one-router-4-switch-8pcs/`  
📥 **Download**: [one router 4 switch 8 pcs.pkt](01_Basic_Connectivity/one-router-4-switch-8pcs/one%20router%204%20switch%208%20pcs.pkt)

### 🎯 Objective
Design and configure a basic Local Area Network (LAN) with one central router, four managed switches, and eight end-user PCs — demonstrating fundamental Ethernet connectivity, IP addressing, and switch port configuration within Cisco Packet Tracer.

### 🗺️ Network Diagram

![One Router 4 Switch 8 PCs Topology](01_Basic_Connectivity/one-router-4-switch-8pcs/screenshots/one%20router%204%20switch%208%20pcs.png)

### 🖥️ Topology Overview

| Device | Role | Count |
|--------|------|-------|
| Router (2911) | Default Gateway / Central Hub | 1 |
| Switches (2960) | Layer 2 Aggregation | 4 |
| PCs | End-User Workstations | 8 |

**Network**: `192.168.1.0/24`  
**Gateway**: `192.168.1.1`  
**PC Range**: `192.168.1.10 – 192.168.1.17`

### ⚙️ Step-by-Step Configuration

#### Step 1 — Build the Topology
1. Place **1x Router 2911** in the center of the workspace
2. Place **4x Switch 2960** around the router
3. Place **2x PCs** connected to each switch (8 PCs total)
4. Connect each Switch → Router using **straight-through** copper cables
5. Connect each PC → Switch using **straight-through** copper cables

#### Step 2 — Configure Router Interface
```bash
Router> enable
Router# configure terminal
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# description "LAN Gateway"
Router(config-if)# exit
Router(config)# exit
Router# write memory
```

#### Step 3 — Configure Each PC
For each PC, open **IP Configuration** and set:

| PC | IP Address | Subnet Mask | Default Gateway |
|----|-----------|-------------|-----------------|
| PC0 | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 |
| PC1 | 192.168.1.11 | 255.255.255.0 | 192.168.1.1 |
| PC2 | 192.168.1.12 | 255.255.255.0 | 192.168.1.1 |
| PC3 | 192.168.1.13 | 255.255.255.0 | 192.168.1.1 |
| PC4 | 192.168.1.14 | 255.255.255.0 | 192.168.1.1 |
| PC5 | 192.168.1.15 | 255.255.255.0 | 192.168.1.1 |
| PC6 | 192.168.1.16 | 255.255.255.0 | 192.168.1.1 |
| PC7 | 192.168.1.17 | 255.255.255.0 | 192.168.1.1 |

#### Step 4 — Verify Connectivity
From any PC, open the **Command Prompt** and run:
```
ping 192.168.1.1        ← Ping the gateway
ping 192.168.1.12       ← Ping another PC
```
✅ All pings should succeed with 0% packet loss.

---

# 🌐 Section 02 — Network Services

> **Server-side networking** — deploying and configuring DHCP, DNS, Web, and Mail servers

---

## Lab 2.1 — DHCP Server

📁 **Path**: `02_Network_Services/dhcp-server/`  
📥 **Download**: [DHCP SERVER.pkt](02_Network_Services/dhcp-server/DHCP%20SERVER.pkt)

### 🎯 Objective
Configure a dedicated DHCP server that automatically assigns IP addresses to client PCs — eliminating the need for manual static IP configuration.

### 🗺️ Network Diagram

![DHCP Server Topology](02_Network_Services/dhcp-server/screenshots/DHCP%20Server.png)

### 🖥️ Topology Overview

| Device | Role | IP |
|--------|------|-----|
| Router | Gateway | 192.168.1.1 |
| DHCP Server | IP Address Assignment | 192.168.1.2 (Static) |
| PCs (x4) | DHCP Clients | Auto-assigned |

### ⚙️ Step-by-Step Configuration

#### Step 1 — Configure Server Static IP
Open Server → Desktop → IP Configuration:
```
IP Address:     192.168.1.2
Subnet Mask:    255.255.255.0
Default Gateway: 192.168.1.1
```

#### Step 2 — Enable DHCP Service
Open Server → Services → **DHCP**:
```
Service:        ON
Pool Name:      serverPool
Default Gateway: 192.168.1.1
DNS Server:     192.168.1.2
Start IP:       192.168.1.10
Subnet Mask:    255.255.255.0
Maximum Users:  50
```
Click **Save** then **Add**.

#### Step 3 — Configure Client PCs
For each PC → Desktop → IP Configuration:
- Select **DHCP** (radio button)
- Wait for IP assignment (3–5 seconds)
- Verify the assigned IP appears automatically

#### Step 4 — Verify
```
ipconfig          ← Should show auto-assigned IP in 192.168.1.x range
ping 192.168.1.2  ← Ping the DHCP server
ping 192.168.1.1  ← Ping the gateway
```

---

## Lab 2.2 — DNS & Web Server

📁 **Path**: `02_Network_Services/dns-web-server/`  
📥 **Download**: [DNS and WEB Server.pkt](02_Network_Services/dns-web-server/DNS%20and%20WEB%20Server.pkt)

### 🎯 Objective
Configure a combined DNS and Web server so that PCs can browse a website using a domain name (e.g., `www.mysite.com`) instead of an IP address.

### 🗺️ Network Diagram

![DNS and Web Server Topology](02_Network_Services/dns-web-server/screenshots/DNS%20and%20WEB%20Server.png)

### 🖥️ Topology Overview

| Device | Role | IP |
|--------|------|-----|
| Router | Gateway | 192.168.1.1 |
| Server | DNS + HTTP | 192.168.1.2 (Static) |
| PCs | Web Clients | 192.168.1.10+ |

### ⚙️ Step-by-Step Configuration

#### Step 1 — Configure Server Static IP
```
IP Address:     192.168.1.2
Subnet Mask:    255.255.255.0
Default Gateway: 192.168.1.1
```

#### Step 2 — Enable HTTP Service
Server → Services → **HTTP**:
```
Service: ON
```
Optionally edit `index.html` to customize your webpage content.

#### Step 3 — Enable DNS Service
Server → Services → **DNS**:
```
Service: ON
Name:    www.mysite.com
Type:    A Record
Address: 192.168.1.2
```
Click **Add**.

#### Step 4 — Configure PC DNS
Each PC → Desktop → IP Configuration:
```
DNS Server: 192.168.1.2
```

#### Step 5 — Verify
Open PC → Desktop → **Web Browser**:
```
URL: www.mysite.com
```
✅ The Cisco Packet Tracer default webpage should load.

---

## Lab 2.3 — DNS + Web Servers With Router

📁 **Path**: `02_Network_Services/dns-web-router/`  
📥 **Download**: [DNS+ Web Servers With Router.pkt](02_Network_Services/dns-web-router/DNS%2B%20Web%20Servers%20With%20Router.pkt)

### 🎯 Objective
Extend the DNS/Web server setup across multiple network segments using a router, demonstrating inter-network service resolution and routing.

### 🗺️ Network Diagram

![DNS and Web Servers With Router](02_Network_Services/dns-web-router/screenshots/DNS%2B%20Web%20Servers%20With%20Route.png)

### 🖥️ Topology Overview

| Device | Role | Network |
|--------|------|---------|
| Router | Inter-network Routing | —  |
| Server (Network A) | DNS + HTTP | 192.168.1.0/24 |
| PCs (Network B) | Web Clients | 192.168.2.0/24 |

### ⚙️ Step-by-Step Configuration

#### Step 1 — Configure Router Interfaces
```bash
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip address 192.168.2.1 255.255.255.0
Router(config-if)# no shutdown
```

#### Step 2 — Configure Server (Network A)
```
IP:      192.168.1.2
Gateway: 192.168.1.1
DNS:     192.168.1.2
```
Enable HTTP and DNS services as described in Lab 2.2.

#### Step 3 — Configure Client PCs (Network B)
```
IP:      192.168.2.10
Gateway: 192.168.2.1
DNS:     192.168.1.2   ← Points across the router to DNS server
```

#### Step 4 — Verify Cross-Network Browsing
From Network B PC → Web Browser:
```
URL: www.mysite.com
```
✅ Page should load from the server on Network A, proving routing works.

---

## Lab 2.4 — Mail Server

📁 **Path**: `02_Network_Services/mail-server/`  
📥 **Download**: [mail practical.pkt](02_Network_Services/mail-server/mail%20practical.pkt)

### 🎯 Objective
Configure a Mail server (SMTP/POP3) and set up email clients on PCs to send and receive emails within the simulated network.

### 🗺️ Network Diagram

![Mail Server Topology](02_Network_Services/mail-server/screenshots/mail%20practical.png)

### 🖥️ Topology Overview

| Device | Role | IP |
|--------|------|-----|
| Server | Mail (SMTP + POP3) + DNS | 192.168.1.2 |
| PC0 | Email User A | 192.168.1.10 |
| PC1 | Email User B | 192.168.1.11 |

### ⚙️ Step-by-Step Configuration

#### Step 1 — Configure DNS (Required for Mail)
Server → Services → DNS:
```
Name:    mymail.com
Type:    A Record
Address: 192.168.1.2
```

#### Step 2 — Enable Mail Service
Server → Services → **Email**:
```
Domain Name: mymail.com
Service:     ON
```
Add users:
```
Username: user1    Password: pass123   → Add
Username: user2    Password: pass123   → Add
```

#### Step 3 — Configure Email on PC0
PC → Desktop → **Email**:
```
Your Name:    User One
Email:        user1@mymail.com
Incoming Mail Server (POP3): 192.168.1.2
Outgoing Mail Server (SMTP): 192.168.1.2
Username:     user1
Password:     pass123
```
Click **Save**.

#### Step 4 — Send and Receive
- PC0: Compose → To: `user2@mymail.com` → Send
- PC1: Click **Receive** to fetch the email

✅ Email should appear in PC1's inbox.

---

## Lab 2.5 — Web Project with 2 Routers

📁 **Path**: `02_Network_Services/web-project/`  
📥 **Download**: [project with web server.pkt](02_Network_Services/web-project/project%20with%20web%20server.pkt)

### 🎯 Objective
Build a multi-router network where web servers on one segment are accessible to clients across two routed networks — simulating a real-world ISP-like infrastructure.

### 🗺️ Network Diagram

![Web Project with 2 Routers](02_Network_Services/web-project/screenshots/Web%20Project%20with%202%20routers.png)

### ⚙️ Key Configuration Points

#### Static Routing Between Routers
```bash
! On Router 1
Router1(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.2

! On Router 2
Router2(config)# ip route 192.168.1.0 255.255.255.0 192.168.2.1
```

---

# 🔀 Section 03 — VLAN & Switching

> **Advanced switching** — network segmentation, traffic isolation, and inter-VLAN communication

---

## Lab 3.1 — VLAN Basic Configuration

📁 **Path**: `03_VLAN_and_Switching/2.vlan-basic/`  
📥 **Download**: [Creating and Configuring VLANs on a Managed Switch.pkt](03_VLAN_and_Switching/2.vlan-basic/Creating%20and%20Configuring%20VLANs%20on%20a%20Managed%20Switch.pkt)

### 🎯 Objective
Create and configure VLANs on a managed switch to isolate different departments or user groups onto separate logical networks, even when sharing the same physical infrastructure.

### 🗺️ Network Diagram

![VLAN Basic Configuration](03_VLAN_and_Switching/2.vlan-basic/screenshots/Creating%20and%20Configuring%20VLANs%20on%20a%20Managed%20Switch.png)

### 🖥️ VLAN Plan

| VLAN ID | Name | Purpose | Ports |
|---------|------|----------|-------|
| 10 | VLAN_ADMIN | Administration | Fa0/1 – Fa0/4 |
| 20 | VLAN_IT | IT Department | Fa0/5 – Fa0/8 |
| 30 | VLAN_SALES | Sales Team | Fa0/9 – Fa0/12 |

### ⚙️ Step-by-Step Configuration

#### Step 1 — Create VLANs on Switch
```bash
Switch> enable
Switch# configure terminal

Switch(config)# vlan 10
Switch(config-vlan)# name VLAN_ADMIN
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name VLAN_IT
Switch(config-vlan)# exit

Switch(config)# vlan 30
Switch(config-vlan)# name VLAN_SALES
Switch(config-vlan)# exit
```

#### Step 2 — Assign Ports to VLANs
```bash
! Assign ports to VLAN 10
Switch(config)# interface range FastEthernet0/1 - 4
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit

! Assign ports to VLAN 20
Switch(config)# interface range FastEthernet0/5 - 8
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit

! Assign ports to VLAN 30
Switch(config)# interface range FastEthernet0/9 - 12
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
Switch(config-if-range)# exit
```

#### Step 3 — Verify VLANs
```bash
Switch# show vlan brief
```
✅ Should show all 3 VLANs with their assigned ports.

#### Step 4 — Test Isolation
- Ping between PCs in **same VLAN** → ✅ Success
- Ping between PCs in **different VLANs** → ❌ Fails (by design — isolation works!)

---

## Lab 3.2 — Inter-VLAN Routing (Layer 3 Switch)

📁 **Path**: `03_VLAN_and_Switching/3.inter-vlan-routing/`  
📥 **Download**: [Inter Vlan Routing.pkt](03_VLAN_and_Switching/3.inter-vlan-routing/Inter%20Vlan%20Routing.pkt)

### 🎯 Objective
Enable communication between different VLANs using a Layer 3 multilayer switch with Switched Virtual Interfaces (SVIs), allowing inter-department communication while maintaining VLAN segmentation.

### 🗺️ Network Diagram

![Inter VLAN Routing](03_VLAN_and_Switching/3.inter-vlan-routing/screenshots/Inter%20Vlan%20Routing.png)

### 🖥️ VLAN & IP Plan

| VLAN | Name | SVI Gateway |
|------|------|-------------|
| 10 | ADMIN | 192.168.10.1 |
| 20 | IT | 192.168.20.1 |
| 30 | SALES | 192.168.30.1 |

### ⚙️ Step-by-Step Configuration

#### Step 1 — Enable IP Routing on Multilayer Switch
```bash
MLS(config)# ip routing
```

#### Step 2 — Create VLAN Interfaces (SVIs)
```bash
MLS(config)# interface vlan 10
MLS(config-if)# ip address 192.168.10.1 255.255.255.0
MLS(config-if)# no shutdown
MLS(config-if)# exit

MLS(config)# interface vlan 20
MLS(config-if)# ip address 192.168.20.1 255.255.255.0
MLS(config-if)# no shutdown
MLS(config-if)# exit

MLS(config)# interface vlan 30
MLS(config-if)# ip address 192.168.30.1 255.255.255.0
MLS(config-if)# no shutdown
MLS(config-if)# exit
```

#### Step 3 — Verify Inter-VLAN Routing
From a PC in VLAN 10:
```
ping 192.168.20.x   ← Ping PC in VLAN 20
ping 192.168.30.x   ← Ping PC in VLAN 30
```
✅ All pings should succeed — SVIs act as gateways for each VLAN.

---

## Lab 3.3 — Router-on-a-Stick

📁 **Path**: `03_VLAN_and_Switching/4.router-on-a-stick/`  
📥 **Download**: [Router on Stick.pkt](03_VLAN_and_Switching/4.router-on-a-stick/Router%20on%20Stick.pkt)

### 🎯 Objective
Implement inter-VLAN routing using a single router interface with 802.1Q VLAN **subinterfaces** — the "Router-on-a-Stick" technique used when a Layer 3 switch is not available.

### 🗺️ Network Diagram

![Router-on-a-Stick](03_VLAN_and_Switching/4.router-on-a-stick/screenshots/router-on-a-stick.png)

### 🖥️ Topology Overview

```
[Router]
   |
   | (Trunk Link - 802.1Q)
   |
[Switch]
 /   \
VLAN10  VLAN20
PCs    PCs
```

### ⚙️ Step-by-Step Configuration

#### Step 1 — Configure Trunk Port on Switch
```bash
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# exit
```

#### Step 2 — Create Subinterfaces on Router
```bash
Router(config)# interface GigabitEthernet0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface GigabitEthernet0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface GigabitEthernet0/0
Router(config-if)# no shutdown
```

#### Step 3 — Configure PCs
- VLAN 10 PCs → Gateway: `192.168.10.1`
- VLAN 20 PCs → Gateway: `192.168.20.1`

#### Step 4 — Verify
```
ping 192.168.20.x   ← From VLAN 10 PC to VLAN 20 PC
```
✅ Should succeed via the router subinterfaces.

---

## Lab 3.4 — Inter-VLAN Advanced Configuration

📁 **Path**: `03_VLAN_and_Switching/5.inter-vlan-advanced/`  
📥 **Download**: [Inter Vlan configuration Practical 2.pkt](03_VLAN_and_Switching/5.inter-vlan-advanced/Inter%20Vlan%20configuration%20Practical%202.pkt)

### 🎯 Objective
Advanced inter-VLAN configuration combining multiple switches, trunk links, and a multilayer switch — reflecting a real enterprise switching environment.

### 🗺️ Network Diagram

![Inter VLAN Advanced Configuration](03_VLAN_and_Switching/5.inter-vlan-advanced/screenshots/Inter%20Vlan%20configuration%20Practical.png)

### ⚙️ Key Additional Steps

#### Trunk Links Between Switches
```bash
! On each switch uplink port
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20,30
Switch(config-if)# exit
```

#### Verify Trunk Status
```bash
Switch# show interfaces trunk
```
✅ Should show allowed VLANs and trunk mode active.

---

# 🔒 Section 04 — Security & Remote Access

> **Network hardening** — securing devices and enabling remote management

---

## Lab 4.1 — Telnet Remote Access

📁 **Path**: `04_Security_and_Remote_Access/telnet/`  
📥 **Download**: [telnet practical 01.pkt](04_Security_and_Remote_Access/telnet/telnet%20practical%2001.pkt)

### 🎯 Objective
Configure Telnet access on a Cisco router, enabling remote command-line management from any PC on the network — demonstrating secure remote access fundamentals.

### 🗺️ Network Diagram

![Telnet Practical](04_Security_and_Remote_Access/telnet/screenshots/telnet%20practical.png)

### 🖥️ Topology Overview

| Device | IP | Role |
|--------|-----|------|
| Router | 192.168.1.1 | Telnet Server |
| PC | 192.168.1.10 | Telnet Client |

### ⚙️ Step-by-Step Configuration

#### Step 1 — Set Enable Password
```bash
Router(config)# enable secret cisco123
```

#### Step 2 — Configure VTY Lines for Telnet
```bash
Router(config)# line vty 0 4
Router(config-line)# password telnet123
Router(config-line)# login
Router(config-line)# transport input telnet
Router(config-line)# exit
```

#### Step 3 — Set Console Password
```bash
Router(config)# line console 0
Router(config-line)# password console123
Router(config-line)# login
Router(config-line)# exit
```

#### Step 4 — Create Hostname Banner (Optional)
```bash
Router(config)# hostname MyRouter
MyRouter(config)# banner motd # Authorized Access Only! #
```

#### Step 5 — Test Telnet from PC
PC → Desktop → **Command Prompt**:
```
telnet 192.168.1.1
```
- Enter VTY password: `telnet123`
- Enter `enable` → Password: `cisco123`

✅ You should have full remote CLI access to the router.

> ⚠️ **Security Note**: Telnet transmits data in plain text. In production environments, always use **SSH** instead of Telnet for encrypted remote access.

---

## Lab 4.2 — Port Security

📁 **Path**: `04_Security_and_Remote_Access/port-security/`  
📥 **Download**: [Port Security.pkt](04_Security_and_Remote_Access/port-security/Port%20Security.pkt)

### 🎯 Objective
Configure Port Security on a Cisco switch to restrict which MAC addresses can connect to specific ports — preventing unauthorized devices from accessing the network.

### 🗺️ Network Diagrams

**Authorized PC Connected:**
![Port Security - Authorized PC](04_Security_and_Remote_Access/port-security/screenshots/port%20security%20authorized%20pc.png)

**Unauthorized PC Attempt (Port Violation):**
![Port Security - Unauthorized PC](04_Security_and_Remote_Access/port-security/screenshots/port%20security%20unauthorized%20pc.png)

### ⚙️ Step-by-Step Configuration

#### Step 1 — Enable Port Security on a Switch Port
```bash
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
```

#### Step 2 — Set Maximum Allowed MAC Addresses
```bash
Switch(config-if)# switchport port-security maximum 1
```

#### Step 3 — Configure Sticky MAC Learning
```bash
Switch(config-if)# switchport port-security mac-address sticky
```
> This automatically learns and "sticks" the first MAC address that connects.

#### Step 4 — Set Violation Mode
```bash
Switch(config-if)# switchport port-security violation shutdown
```

| Violation Mode | Action |
|----------------|--------|
| `shutdown` | Disables port immediately (error-disabled) |
| `restrict` | Drops packets, logs violation |
| `protect` | Drops packets silently |

#### Step 5 — Verify Port Security
```bash
Switch# show port-security interface FastEthernet0/1
Switch# show port-security address
```

#### Step 6 — Recover from Violation (if port shuts down)
```bash
Switch(config)# interface FastEthernet0/1
Switch(config-if)# shutdown
Switch(config-if)# no shutdown
```

✅ **Authorized PC** → Connects and communicates normally  
❌ **Unauthorized PC** → Port immediately goes into error-disabled state

---

# 📶 Section 05 — Wireless & IoT

> **Modern connectivity** — wireless networking and smart IoT device integration

---

## Lab 5.1 — Wireless Networking

📁 **Path**: `05_Wireless/Wireless Networking/`  
📥 **Download**: [Wireless Networking.pkt](05_Wireless/Wireless%20Networking/Wireless%20Networking.pkt)

### 🎯 Objective
Configure a WRT300N wireless router to create a Wi-Fi network, and connect wireless laptops to it — demonstrating basic wireless LAN (WLAN) setup in Cisco Packet Tracer.

### 🗺️ Network Diagram

![Wireless Networking Practical](05_Wireless/Wireless%20Networking/wireles%20networking%20practical.png)

### 🖥️ Topology Overview

| Device | Role | Connection |
|--------|------|------------|
| WRT300N Router | Wireless Access Point / Router | — |
| Laptop x2 | Wireless Clients | Wi-Fi (802.11g) |

### ⚙️ Step-by-Step Configuration

#### Step 1 — Add Wireless Module to Laptops
1. Click on Laptop → Physical tab
2. **Power OFF** the laptop (click the power button)
3. Remove the default Ethernet module (drag out)
4. Drag in **Linksys-WPC300N** wireless module
5. Power the laptop back ON

#### Step 2 — Configure WRT300N Wireless Router
Click on WRT300N → GUI tab:

**Basic Wireless Settings:**
```
Network Mode:      Mixed (B/G/N)
Network Name (SSID): MyWiFi
Standard Channel:  6 - 2.437GHz
SSID Broadcast:   Enabled
```

**Wireless Security (Optional WPA2):**
```
Security Mode:    WPA2 Personal
Encryption:       AES
Passphrase:       mypassword123
```

#### Step 3 — Connect Laptops to Wi-Fi
Click Laptop → Desktop → **PC Wireless**:
1. Click **Connect** tab
2. Select your SSID: `MyWiFi`
3. If secured, enter the passphrase
4. Click **Connect**

#### Step 4 — Configure IP (DHCP)
Laptop → Desktop → IP Configuration → Select **DHCP**
> The WRT300N has a built-in DHCP server (range: 192.168.0.100–149)

#### Step 5 — Verify Connectivity
```
ping 192.168.0.1    ← Ping the wireless gateway
ping 192.168.0.101  ← Ping the other laptop
```
✅ Both laptops should communicate over wireless.

---

## Lab 5.2 — IoT Connectivity (Smart Home Network)

📁 **Path**: `05_Wireless/IOT Practical/`  
📥 **Download**: [IOT Practical.pkt](05_Wireless/IOT%20Practical/IOT%20Practical.pkt)

### 🎯 Objective
Connect IoT smart devices (fans, lights, doors) to a Home Gateway via a wireless network and control them remotely using the Cisco Packet Tracer IoT interface.

### 🗺️ Network Diagram

![IoT Practical](05_Wireless/IOT%20Practical/screenshots/IOT%20practical.png)

### 🖥️ Topology Overview

| Device | Role |
|--------|------|
| Home Gateway | IoT Controller + DHCP Server |
| Laptop/PC | IoT Management Client |
| IoT Devices | Smart Fan, LED Light, Door Lock, etc. |

### ⚙️ Step-by-Step Configuration

#### Step 1 — Place Devices
1. Home Gateway → under **Home** category in Packet Tracer
2. IoT devices (fan, light, thermostat, etc.)
3. Laptop or PC for management

#### Step 2 — Configure Home Gateway
Click Home Gateway → GUI:
```
IoT Server:     Enabled
Band:           2.4 GHz
SSID:           HomeNet
DHCP:           Enabled (192.168.25.x)
```

#### Step 3 — Connect IoT Devices to Gateway
For each IoT device:
1. Click device → Config → IoT Settings
2. Set **IoT Server**: `192.168.25.1` (or Auto-discover)
3. Connect to SSID: `HomeNet`

#### Step 4 — Control from Laptop
Laptop → Desktop → **IoT Monitor**:
- Log in with gateway credentials
- Turn devices ON/OFF remotely
- View sensor readings

✅ IoT devices should respond to remote commands from the laptop.

---

# 🏢 Section 06 — Final Project

> **Capstone projects** — applying all learned concepts in full enterprise network designs

---

## Project 6.1 — TechNova Enterprise Network Design (Capstone)

📁 **Path**: `06_Final_Project/Capstone Project/TechNova-Enterprise-Network-Design/`  
📥 **Download**: [TechNova-Enterprise-Network-Design.pkt](06_Final_Project/Capstone%20Project/TechNova-Enterprise-Network-Design/TechNova-Enterprise-Network-Design.pkt)

### 🎯 Objective
Design and implement a complete enterprise network for **TechNova**, a fictional mid-sized company — demonstrating end-to-end network design, redundancy planning, VLAN segmentation, security policies, and professional documentation.

### 🗺️ Network Diagram

![TechNova Capstone Project](06_Final_Project/Capstone%20Project/TechNova-Enterprise-Network-Design/capstone%20project.png)

### 🏛️ Enterprise Design Highlights

| Feature | Implementation |
|---------|---------------|
| **Topology** | Hierarchical (Core / Distribution / Access) |
| **VLANs** | Management, Finance, HR, IT, Sales |
| **Routing** | OSPF / Static Routes |
| **Security** | Port Security, ACLs, SSH |
| **Services** | DHCP, DNS, HTTP, Email |
| **Redundancy** | Spanning Tree Protocol (STP) |
| **Wireless** | Corporate SSID + Guest SSID |

---

## Project 6.2 — Network Lab 80 Workstations (NVQ Final Proposal)

📁 **Path**: `06_Final_Project/Network Lab 80 ( Workstations ) Projects Proposal/`

### 🎯 Objective
Design a secure, reliable, and scalable wired network for an IT educational institution with **80 workstations** across 3 specialized computer labs — submitted as the NVQ Level 5 final project proposal.

### 🗺️ Network Architecture

**High-Level Architecture:**
![Final Networking Architecture](06_Final_Project/Network%20Lab%2080%20%28%20Workstations%20%29%20Projects%20Proposal/Final%20Project%20Diagrams/Final%20Networking%20Architecture.jpg)

**Packet Tracer Implementation Diagram:**
![Packet Tracer Network Diagram](06_Final_Project/Network%20Lab%2080%20%28%20Workstations%20%29%20Projects%20Proposal/Final%20Project%20Diagrams/Networking%20Diagram%20-%20Packet%20Tracer%20Diagram.png)

### 🏗️ Network Design Specification

#### Lab Layout

| Lab | Workstations | Purpose |
|-----|-------------|---------|
| Hardware Lab | 15 PCs | Basic computing & hardware training |
| NDICT Lab | 35 PCs | Programming & database development |
| Graphics Lab | 30 PCs | Design & multimedia production |
| **Total** | **80 PCs** | — |

#### VLAN Plan

| VLAN | Name | Network | Gateway |
|------|------|---------|---------|
| 10 | HARDWARE | 192.168.10.0/24 | 192.168.10.1 |
| 20 | NDICT | 192.168.20.0/24 | 192.168.20.1 |
| 30 | GRAPHICS | 192.168.30.0/24 | 192.168.30.1 |
| 99 | MANAGEMENT | 192.168.99.0/24 | 192.168.99.1 |

#### Device List (Bill of Materials)

| Device | Model | Quantity | Purpose |
|--------|-------|----------|---------|
| Core Switch | Cisco 3560 (MLS) | 1 | Inter-VLAN Routing |
| Distribution Switch | Cisco 2960 | 3 | Lab-level Aggregation |
| Access Switch | Cisco 2960 | 6 | Workstation Connectivity |
| Router | Cisco 2911 | 1 | Internet / WAN Gateway |
| Server | Generic | 1 | DHCP + DNS + HTTP |
| Workstations | PCs | 80 | End-user Devices |
| Cabling | Cat6 UTP | — | Structured Cabling |

#### IP Addressing Plan

```
Core Switch (MLS):
  VLAN 10 SVI:   192.168.10.1/24
  VLAN 20 SVI:   192.168.20.1/24
  VLAN 30 SVI:   192.168.30.1/24
  VLAN 99 SVI:   192.168.99.1/24

Router:
  LAN Interface: 192.168.99.2/24
  WAN Interface: DHCP (from ISP)

Server:
  IP:            192.168.99.10/24
  Gateway:       192.168.99.1

DHCP Pool (per VLAN):
  VLAN 10:  192.168.10.10 – 192.168.10.30     (15 hosts)
  VLAN 20:  192.168.20.10 – 192.168.20.50     (35 hosts)
  VLAN 30:  192.168.30.10 – 192.168.30.45     (30 hosts)
```

#### Core Switch Configuration (Key Commands)
```bash
! Enable routing
MLS(config)# ip routing

! VLAN creation
MLS(config)# vlan 10
MLS(config-vlan)# name HARDWARE
MLS(config)# vlan 20
MLS(config-vlan)# name NDICT
MLS(config)# vlan 30
MLS(config-vlan)# name GRAPHICS
MLS(config)# vlan 99
MLS(config-vlan)# name MANAGEMENT

! SVI Interfaces
MLS(config)# interface vlan 10
MLS(config-if)# ip address 192.168.10.1 255.255.255.0
MLS(config-if)# no shutdown

MLS(config)# interface vlan 20
MLS(config-if)# ip address 192.168.20.1 255.255.255.0
MLS(config-if)# no shutdown

MLS(config)# interface vlan 30
MLS(config-if)# ip address 192.168.30.1 255.255.255.0
MLS(config-if)# no shutdown

MLS(config)# interface vlan 99
MLS(config-if)# ip address 192.168.99.1 255.255.255.0
MLS(config-if)# no shutdown

! Uplink to Router (routed port)
MLS(config)# interface GigabitEthernet0/1
MLS(config-if)# no switchport
MLS(config-if)# ip address 192.168.99.2 255.255.255.0
MLS(config-if)# no shutdown

! Default route to Router
MLS(config)# ip route 0.0.0.0 0.0.0.0 192.168.99.1
```

#### Security Policies Applied
```bash
! Port security on access ports
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 1
Switch(config-if)# switchport port-security mac-address sticky
Switch(config-if)# switchport port-security violation shutdown

! Disable unused ports
Switch(config)# interface range FastEthernet0/13 - 24
Switch(config-if-range)# shutdown
```

#### Verification Checklist
- [ ] `show vlan brief` — All VLANs present and ports assigned
- [ ] `show interfaces trunk` — Trunk links active
- [ ] `show ip route` — Routing table populated
- [ ] `show ip interface brief` — All SVIs up/up
- [ ] `ping 192.168.10.x` from VLAN 20 — Inter-VLAN routing works
- [ ] `show port-security` — Security policies active
- [ ] DHCP assigned to all PCs automatically
- [ ] Web browser access to server from all VLANs

---

## 📊 Project Summary

| Category | Count | Concepts Covered |
|----------|-------|-----------------|
| Basic Connectivity | 1 lab | LAN, routing, IP addressing |
| Network Services | 5 labs | DHCP, DNS, HTTP, SMTP, multi-router |
| VLAN & Switching | 4 labs | VLANs, inter-VLAN, trunk links |
| Security | 2 labs | Telnet, Port Security |
| Wireless & IoT | 2 labs | Wi-Fi, smart devices |
| Final Projects | 2 projects | Enterprise design, 80-workstation proposal |
| **TOTAL** | **16 labs/projects** | **Complete networking curriculum** |

---

## 📄 License

This project is licensed under the [MIT License](LICENSE) — see the LICENSE file for details.  
Created for educational purposes as part of the NDICT NVQ Level 5 programme.

---

## 📫 Contact

**D.M. Theekshana Prabodha**  
TheekMind *(networking enthusiast 📡)*

[![GitHub](https://img.shields.io/badge/GitHub-HomemadeICT-black?style=flat-square&logo=github)](https://github.com/HomemadeICT)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Theekshana%20Prabodha-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/theekshana-prabodha/)

---

<div align="center">

*Built with ❤️ and lots of `ping` commands* 📡

</div>
