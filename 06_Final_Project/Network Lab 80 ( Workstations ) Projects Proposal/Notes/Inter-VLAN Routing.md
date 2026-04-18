# Router-on-a-Stick (RoaS) — Inter-VLAN Routing

## 1. Core Concept

**Router-on-a-Stick** is a technique where a single physical router interface carries traffic for multiple VLANs by splitting that interface into logical **sub-interfaces**. Each sub-interface is assigned to one VLAN and acts as that VLAN's default gateway.

> In short: one cable, many networks.

---

## 2. Why Does This Exist? — The Real-World Problem

In any organization — a school, a hospital, a corporate office — different departments need network isolation for **security** and **performance**:

| Department | Why Isolate? |
|---|---|
| Finance | Sensitive payroll and banking data must not leak to general staff. |
| Guest Wi-Fi | Visitors should have internet access but zero access to internal servers. |
| IP Cameras / CCTV | Surveillance traffic must be separated so it doesn't choke employee bandwidth. |
| VoIP Phones | Voice traffic needs priority; mixing it with downloads causes choppy calls. |

VLANs create this isolation at Layer 2 (the switch level). But isolated VLANs **cannot talk to each other by default** — that's the whole point. When they *do* need to communicate (e.g., Finance printing to a shared printer on the Server VLAN), you need a **Layer 3 device** (a router) to route between them.

**Router-on-a-Stick** solves this with minimal hardware: one router, one cable, one switch.

---

## 3. When to Use It vs. When Not To

| Scenario                                | Use RoaS?              | Better Alternative                         |
| --------------------------------------- | ---------------------- | ------------------------------------------ |
| Small office, 2–5 VLANs, light traffic  | ✅ Yes                  | —                                          |
| Home lab / Cisco Packet Tracer practice | ✅ Yes                  | —                                          |
| Medium office, 10+ VLANs, heavy traffic | ⚠️ Possible bottleneck | Layer 3 Switch (SVI routing)               |
| Data center, hundreds of VLANs          | ❌ No                   | Layer 3 switches, dedicated routing fabric |

> **Key limitation:** All inter-VLAN traffic funnels through a single physical link (the "stick"), which becomes a bandwidth bottleneck under heavy load.

---

## 4. Devices Required

| Device | Example Model | Role |
|---|---|---|
| **Router** | Cisco 2911, 4321 | Performs Layer 3 routing between VLANs via sub-interfaces. |
| **Switch** | Cisco 2960 | Connects all end devices; manages VLAN assignments; trunks uplink to router. |
| **End Devices** | PCs, Printers, IP Phones | At least two, each placed in a different VLAN to test inter-VLAN communication. |

### Cabling

- **Switch ↔ PCs:** Regular straight-through Ethernet (access ports).
- **Switch ↔ Router:** Single Ethernet cable configured as a **trunk** (carries tagged VLAN traffic).

---

## 5. Key Protocols & Standards

### IEEE 802.1Q (dot1Q)

This is the **industry-standard trunking protocol**. When a frame leaves an access port and enters the trunk, the switch inserts a **4-byte VLAN tag** into the Ethernet header. This tag tells every device on the trunk which VLAN the frame belongs to.

- **Tag structure:** TPID (Tag Protocol Identifier) + PCP (Priority) + DEI + **VLAN ID (12 bits → up to 4094 VLANs)**
- **Native VLAN:** Frames on the native VLAN are sent **untagged** over the trunk. Default is VLAN 1.

> Think of dot1Q tagging like putting a colored sticker on each envelope before dropping it into a shared mailbox. The router reads the sticker color to know which department the letter belongs to.

---

## 6. Configuration Breakdown

### 6.1 Switch Configuration

The switch has two jobs: **assign ports to VLANs** and **trunk the uplink to the router**.

#### Step 1 — Create VLANs

```
Switch(config)# vlan 10
Switch(config-vlan)# name SALES
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name ENGINEERING
Switch(config-vlan)# exit
```

| Command | Full Form | Purpose |
|---|---|---|
| `vlan 10` | Virtual Local Area Network 10 | Creates a new broadcast domain. Without this, the switch won't recognize VLAN 10 tagged traffic. |
| `name SALES` | VLAN Name | Labels the VLAN for human readability. Not functionally required but essential in production networks. |

#### Step 2 — Assign Access Ports to VLANs

```
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10

Switch(config)# interface fa0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
```

| Command | Full Form | Purpose |
|---|---|---|
| `switchport mode access` | Switchport Mode: Access | Sets the port to carry **only one VLAN**. Used for end devices (PCs, printers). |
| `switchport access vlan 10` | Switchport Access VLAN 10 | Binds this physical port to VLAN 10. Any device plugged in here is automatically in VLAN 10. |

#### Step 3 — Configure the Trunk Port (to the Router)

```
Switch(config)# interface g0/1
Switch(config-if)# switchport mode trunk
```

| Command | Full Form | Purpose |
|---|---|---|
| `interface g0/1` | Interface GigabitEthernet 0/1 | Selects the physical port connected to the router. |
| `switchport mode trunk` | Switchport Mode: Trunk | **Critical.** Converts the link to a trunk, allowing it to carry **multiple VLANs** simultaneously by tagging each frame with its VLAN ID. |

---

### 6.2 Router Configuration

The router does **not** use its physical interface directly. Instead, we create **sub-interfaces** — one per VLAN.

#### Step 1 — Bring Up the Physical Interface

```
Router(config)# interface g0/0
Router(config-if)# no shutdown
```

| Command | Full Form | Purpose |
|---|---|---|
| `no shutdown` | No Shutdown | Powers on the physical interface. Sub-interfaces won't work if the parent interface is administratively down. |

> **Important:** Do NOT assign an IP address to the physical interface itself. Only sub-interfaces get IPs.

#### Step 2 — Create Sub-Interfaces

```
Router(config)# interface g0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0

Router(config)# interface g0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
```

| Command | Full Form | Purpose |
|---|---|---|
| `interface g0/0.10` | Interface Gig0/0, Sub-interface 10 | Creates a virtual interface. The `.10` suffix is a label — best practice is to **match it to the VLAN number** for clarity. |
| `encapsulation dot1Q 10` | Encapsulation IEEE 802.1Q, VLAN 10 | Tells the router: "Read 802.1Q tags on incoming frames, and only process frames tagged as VLAN 10 on this sub-interface." |
| `ip address 192.168.10.1 255.255.255.0` | IP Address Assignment | This becomes the **default gateway** for all PCs in VLAN 10. Each VLAN gets its own subnet. |

#### Step 3 — Configure End Devices

On each PC, set the default gateway to the router's sub-interface IP for that VLAN:

| PC | VLAN | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| PC-A | 10 (Sales) | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC-B | 20 (Engineering) | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |

---

## 7. How Traffic Flows — Step by Step

Let's trace what happens when **PC-A (VLAN 10, 192.168.10.10)** pings **PC-B (VLAN 20, 192.168.20.10)**:

```
PC-A ──► Switch ──► Router ──► Switch ──► PC-B
         (tag 10)   (route)    (tag 20)
```

1. **PC-A** creates an ICMP echo request destined for `192.168.20.10`. Since the destination is on a **different subnet**, PC-A sends the frame to its **default gateway** (`192.168.10.1`).

2. The frame arrives at the **Switch** on an access port (VLAN 10). The switch internally tags it as **VLAN 10**.

3. The switch forwards the tagged frame out the **trunk port** (g0/1) toward the router, with an 802.1Q tag of **VLAN 10**.

4. The **Router** receives the frame on **sub-interface g0/0.10** (because the dot1Q encapsulation matches VLAN 10). It strips the VLAN tag and reads the IP header.

5. The router's routing table shows `192.168.20.0/24` is reachable via **sub-interface g0/0.20**. It forwards the packet out g0/0.20, which re-tags the frame as **VLAN 20**.

6. The tagged frame returns to the **Switch** via the trunk. The switch reads the VLAN 20 tag and delivers the frame out the appropriate **access port** to **PC-B**.

7. **PC-B** receives the ping and replies. The return path follows the same process in reverse.

---

## 8. Verification & Troubleshooting

### Useful Commands

| Command | Device | What It Shows |
|---|---|---|
| `show vlan brief` | Switch | Lists all VLANs and their assigned ports. Verify your ports are in the correct VLAN. |
| `show interfaces trunk` | Switch | Shows which ports are trunking and which VLANs are allowed. |
| `show ip interface brief` | Router | Displays all interfaces/sub-interfaces and their IP + status. Look for `up/up`. |
| `show ip route` | Router | Confirms connected routes for each VLAN subnet exist. |
| `ping <destination IP>` | PC | End-to-end connectivity test. |

### Common Mistakes

| Symptom | Likely Cause |
|---|---|
| PCs in the same VLAN can't ping each other | Access port not assigned to the correct VLAN (`show vlan brief`). |
| PCs in different VLANs can't ping each other | Trunk not configured, encapsulation VLAN ID mismatch, or `no shutdown` missing on the physical interface. |
| Sub-interface shows `up/down` | Parent physical interface is shut down. Run `no shutdown` on `g0/0`. |
| Trunk shows but VLANs are missing | VLAN not created on the switch, or VLAN pruned from the trunk. |

---

## 9. Real-World Application Examples

### Small Business Office

A 20-person company with Sales, Admin, and Guest Wi-Fi:
- **VLAN 10 — Sales:** Desktops accessing CRM and internal file shares.
- **VLAN 20 — Admin:** Accounting machines with access to banking portals.
- **VLAN 30 — Guest:** Isolated internet-only access for visitors.
- **Router-on-a-Stick** handles inter-VLAN routing cheaply with a single Cisco 2911.

### Retail Store

- **VLAN 10 — POS Terminals:** Point-of-sale machines processing card transactions (PCI-DSS compliance requires isolation).
- **VLAN 20 — Back Office:** Manager workstations.
- **VLAN 30 — Customer Wi-Fi:** Open network, completely firewalled from POS.

### Home Lab / CCNA Study

- Perfect for Cisco Packet Tracer practice: low device count, easy to set up, and covers **VLAN, trunking, sub-interface,** and **routing** concepts in one lab.

---

## 10. Summary — Key Takeaways

| Concept | One-Liner |
|---|---|
| **VLAN** | Logical broadcast domain — isolates traffic at Layer 2. |
| **Trunk** | A link that carries multiple VLANs using 802.1Q tags. |
| **Sub-interface** | A virtual slice of a physical router port, tied to one VLAN. |
| **dot1Q Encapsulation** | The protocol that stamps and reads VLAN tags on trunk links. |
| **Default Gateway** | The router sub-interface IP that end devices use to reach other VLANs. |
| **RoaS Limitation** | All inter-VLAN traffic shares one physical link — potential bottleneck at scale. |