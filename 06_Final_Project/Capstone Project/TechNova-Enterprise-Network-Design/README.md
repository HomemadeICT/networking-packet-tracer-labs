# 📡 Lab: TechNova Ltd. Enterprise Network Deployment
### *One Router · Two Switches · Eight PCs · Three Servers*

---

## 🎯 Objective

Design and deploy a **production-grade enterprise network** for TechNova Ltd. — a mid-sized IT solutions provider — featuring **VLAN-based departmental segmentation**, **Router-on-a-Stick Inter-VLAN routing**, **centralized DHCP services** via IP Helper, and **internal DNS/HTTP** corporate resources. The goal is to achieve full cross-department communication while maintaining logical separation and security.

---

## 🧠 Concepts Covered

| Concept | Description |
| :--- | :--- |
| **VLAN Segmentation** | Isolating departments (Admin, HR, IT, Server, Management) into separate broadcast domains |
| **802.1Q Trunking** | Carrying multiple VLAN-tagged frames over a single physical link between switches and the router |
| **Router-on-a-Stick (RoAS)** | Using sub-interfaces on a single router port to act as the gateway for every VLAN |
| **DHCP Relay (IP Helper)** | Forwarding DHCP broadcasts across VLAN boundaries to a centralized server in VLAN 50 |
| **DNS & HTTP Services** | Resolving `technova.com` and hosting an internal company portal |
| **Device Hardening** | Securing access with `enable secret`, console/VTY passwords, and login banners |
| **Config Persistence** | Saving running configuration to NVRAM with `copy running-config startup-config` |

---

## 🛠️ Devices Used

| Device | Model | Qty | Role |
| :--- | :--- | :---: | :--- |
| **Router** | 1841 | 1 | Central gateway — hosts sub-interfaces for Inter-VLAN routing & DHCP relay |
| **Switch** | 2960-24TT | 2 | Layer 2 distribution — provides VLAN segmentation and trunk uplinks |
| **PC** | PC-PT | 8 | End-user workstations across Admin, HR, IT, and Management departments |
| **Server** | Server-PT | 3 | Email, DHCP, and Web/DNS servers in the dedicated Server VLAN |

---

## 🗺️ High-Level Network Plan

The network follows a **hierarchical design** with a central router acting as the gateway for all departments. Two distribution switches provide Layer 2 connectivity to end devices, while a dedicated **Server VLAN (50)** hosts all critical infrastructure services.

```
                            ┌────────────────────┐
                            │     Router0        │
                            │   (1841 Gateway)    │
                            │                    │
                            │  Fa0/0: 192.168.1.1│  ← WAN / Management Link
                            │  Fa0/1: Trunk      │  ← Sub-interfaces below
                            │   ├─ .10  VLAN 10  │     192.168.10.1  (Admin)
                            │   ├─ .20  VLAN 20  │     192.168.20.1  (HR)
                            │   ├─ .30  VLAN 30  │     192.168.30.1  (IT)
                            │   ├─ .50  VLAN 50  │     192.168.50.1  (Server)
                            │   └─ .99  VLAN 99  │     192.168.99.1  (Mgmt)
                            └────────┬───────────┘
                                     │ Trunk (All VLANs)
                                     │
                    ┌────────────────┴────────────────┐
                    │                                 │
             ┌──────┴──────┐                   ┌──────┴──────┐
             │ Switch-Left │                   │ Switch-Right│
             │  (2960-24TT)│                   │  (2960-24TT)│
             │             │◄──── Trunk ──────►│             │
             │  Fa0/1 Trunk│   (Fa0/1↔Fa0/1)  │  Fa0/1 Trunk│
             │  Fa0/2 V10  │                   │  Fa0/3 V10  │ (via Gig uplink)
             │  Fa0/3 V20  │                   │  Fa0/4 V30  │
             │  Fa0/4 V30  │                   │  Fa0/5 V10  │
             │  Fa0/5 V99  │                   │  Fa0/6 V20  │
             └──────┬──────┘                   │  Fa0/7 V99  │
                    │                          │  Fa0/12-14  │ V50
                    │                          └──────┬──────┘
        ┌───────┬───┴────┬────────┐                   │
       PC6     PC7     PC8    NOC PC         ┌────────┼────────┐
     (V10)   (V20)   (V30)   (V99)         PC9     PC10    NOC PC
     Admin    HR      IT     Mgmt         (V10)   (V20)    (V99)
                                          Admin    HR      Mgmt
                                                    │
                                            ┌───────┼───────┐
                                          Email   DHCP   Web/DNS
                                         Server  Server  Server
                                          (VLAN 50 — 192.168.50.0/24)
```

### Addressing & VLAN Schema

| Department | VLAN ID | Network Subnet | Gateway IP | DHCP Scope |
| :--- | :---: | :--- | :--- | :--- |
| 🔴 **Admin** | 10 | `192.168.10.0/24` | `192.168.10.1` | `192.168.10.2` – `.254` |
| 🟣 **HR** | 20 | `192.168.20.0/24` | `192.168.20.1` | `192.168.20.2` – `.254` |
| 🟢 **IT** | 30 | `192.168.30.0/24` | `192.168.30.1` | `192.168.30.2` – `.254` |
| 🔵 **Server** | 50 | `192.168.50.0/24` | `192.168.50.1` | Static (`.10` – `.12`) |
| 🟡 **Management** | 99 | `192.168.99.0/24` | `192.168.99.1` | `192.168.99.2` – `.254` |

### Server Addressing (Static — VLAN 50)

| Server | Function | IP Address |
| :--- | :--- | :--- |
| **Email Server** | Internal email services | `192.168.50.10` |
| **DHCP Server** | Centralized IP allocation for all VLANs | `192.168.50.11` |
| **Web & DNS Server** | Hosts `technova.com` portal & DNS resolution | `192.168.50.12` |

> **Design Note:** The DHCP Server at `192.168.50.10` provides dynamic addressing for VLANs 10, 20, 30, and 99. The router's `ip helper-address` relays broadcast DHCP requests from each VLAN to this server.

---

## ⚙️ Low-Level Configuration (Device-by-Device)

### 📦 Phase I — Switch-Left (Switch0) Configuration

> **Purpose:** Switch-Left serves as the distribution switch for the left-side workstations (Admin PC6, HR PC7, IT PC8, NOC PC).

---

#### Step 1 — Basic Security & Identity

Set the hostname, enable secret, console password, VTY password, and a login banner. These commands prevent unauthorized access and clearly identify the device.

```bash
Switch> enable
Switch# configure terminal

! --- Set a descriptive hostname for identification ---
Switch(config)# hostname Switch-Left

! --- Encrypt the privileged EXEC password (stored as MD5 hash) ---
Switch-Left(config)# enable secret leftsecret

! --- Display a legal/identification banner on login ---
Switch-Left(config)# banner motd # Welcome to the Left Switch @@@@@########$$$%%^^&& #

! --- Secure the console port (physical access) ---
Switch-Left(config)# line console 0
Switch-Left(config-line)# password bcot
Switch-Left(config-line)# login
Switch-Left(config-line)# exit

! --- Secure remote Telnet access (VTY lines 0-4) ---
Switch-Left(config)# line vty 0 4
Switch-Left(config-line)# password telnetbcotleft
Switch-Left(config-line)# login
Switch-Left(config-line)# exit
```

**Why?** — `enable secret` uses MD5 hashing (unlike `enable password` which stores in plaintext). The banner warns unauthorized users. VTY passwords allow remote management via Telnet.

---

#### Step 2 — VLAN Creation

Create the VLANs that will logically segment the network traffic. Each VLAN is a separate broadcast domain.

```bash
! --- Create departmental VLANs ---
Switch-Left(config)# vlan 10
Switch-Left(config-vlan)# name Admin
Switch-Left(config-vlan)# exit

Switch-Left(config)# vlan 20
Switch-Left(config-vlan)# name HR
Switch-Left(config-vlan)# exit

Switch-Left(config)# vlan 30
Switch-Left(config-vlan)# name IT
Switch-Left(config-vlan)# exit

Switch-Left(config)# vlan 99
Switch-Left(config-vlan)# name Management
Switch-Left(config-vlan)# exit
```

**Why?** — VLANs isolate broadcast traffic. A broadcast from an Admin PC will never reach the HR or IT segments, improving both **security** and **performance**.

---

#### Step 3 — Access Port Assignments

Assign each physical port to the VLAN corresponding to the connected device.

```bash
! --- Fa0/2 → Admin PC (VLAN 10) ---
Switch-Left(config)# interface FastEthernet 0/2
Switch-Left(config-if)# switchport mode access
Switch-Left(config-if)# switchport access vlan 10
Switch-Left(config-if)# exit

! --- Fa0/3 → HR PC (VLAN 20) ---
Switch-Left(config)# interface FastEthernet 0/3
Switch-Left(config-if)# switchport mode access
Switch-Left(config-if)# switchport access vlan 20
Switch-Left(config-if)# exit

! --- Fa0/4 → IT PC (VLAN 30) ---
Switch-Left(config)# interface FastEthernet 0/4
Switch-Left(config-if)# switchport mode access
Switch-Left(config-if)# switchport access vlan 30
Switch-Left(config-if)# exit

! --- Fa0/5 → NOC / Management PC (VLAN 99) ---
Switch-Left(config)# interface FastEthernet 0/5
Switch-Left(config-if)# switchport mode access
Switch-Left(config-if)# switchport access vlan 99
Switch-Left(config-if)# exit
```

**Why?** — `switchport mode access` tells the port it will carry traffic for only one VLAN. `switchport access vlan X` assigns it. Without this, all ports remain in the default VLAN 1.

---

#### Step 4 — Trunk Configuration

Configure the uplink port Fa0/1 as a trunk to carry all VLAN traffic to the router and the other switch.

```bash
! --- Fa0/1 → Trunk to Router0 / Switch-Right ---
Switch-Left(config)# interface FastEthernet 0/1
Switch-Left(config-if)# switchport mode trunk
Switch-Left(config-if)# switchport trunk allowed vlan 10,20,30,99
Switch-Left(config-if)# no shutdown
Switch-Left(config-if)# exit
```

**Why?** — A trunk port tags each frame with an 802.1Q VLAN ID so the receiving device (router or other switch) knows which VLAN the frame belongs to. The `allowed vlan` command restricts which VLANs can traverse this link.

---

#### Step 5 — Save Configuration

```bash
Switch-Left# copy running-config startup-config
```

---

### 📦 Phase II — Switch-Right (Switch1) Configuration

> **Purpose:** Switch-Right serves the right-side workstations and — critically — the three servers in VLAN 50.

---

#### Step 1 — Basic Security & Identity

```bash
Switch> enable
Switch# configure terminal

Switch(config)# hostname Switch-Right
Switch-Right(config)# enable secret rightsecret
Switch-Right(config)# banner motd # This is the Right Switch &&&*^%*&^%*&^%*%%$* #

Switch-Right(config)# line console 0
Switch-Right(config-line)# password bcot2
Switch-Right(config-line)# login
Switch-Right(config-line)# exit

Switch-Right(config)# line vty 0 4
Switch-Right(config-line)# password telnetcotright
Switch-Right(config-line)# login
Switch-Right(config-line)# exit
```

---

#### Step 2 — VLAN Creation (Including VLAN 50 — Server)

```bash
Switch-Right(config)# vlan 10
Switch-Right(config-vlan)# name Admin
Switch-Right(config-vlan)# exit

Switch-Right(config)# vlan 20
Switch-Right(config-vlan)# name HR
Switch-Right(config-vlan)# exit

Switch-Right(config)# vlan 30
Switch-Right(config-vlan)# name IT
Switch-Right(config-vlan)# exit

Switch-Right(config)# vlan 50
Switch-Right(config-vlan)# name Server
Switch-Right(config-vlan)# exit

Switch-Right(config)# vlan 99
Switch-Right(config-vlan)# name Management
Switch-Right(config-vlan)# exit
```

**Why?** — VLAN 50 is exclusive to Switch-Right because the physical servers are directly connected here. Creating it on this switch allows the ports to be assigned to this VLAN.

---

#### Step 3 — Access Port Assignments

```bash
! --- Fa0/5 → Admin PC (VLAN 10) ---
Switch-Right(config)# interface FastEthernet 0/5
Switch-Right(config-if)# switchport mode access
Switch-Right(config-if)# switchport access vlan 10
Switch-Right(config-if)# exit

! --- Fa0/6 → HR PC (VLAN 20) ---
Switch-Right(config)# interface FastEthernet 0/6
Switch-Right(config-if)# switchport mode access
Switch-Right(config-if)# switchport access vlan 20
Switch-Right(config-if)# exit

! --- Fa0/4 → IT PC (VLAN 30) ---
Switch-Right(config)# interface FastEthernet 0/4
Switch-Right(config-if)# switchport mode access
Switch-Right(config-if)# switchport access vlan 30
Switch-Right(config-if)# exit

! --- Fa0/2 → Management / NOC PC (VLAN 99) ---
Switch-Right(config)# interface FastEthernet 0/2
Switch-Right(config-if)# switchport mode access
Switch-Right(config-if)# switchport access vlan 99
Switch-Right(config-if)# exit

! --- Fa0/12–14 → Servers (VLAN 50) ---
! Using interface range to configure multiple ports at once
Switch-Right(config)# interface range FastEthernet 0/12 - 14
Switch-Right(config-if-range)# switchport mode access
Switch-Right(config-if-range)# switchport access vlan 50
Switch-Right(config-if-range)# exit
```

**Why?** — The `interface range` command is an efficiency tool. Instead of configuring Fa0/12, Fa0/13, and Fa0/14 individually, you apply the same VLAN assignment in a single command block.

---

#### Step 4 — Trunk Configuration (with VLAN 50 Added)

```bash
! --- Fa0/1 → Trunk to Router0 / Switch-Left ---
Switch-Right(config)# interface FastEthernet 0/1
Switch-Right(config-if)# switchport mode trunk
Switch-Right(config-if)# switchport trunk allowed vlan 10,20,30,50,99
Switch-Right(config-if)# no shutdown
Switch-Right(config-if)# exit
```

> **Important:** If you initially configured the trunk with only VLANs `10,20,30,99`, you must add VLAN 50 afterwards:
> ```bash
> Switch-Right(config-if)# switchport trunk allowed vlan add 50
> ```
> This ensures server traffic can flow across the trunk to the router without wiping the existing allowed list.

---

#### Step 5 — Save Configuration

```bash
Switch-Right# copy running-config startup-config
```

---

### 🌐 Phase III — Router0 (Router-on-a-Stick) Configuration

> **Purpose:** Router0 is the brain of the network. It uses **sub-interfaces** on a single physical port (Fa0/1) to act as the default gateway for every VLAN. It also relays DHCP requests to the centralized server.

---

#### Step 1 — Sub-Interface Creation (Inter-VLAN Routing)

Each sub-interface is a virtual interface bound to a specific VLAN using `encapsulation dot1Q`. The IP address assigned becomes the **default gateway** for all devices in that VLAN.

```bash
Router> enable
Router# configure terminal

! ====================================
! Sub-interface for VLAN 10 (Admin)
! ====================================
Router(config)# interface FastEthernet 0/1.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# ip helper-address 192.168.50.10
Router(config-subif)# exit

! ====================================
! Sub-interface for VLAN 20 (HR)
! ====================================
Router(config)# interface FastEthernet 0/1.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# ip helper-address 192.168.50.10
Router(config-subif)# exit

! ====================================
! Sub-interface for VLAN 30 (IT)
! ====================================
Router(config)# interface FastEthernet 0/1.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.30.1 255.255.255.0
Router(config-subif)# ip helper-address 192.168.50.10
Router(config-subif)# exit

! ====================================
! Sub-interface for VLAN 50 (Server) — NO helper needed
! ====================================
Router(config)# interface FastEthernet 0/1.50
Router(config-subif)# encapsulation dot1Q 50
Router(config-subif)# ip address 192.168.50.1 255.255.255.0
! No ip helper-address here — the DHCP server is LOCAL to this VLAN
Router(config-subif)# exit

! ====================================
! Sub-interface for VLAN 99 (Management)
! ====================================
Router(config)# interface FastEthernet 0/1.99
Router(config-subif)# encapsulation dot1Q 99
Router(config-subif)# ip address 192.168.99.1 255.255.255.0
Router(config-subif)# ip helper-address 192.168.50.10
Router(config-subif)# exit
```

**Why `encapsulation dot1Q`?** — This command tells the sub-interface which VLAN tag to listen for. When a frame tagged with VLAN 10 arrives on Fa0/1, the router knows to process it on the `.10` sub-interface.

**Why `ip helper-address`?** — DHCP uses broadcasts (`255.255.255.255`). Broadcasts are confined to their VLAN. Without a helper, a PC in VLAN 10 can never reach the DHCP server in VLAN 50. The router intercepts the broadcast and unicasts it to `192.168.50.10`.

---

#### Step 2 — Activate Physical Interfaces

Sub-interfaces inherit the state of their parent. You must bring the physical interface up.

```bash
! --- Activate the trunk-facing interface ---
Router(config)# interface FastEthernet 0/1
Router(config-if)# no shutdown
Router(config-if)# exit

! --- Configure the WAN / Management interface ---
Router(config)# interface FastEthernet 0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# exit
```

**Why?** — Cisco router interfaces are `administratively down` by default. `no shutdown` enables them. Fa0/0 is configured as a separate management or WAN-facing link.

---

#### Step 3 — Save Configuration

```bash
Router# copy running-config startup-config
```

---

### 🖥️ Phase IV — Server Configuration (Application Layer)

> **Purpose:** Configure the three servers in VLAN 50 to provide DHCP, DNS, and HTTP services to the entire network.

---

#### DHCP Server (`192.168.50.10`)

Configure DHCP pools on the server for each department VLAN:

| Pool Name | Network | Start IP | Default Gateway | DNS Server |
| :--- | :--- | :--- | :--- | :--- |
| VLAN10_Admin | `192.168.10.0` | `192.168.10.2` | `192.168.10.1` | `192.168.50.12` |
| VLAN20_HR | `192.168.20.0` | `192.168.20.2` | `192.168.20.1` | `192.168.50.12` |
| VLAN30_IT | `192.168.30.0` | `192.168.30.2` | `192.168.30.1` | `192.168.50.12` |
| VLAN99_Mgmt | `192.168.99.0` | `192.168.99.2` | `192.168.99.1` | `192.168.50.12` |

> **Why separate pools?** — Each VLAN has its own subnet. The DHCP server needs a unique pool per subnet so it assigns IPs in the correct range with the correct gateway.

---

#### Web & DNS Server (`192.168.50.12`)

| Service | Setting | Value |
| :--- | :--- | :--- |
| **HTTP** | Status | `ON` — Serves the TechNova company portal |
| **DNS** | Status | `ON` |
| **DNS Record** | A Record | `technova.com` → `192.168.50.12` |

> **Result:** Any PC in any VLAN can type `technova.com` in the browser and reach the internal web server, as long as their DNS server setting points to `192.168.50.12`.

---

## ✅ Verification Checklist

- [x] **VLAN Database** — Run `show vlan brief` on both switches; confirm VLANs 10, 20, 30, 50, 99 exist with correct names
- [x] **Trunk Status** — Run `show interfaces trunk`; confirm Fa0/1 is trunking with all required VLANs listed
- [x] **Router Sub-Interfaces** — Run `show ip interface brief`; all sub-interfaces show `up/up`
- [x] **DHCP Success** — PCs show IPs in their correct range (e.g., `192.168.20.x` for HR)
- [x] **Intra-VLAN Ping** — Admin PC6 → Admin PC9 (same VLAN, different switches) ✅
- [x] **Inter-VLAN Ping** — Admin PC (VLAN 10) → IT PC (VLAN 30) via Router ✅
- [x] **Server Access** — PC (VLAN 10) → Server (VLAN 50) ping successful ✅
- [x] **DNS Resolution** — `nslookup technova.com` returns `192.168.50.12` ✅
- [x] **Web Access** — Browser on any PC loads `technova.com` ✅
- [x] **Routing Table** — `show ip route` on Router0 shows all five networks as `C` (Connected) ✅
- [x] **Config Saved** — `copy running-config startup-config` confirmed on all devices ✅

### Useful Verification Commands

```bash
! --- On Switches ---
Switch# show vlan brief                    ! View VLAN-to-port mappings
Switch# show interfaces trunk              ! Verify trunk status & allowed VLANs
Switch# show running-config                ! Full config audit

! --- On Router ---
Router# show ip interface brief            ! All interfaces at a glance
Router# show ip route                      ! Routing table — should show 5 Connected networks
Router# show running-config | section Fa0/1! View sub-interface configs

! --- On PCs (Packet Tracer Command Prompt) ---
> ipconfig                                 ! Verify DHCP-assigned IP & gateway
> ping 192.168.50.10                       ! Test reachability to DHCP server
> nslookup technova.com                    ! Test DNS resolution
```

---

## 📋 IOS Command Log — Key Observations

> The following patterns were observed directly from the Packet Tracer IOS Command Logs captured during this lab session (33 screenshots).

| Observation | Detail |
| :--- | :--- |
| **Sub-interface on switch (error)** | Initially attempted `interface fa0/1.10` on Switch-Left. Layer 2 switches do not support sub-interfaces — this is a Layer 3 feature. The logic was correctly moved to Router0. |
| **VLAN 50 trunk omission** | The initial trunk config on Switch-Right only allowed VLANs `10,20,30,99`. VLAN 50 was added later using `switchport trunk allowed vlan add 50`. |
| **DHCP relay missing** | PCs initially could not receive IPs. Fixed by adding `ip helper-address 192.168.50.10` to every sub-interface (except Fa0/1.50). |
| **Config saved multiple times** | `copy running-config startup-config` / `write memory` executed after each phase to prevent data loss. |
| **Show commands used** | `show vlan brief`, `show interfaces trunk`, `show ip route`, and `show ip interface brief` were used throughout for real-time verification. |

---

## ❗ Common Issues & Fixes

| Issue | Symptom | Fix |
| :--- | :--- | :--- |
| **Sub-interface on a L2 switch** | `% Invalid input detected at '^' marker` | Sub-interfaces belong on the **router**, not on Layer 2 switches |
| **Encapsulation mismatch** | Sub-interface won't process traffic | Always run `encapsulation dot1Q [VLAN_ID]` **before** assigning the IP address |
| **Missing `ip helper-address`** | PCs in remote VLANs cannot get DHCP IPs | Add `ip helper-address 192.168.50.10` to each sub-interface (except the server's own VLAN) |
| **VLAN not in trunk allowed list** | Devices in that VLAN have no connectivity across switches | Run `switchport trunk allowed vlan add [VLAN_ID]` on the trunk port |
| **Forgot `no shutdown`** | Router interface shows `administratively down` (red dot in PT) | Enter the **physical** parent interface and run `no shutdown` |
| **Wrong default gateway on PC** | PC can reach same-VLAN devices but fails cross-VLAN pings | Verify gateway matches the router sub-interface IP for that VLAN |
| **Config lost after reload** | All VLANs, IPs, and trunks disappear | Always run `copy running-config startup-config` before exiting |

---

## 📁 Lab File

> 💾 **Try it yourself** — Download [`TechNova-Enterprise-Network-Design.pkt`](TechNova-Enterprise-Network-Design.pkt) and open in **Cisco Packet Tracer 8.1+**. Use Simulation Mode to watch a DHCP Discover packet travel from a PC through the router's `ip helper-address` relay all the way to the DHCP server — then back again.

- `TechNova-Enterprise-Network-Design.pkt` — Full enterprise network with VLAN segmentation, Router-on-a-Stick, DHCP relay, and DNS/HTTP services.
- `TechNova-Enterprise-Network-Design.pdf` — Formal project proposal document.

---

## 🚀 What I Learned

- **VLAN segmentation is the foundation of enterprise security** — isolating departments prevents unnecessary broadcast traffic and limits the blast radius of network issues.
- **Router-on-a-Stick is elegant but has limits** — a single physical link carries all inter-VLAN traffic, making it a potential bottleneck in larger deployments. Layer 3 switches solve this.
- **`ip helper-address` is the unsung hero** — without it, centralized DHCP is impossible because broadcast requests die at the VLAN boundary.
- **Trunking discipline matters** — forgetting to add a VLAN to the allowed list silently drops all traffic for that VLAN on the trunk link.
- **Configuration persistence is non-negotiable** — learned to always `write memory` after every significant change, not just at the end.
- **Troubleshooting is part of the process** — initially configuring sub-interfaces on a switch (instead of the router) was an error, but recognizing and correcting it demonstrated professional-level diagnostic skills.

---

## ❗ Troubleshooting & Lessons Learned

### The "Invalid Command" Incident

While configuring Switch-Left, an attempt was made to create sub-interfaces directly on the switch:

```bash
Switch-Left(config)# interface fa0/1.10
                                     ^
% Invalid input detected at '^' marker.
```

**Root Cause:** Standard Layer 2 switches (2960 series) do not support sub-interfaces or IP routing. Sub-interfaces are a **Layer 3** feature that belongs on the router.

**Resolution:** The sub-interface logic was successfully migrated to Router0 using the Router-on-a-Stick method.

### DHCP Connectivity Failure

Initially, PCs in Admin, HR, and IT VLANs could not receive DHCP-assigned IP addresses.

**Root Cause:** DHCP uses broadcasts (`DHCPDISCOVER`), and broadcasts do not cross VLAN boundaries.

**Fix:** Applied `ip helper-address 192.168.50.10` to every router sub-interface (except Fa0/1.50, where the DHCP server resides locally).

```
Before: PC (VLAN 10) --broadcast--> [BLOCKED at VLAN boundary]
After:  PC (VLAN 10) --broadcast--> Router (relay) --unicast--> DHCP Server (VLAN 50)
```

---

## 📸 Screenshots

### Network Topology

![Network Topology](capstone%20project.png)
*TechNova Ltd. full network topology in Cisco Packet Tracer*

---

## 🎯 Project Completion Summary

| Phase | Focus | Status |
| :--- | :--- | :---: |
| **I. Infrastructure** | Switch/Router Identity & Security | ✅ Complete |
| **II. Segmentation** | VLAN Creation & Trunking | ✅ Complete |
| **III. Routing** | Router-on-a-Stick & DHCP Relay | ✅ Complete |
| **IV. Services** | DHCP, DNS, HTTP Configuration | ✅ Complete |
| **V. Verification** | Cross-VLAN Pings, Web Access, Persistence | ✅ Complete |
