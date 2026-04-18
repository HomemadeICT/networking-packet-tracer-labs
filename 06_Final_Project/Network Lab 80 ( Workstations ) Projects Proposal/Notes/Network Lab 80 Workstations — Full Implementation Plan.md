# Network Lab 80 Workstations — Full Implementation Plan
### Cisco Packet Tracer | Hierarchical Campus Network

---

## 📊 Current Progress Status

| Configuration Area | Status | Notes |
|---|---|---|
| Core Switch — VLANs (10/20/30/40) | ✅ Done | Verified in IOS log lines 8–21 |
| Core Switch — SVIs (gateways) | ✅ Done | Lines 25–40, all `no shutdown` applied |
| Core Switch — DHCP helper-address | ✅ Done | Lines 54–65, set on all 4 SVIs |
| Core Switch — Trunk Fa0/24 → Hardware | ✅ Done | Lines 108–113, allowed vlan 10,40 |
| Core Switch — Trunk Gi0/1 → NDICT | ✅ Done | Lines 115–120, allowed vlan 20,40 |
| Core Switch — Trunk Gi0/2 → Graphics | ✅ Done | Lines 122–127, allowed vlan 30,40 |
| Core Switch — Staff access ports Fa0/1–10 | ✅ Done | Lines 129–133, VLAN 40 |
| Core Switch — Fa0/2 (Router port) | ✅ Done | Lines 170–173, access VLAN 40 |
| Core Switch — Default Route | ✅ Done | Line 143, `0.0.0.0/0 via 192.168.40.254` |
| Core Switch — `ip routing` | ✅ Done | Line 176 (confirmed re-enabled) |
| Router0 — Gi0/0 → Core Switch | ✅ Done | Line 140, IP 192.168.40.254 |
| Router0 — Return static routes | ✅ Done | Lines 182–184, all 3 lab subnets |
| Router0 — Gi0/1 WAN interface | ❌ Not Done | No ISP/WAN IP assigned |
| Router0 — NAT/PAT | ❌ Not Done | Required for internet simulation |
| DHCP Server (192.168.40.5) | ❌ Not Done | Pools not configured |
| Switch2 (Hardware Lab) — uplink + access | ❌ Not Done | |
| Switch3 (NDICT Lab primary) — all config | ❌ Not Done | |
| Switch4 (NDICT Lab secondary) — all config | ❌ Not Done | Initial `enable` only |
| Switch5 (Graphics Lab primary) — all config | ❌ Not Done | |
| Switch6 (Graphics Lab secondary) — all config | ❌ Not Done | |
| ACLs (VLAN isolation) | ❌ Not Done | |
| SSH on all devices | ❌ Not Done | |
| Port Security on all access switches | ❌ Not Done | |
| End device DHCP verification | ❌ Not Done | |

---

## 🏗️ Part 1 — HIGH LEVEL DESIGN (HLD)

### 1.1 Network Architecture

```
                        [ INTERNET / ISP ]
                               │
                       ┌───────────────┐
                       │   Router0     │  Cisco 2911
                       │  (Edge/WAN)   │  Gi0/0 = 192.168.40.254 (LAN)
                       │               │  Gi0/1 = 203.0.113.1/30 (WAN)
                       └───────┬───────┘
                               │ Access port — VLAN 40
                               │ (Fa0/2 on Core Switch)
                       ┌───────────────┐
                       │  Core-Switch  │  Cisco 3560 Multilayer
                       │  (Layer 3)    │  ip routing enabled
                       │               │  SVIs: .10.1 / .20.1 / .30.1 / .40.1
                       │   + Server    │  DHCP Server: 192.168.40.5
                       └──┬────┬────┬──┘
                          │    │    │
              ┌───────────┘    │    └────────────┐
              │ Fa0/24         │ Gi0/1           │ Gi0/2
              │ Trunk          │ Trunk           │ Trunk
              │ VLAN10,40      │ VLAN20,40       │ VLAN30,40
              ▼                ▼                 ▼
        ┌──────────┐    ┌──────────┐      ┌──────────┐
        │ Switch2  │    │ Switch3  │      │ Switch5  │
        │ Hardware │    │  NDICT   │      │ Graphics │
        │   Lab    │    │   Lab    │      │   Lab    │
        └────┬─────┘    └────┬─────┘      └────┬─────┘
             │  Trunk        │  Trunk           │  Trunk
             │  VLAN10,40    │  VLAN20,40       │  VLAN30,40
             ▼               ▼                  ▼
        (No 2nd SW)     ┌──────────┐      ┌──────────┐
                        │ Switch4  │      │ Switch6  │
                        │  NDICT   │      │ Graphics │
                        │ (2nd)    │      │  (2nd)   │
                        └──────────┘      └──────────┘
```

### 1.2 VLAN Strategy

| VLAN | Name | Purpose | Subnet | Gateway | DHCP Pool Range |
|---|---|---|---|---|---|
| **10** | Hardware_Lab | 15 PCs + 1 Laptop (16 devices) | `192.168.10.0/24` | `192.168.10.1` | `.100 – .200` |
| **20** | NDICT_Lab | 35 PCs + 1 Laptop + 1 Printer (37 devices) | `192.168.20.0/24` | `192.168.20.1` | `.100 – .200` |
| **30** | Graphics_Lab | 30 PCs + 1 Laptop + 1 Printer (32 devices) | `192.168.30.0/24` | `192.168.30.1` | `.100 – .200` |
| **40** | Staff | 4 Laptops + 1 Printer + DNS/DHCP/WEB Server | `192.168.40.0/24` | `192.168.40.1` | `.20 – .50` |

### 1.3 Device Inventory (from Packet Tracer diagram)

| Device | Model | Location | Role |
|---|---|---|---|
| Router0 | Cisco 2911 | Server Rack (Staff Room area) | Edge/WAN/NAT |
| Core-Switch | Cisco 3560 Multilayer | Server Rack | Core L3 Routing, DHCP relay |
| Switch2 | Cisco 2960-24TT | Hardware Lab | Access switch (16 devices) |
| Switch3 | Cisco 2960-24TT | NDICT Lab | Access switch primary (21 devices) |
| Switch4 | Cisco 2960-24TT | NDICT Lab | Access switch secondary (16 devices) |
| Switch5 | Cisco 2960-24TT | Graphics Lab | Access switch primary (16 devices) |
| Switch6 | Cisco 2960-24TT | Graphics Lab | Access switch secondary (16 devices) |
| Server | Server-PT | Staff Room | DNS + DHCP + WEB (IP: 192.168.40.5) |
| PC1–PC15 | PC-PT | Hardware Lab | VLAN 10 |
| Laptop4 | Laptop-PT | Hardware Lab | VLAN 10 |
| PC16–PC50 | PC-PT | NDICT Lab | VLAN 20 |
| Laptop5 | Laptop-PT | NDICT Lab | VLAN 20 |
| Printer1 | Printer-PT | NDICT Lab | VLAN 20 |
| PC51–PC80 | PC-PT | Graphics Lab | VLAN 30 |
| Laptop6 | Laptop-PT | Graphics Lab | VLAN 30 |
| Printer0 | Printer-PT | Graphics Lab | VLAN 30 |
| Laptop0–3 | Laptop-PT | Staff Room | VLAN 40 |
| Printer3 | Printer-PT | Staff Room | VLAN 40 |

### 1.4 Security Policy (HLD)

| Rule | From | To | Action |
|---|---|---|---|
| 1 | Hardware Lab (VLAN 10) | NDICT Lab (VLAN 20) | **DENY** |
| 2 | Hardware Lab (VLAN 10) | Graphics Lab (VLAN 30) | **DENY** |
| 3 | NDICT Lab (VLAN 20) | Hardware Lab (VLAN 10) | **DENY** |
| 4 | NDICT Lab (VLAN 20) | Graphics Lab (VLAN 30) | **DENY** |
| 5 | Graphics Lab (VLAN 30) | Hardware Lab (VLAN 10) | **DENY** |
| 6 | Graphics Lab (VLAN 30) | NDICT Lab (VLAN 20) | **DENY** |
| 7 | Any Lab | Staff/Internet | **PERMIT** |
| 8 | Staff (VLAN 40) | Any | **PERMIT** |

---

## ⚙️ Part 2 — LOW LEVEL DESIGN (LLD) — Interface/Device Level Configs

### Configuration Order (CRITICAL — follow this sequence)

```
Step 1  →  DHCP Server (192.168.40.5) — configure pools FIRST
Step 2  →  Core Switch — verify ip routing + DHCP relay
Step 3  →  Switch2 (Hardware Lab)
Step 4  →  Switch3 (NDICT Primary)
Step 5  →  Switch4 (NDICT Secondary)
Step 6  →  Switch5 (Graphics Primary)
Step 7  →  Switch6 (Graphics Secondary)
Step 8  →  End Devices — set to DHCP, verify IP assignment
Step 9  →  Router0 — WAN + NAT
Step 10 →  ACLs (LAST — test connectivity BEFORE applying)
Step 11 →  SSH on all devices
Step 12 →  Port Security on all access switches
Step 13 →  Final verification
```

---

### STEP 1 — DHCP Server Configuration (192.168.40.5)

> **Why first?** The Core Switch's `ip helper-address` is already pointing to this server.
> Once you connect access switches and PCs, they will immediately request IPs. The server must be ready.

**In Packet Tracer — Click the Server → Services → DHCP:**

| Pool Name | Default Gateway | DNS Server | Start IP | Max Users | Subnet Mask |
|---|---|---|---|---|---|
| `VLAN10_POOL` | 192.168.10.1 | 192.168.40.5 | 192.168.10.100 | 100 | 255.255.255.0 |
| `VLAN20_POOL` | 192.168.20.1 | 192.168.40.5 | 192.168.20.100 | 100 | 255.255.255.0 |
| `VLAN30_POOL` | 192.168.30.1 | 192.168.40.5 | 192.168.30.100 | 100 | 255.255.255.0 |
| `VLAN40_POOL` | 192.168.40.1 | 192.168.40.5 | 192.168.40.20 | 30 | 255.255.255.0 |

**Also configure the Server's own static IP (Desktop → IP Configuration):**
```
IP Address:      192.168.40.5
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.40.1
DNS Server:      192.168.40.5 (self)
```

> ⚠️ **Pitfall:** Make sure "Service: ON" is toggled in the DHCP tab. Packet Tracer has the service defaulting to OFF.

---

### STEP 2 — Core Switch Verification (Already Configured — Verify Only)

Run these commands to confirm current state before proceeding:

```
en
show vlan brief
show ip interface brief
show ip route
show interfaces trunk
show running-config
```

**Expected `show ip route` output:**
```
C    192.168.10.0/24 is directly connected, Vlan10
C    192.168.20.0/24 is directly connected, Vlan20
C    192.168.30.0/24 is directly connected, Vlan30
C    192.168.40.0/24 is directly connected, Vlan40
S*   0.0.0.0/0 [1/0] via 192.168.40.254
```

**Expected `show interfaces trunk` output:**
```
Port      Mode     Encapsulation  Status    Native VLAN
Fa0/24    on       802.1q         trunking  1
Gi0/1     on       802.1q         trunking  1
Gi0/2     on       802.1q         trunking  1
```

> ⚠️ **Pitfall:** If any SVI shows `down/down`, it means no live port in that VLAN exists yet. This is EXPECTED at this stage — SVIs will come up once you connect access switches with active PCs in the next steps.

---

### STEP 3 — Switch2 (Hardware Lab — VLAN 10)

**Device:** Cisco 2960-24TT  
**Connection:** Gi0/1 → Core Switch Fa0/24 (trunk)  
**Devices served:** PC1–PC15 (Fa0/1–Fa0/15), Laptop4 (Fa0/16)

```
en
conf t
hostname HW-Lab-SW

! Step 3a: Create VLANs locally (REQUIRED even though switches learn from trunk)
vlan 10
 name Hardware_Lab
vlan 40
 name Staff
exit

! Step 3b: Configure the UPLINK to Core Switch as TRUNK
interface GigabitEthernet0/1
 description UPLINK_TO_CORE_SWITCH_Fa0/24
 switchport mode trunk
 switchport trunk allowed vlan 10,40
 no shutdown
exit

! Step 3c: Configure PC ports as ACCESS for VLAN 10
interface range FastEthernet0/1 - 16
 description ACCESS_VLAN10_DEVICES
 switchport mode access
 switchport access vlan 10
 no shutdown
exit

! Step 3d: Shutdown ALL unused ports (security)
interface range FastEthernet0/17 - 24
 description UNUSED_SHUTDOWN
 shutdown
exit

! Step 3e: Save
do write memory
```

> ⚠️ **Pitfall:** On a 2960, you do NOT need `switchport trunk encapsulation dot1q` — it only supports 802.1Q by default. If you type this command, IOS will error. This command is ONLY needed on 3560 multilayer switches.

> ⚠️ **Pitfall:** After configuring the trunk, verify with `show interfaces trunk`. If Gi0/1 doesn't appear, check that the Core Switch's Fa0/24 is already set to trunk (it should be from Step 1 baseline).

---

### STEP 4 — Switch3 (NDICT Lab Primary — VLAN 20)

**Device:** Cisco 2960-24TT  
**Uplink:** Gi0/1 → Core Switch Gi0/1 (trunk)  
**Downlink:** Gi0/2 → Switch4 (inter-switch trunk)  
**Devices served:** PC16–PC33 (Fa0/1–Fa0/18), Laptop5 (Fa0/19), Printer1 (Fa0/20)  
*(remaining PCs on Switch4)*

```
en
conf t
hostname NDICT-SW1

! Step 4a: Create VLANs
vlan 20
 name NDICT_Lab
vlan 40
 name Staff
exit

! Step 4b: UPLINK to Core Switch (trunk)
interface GigabitEthernet0/1
 description UPLINK_TO_CORE_SWITCH_Gi0/1
 switchport mode trunk
 switchport trunk allowed vlan 20,40
 no shutdown
exit

! Step 4c: DOWNLINK to Switch4 (trunk — carries same VLANs)
interface GigabitEthernet0/2
 description DOWNLINK_TO_NDICT_SW2
 switchport mode trunk
 switchport trunk allowed vlan 20,40
 no shutdown
exit

! Step 4d: Access ports for PCs, Laptop, Printer
interface range FastEthernet0/1 - 20
 description ACCESS_VLAN20_DEVICES
 switchport mode access
 switchport access vlan 20
 no shutdown
exit

! Step 4e: Shutdown unused ports
interface range FastEthernet0/21 - 24
 description UNUSED_SHUTDOWN
 shutdown
exit

do write memory
```

> ⚠️ **Pitfall:** The trunk between Switch3 and Switch4 must allow VLAN 20 AND 40. If you only allow VLAN 20, the DHCP relay (which uses VLAN 40 to reach the server) won't work properly for devices on Switch4.

> ⚠️ **Important — Inter-switch trunk port:** Gi0/2 going to Switch4 must also be configured on Switch4's side (Step 5). Until both ends are trunks, the link won't carry tagged traffic.

---

### STEP 5 — Switch4 (NDICT Lab Secondary — VLAN 20)

**Device:** Cisco 2960-24TT  
**Uplink:** Gi0/1 → Switch3 Gi0/2 (trunk)  
**Devices served:** PC34–PC50 (Fa0/1–Fa0/17)  
*(17 PCs — remaining NDICT devices)*

```
en
conf t
hostname NDICT-SW2

! VLANs
vlan 20
 name NDICT_Lab
vlan 40
 name Staff
exit

! UPLINK to Switch3 (trunk)
interface GigabitEthernet0/1
 description UPLINK_TO_NDICT_SW1_Gi0/2
 switchport mode trunk
 switchport trunk allowed vlan 20,40
 no shutdown
exit

! Access ports for PCs
interface range FastEthernet0/1 - 17
 description ACCESS_VLAN20_DEVICES
 switchport mode access
 switchport access vlan 20
 no shutdown
exit

! Shutdown unused ports
interface range FastEthernet0/18 - 24
 description UNUSED_SHUTDOWN
 shutdown
exit

do write memory
```

---

### STEP 6 — Switch5 (Graphics Lab Primary — VLAN 30)

**Device:** Cisco 2960-24TT  
**Uplink:** Gi0/1 → Core Switch Gi0/2 (trunk)  
**Downlink:** Gi0/2 → Switch6 (inter-switch trunk)  
**Devices served:** PC51–PC65 (Fa0/1–Fa0/15), Laptop6 (Fa0/16)  
*(remaining PCs on Switch6)*

```
en
conf t
hostname GFX-SW1

! VLANs
vlan 30
 name Graphics_Lab
vlan 40
 name Staff
exit

! UPLINK to Core Switch (trunk)
interface GigabitEthernet0/1
 description UPLINK_TO_CORE_SWITCH_Gi0/2
 switchport mode trunk
 switchport trunk allowed vlan 30,40
 no shutdown
exit

! DOWNLINK to Switch6 (trunk)
interface GigabitEthernet0/2
 description DOWNLINK_TO_GFX_SW2
 switchport mode trunk
 switchport trunk allowed vlan 30,40
 no shutdown
exit

! Access ports
interface range FastEthernet0/1 - 16
 description ACCESS_VLAN30_DEVICES
 switchport mode access
 switchport access vlan 30
 no shutdown
exit

! Shutdown unused
interface range FastEthernet0/17 - 24
 description UNUSED_SHUTDOWN
 shutdown
exit

do write memory
```

---

### STEP 7 — Switch6 (Graphics Lab Secondary — VLAN 30)

**Device:** Cisco 2960-24TT  
**Uplink:** Gi0/1 → Switch5 Gi0/2 (trunk)  
**Devices served:** PC66–PC80 (Fa0/1–Fa0/15), Printer0 (Fa0/16)  
*(15 PCs + 1 Printer)*

```
en
conf t
hostname GFX-SW2

! VLANs
vlan 30
 name Graphics_Lab
vlan 40
 name Staff
exit

! UPLINK to Switch5 (trunk)
interface GigabitEthernet0/1
 description UPLINK_TO_GFX_SW1_Gi0/2
 switchport mode trunk
 switchport trunk allowed vlan 30,40
 no shutdown
exit

! Access ports
interface range FastEthernet0/1 - 16
 description ACCESS_VLAN30_DEVICES
 switchport mode access
 switchport access vlan 30
 no shutdown
exit

! Shutdown unused
interface range FastEthernet0/17 - 24
 description UNUSED_SHUTDOWN
 shutdown
exit

do write memory
```

---

### STEP 8 — End Device Configuration

**All PCs / Laptops / Printers → set to DHCP:**

On each device in Packet Tracer:
```
Desktop tab → IP Configuration → Select "DHCP"
Wait 10–15 seconds
Verify received IP is in the correct range
```

**Expected IP assignments:**
| Device | VLAN | Expected IP Range |
|---|---|---|
| PC1–PC15, Laptop4 | 10 | `192.168.10.100–200` |
| PC16–PC50, Laptop5, Printer1 | 20 | `192.168.20.100–200` |
| PC51–PC80, Laptop6, Printer0 | 30 | `192.168.30.100–200` |
| Laptop0–3, Printer3 | 40 | `192.168.40.20–50` |

> ⚠️ **Pitfall:** If a PC gets `0.0.0.0` or `169.254.x.x` instead of its VLAN subnet:
> 1. Check the DHCP server pool is ON
> 2. Check the correct `ip helper-address 192.168.40.5` on the SVI
> 3. Ping from Core Switch SVI to `192.168.40.5` — must succeed
> 4. Check that trunk ports are up with `show interfaces trunk`

**Connectivity test BEFORE ACLs (important — confirm routing works first):**
```
From PC1 (VLAN 10):  ping 192.168.10.1   → must succeed (same-VLAN gateway)
From PC1 (VLAN 10):  ping 192.168.20.1   → must succeed (inter-VLAN, no ACL yet)
From PC1 (VLAN 10):  ping 192.168.40.254 → must succeed (router)
```

---

### STEP 9 — Router0 — WAN Interface + NAT/PAT

**Current state:** Gi0/0 = 192.168.40.254 (LAN side, ✅ done)  
**Remaining:** Gi0/1 (WAN) + NAT

```
en
conf t
hostname Router0

! Configure WAN interface (simulated ISP link)
interface GigabitEthernet0/1
 description WAN_TO_ISP
 ip address 203.0.113.1 255.255.255.252
 ip nat outside
 no shutdown
exit

! Mark LAN interface as NAT inside
interface GigabitEthernet0/0
 ip nat inside
exit

! Create ACL to match all internal private networks
ip access-list standard NAT_INTERNAL
 permit 192.168.10.0 0.0.0.255
 permit 192.168.20.0 0.0.0.255
 permit 192.168.30.0 0.0.0.255
 permit 192.168.40.0 0.0.0.255
exit

! PAT (NAT Overload) — all internal IPs share the WAN IP
ip nat inside source list NAT_INTERNAL interface GigabitEthernet0/1 overload

! Default route to simulated ISP (if not already set)
ip route 0.0.0.0 0.0.0.0 203.0.113.2

do write memory
```

> ⚠️ **Pitfall — Packet Tracer ISP simulation:** In Packet Tracer, add a "Cloud" device or a second router as the ISP. Connect it to Router0 Gi0/1 and give it a static IP `203.0.113.2/30` with a loopback representing the internet.

> ⚠️ **Pitfall:** The `ip nat inside source list` command references an ACL. If you use a numbered ACL (`access-list 1`), make sure the ACL number matches. Named ACLs (as shown above) are cleaner and clearer.

---

### STEP 10 — ACLs (VLAN Isolation — Apply LAST)

> **Why last?** Applying ACLs before verifying baseline connectivity means you'll spend hours troubleshooting what is actually a configuration bug, not a security policy issue. Always test first, secure second.

**Strategy:** Apply ACLs **inbound** on each lab's SVI (Core Switch). This stops inter-lab traffic at the routing stage.

```
en
conf t

! ============================================
! ACL for VLAN 10 (Hardware Lab) — inbound
! ============================================
ip access-list extended VLAN10_IN
 remark Allow Hardware Lab to reach Staff/Server/Internet
 permit ip 192.168.10.0 0.0.0.255 192.168.40.0 0.0.0.255
 remark Allow return traffic (ESTABLISHED connections — Packet Tracer limitation: use permit any any for DNS/web)
 permit ip 192.168.10.0 0.0.0.255 host 192.168.40.5
 remark Block Hardware Lab to NDICT Lab
 deny   ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
 remark Block Hardware Lab to Graphics Lab
 deny   ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255
 remark Allow everything else (internet via NAT, same-VLAN traffic)
 permit ip any any
exit

interface vlan 10
 ip access-group VLAN10_IN in
exit

! ============================================
! ACL for VLAN 20 (NDICT Lab) — inbound
! ============================================
ip access-list extended VLAN20_IN
 remark Allow NDICT Lab to reach Staff/Internet
 permit ip 192.168.20.0 0.0.0.255 192.168.40.0 0.0.0.255
 remark Block NDICT Lab to Hardware Lab
 deny   ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
 remark Block NDICT Lab to Graphics Lab
 deny   ip 192.168.20.0 0.0.0.255 192.168.30.0 0.0.0.255
 remark Allow everything else
 permit ip any any
exit

interface vlan 20
 ip access-group VLAN20_IN in
exit

! ============================================
! ACL for VLAN 30 (Graphics Lab) — inbound
! ============================================
ip access-list extended VLAN30_IN
 remark Allow Graphics Lab to reach Staff/Internet
 permit ip 192.168.30.0 0.0.0.255 192.168.40.0 0.0.0.255
 remark Block Graphics Lab to Hardware Lab
 deny   ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
 remark Block Graphics Lab to NDICT Lab
 deny   ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
 remark Allow everything else
 permit ip any any
exit

interface vlan 30
 ip access-group VLAN30_IN in
exit

! VLAN 40 (Staff) — no restrictions, allow all
! No ACL needed on VLAN 40

do write memory
```

> ⚠️ **Critical ACL Pitfall — ACL Processing Order:** Cisco IOS runs ACL rules **top to bottom**, stopping at the FIRST match. The structure above uses "permit specific targets first, then deny the rest." If you put `deny any any` first by mistake, ALL traffic dies.

> ⚠️ **Pitfall — DHCP breaks with ACL:** DHCP discovery uses broadcast (`255.255.255.255`). The `permit ip any any` at the end of each ACL ensures DHCP requests are not blocked. Do NOT add an explicit `deny ip any any` at the end — the implicit deny is fine.

> ⚠️ **Pitfall — ACL direction matters:** Applying `ip access-group VLAN10_IN in` on `interface vlan 10` means it checks traffic COMING FROM VLAN 10 into the routing process. This is correct. If you apply it `out`, it filters traffic GOING TO VLAN 10 (which can block the target lab instead of the source).

---

### STEP 11 — SSH on All Devices

Apply this to Core-Switch, all 5 Access Switches, and Router0. Adjust `hostname` per device.

```
! On Core-Switch (repeat on each switch and router, changing hostname)
en
conf t

! SSH requires a domain name and RSA key
ip domain-name lab.local
crypto key generate rsa
! When prompted: enter 1024

! Create local admin user
username admin privilege 15 secret Admin@Lab123

! Disable Telnet, enable SSH only
line vty 0 4
 transport input ssh
 login local
 exec-timeout 10 0
exit

! Optional: Set SSH version 2 (more secure)
ip ssh version 2

do write memory
```

> ⚠️ **Pitfall — Packet Tracer SSH limitation:** When prompted for key size, enter `1024`. Packet Tracer does not support 2048-bit keys in older versions. If `crypto key generate rsa` gives no prompt, type `crypto key generate rsa modulus 1024`.

> ⚠️ **Pitfall:** `login local` requires a username to be created first. If you configure `login local` before `username admin ...`, you'll lock yourself out of VTY lines.

---

### STEP 12 — Port Security (All Access Switches)

Apply **after** all devices are connected and have been verified working.

> **Why after verification?** Sticky MAC learning locks in the MAC address of the FIRST device that connects. If you apply it before connecting the intended PC, it learns the wrong MAC and shuts down when the real PC connects.

**Apply on ALL access switches (HW-Lab-SW, NDICT-SW1, NDICT-SW2, GFX-SW1, GFX-SW2):**

```
en
conf t

! Hardware Lab example — VLAN 10 PC ports Fa0/1 - Fa0/16
interface range FastEthernet0/1 - 16
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation shutdown
exit

do write memory
```

**Verification:**
```
show port-security
show port-security interface FastEthernet0/1
```

> ⚠️ **Pitfall — Port security only works on ACCESS ports.** If you try to apply `switchport port-security` on a trunk port (like Gi0/1), IOS will reject it. Always apply only to end-device access ports.

> ⚠️ **Pitfall — Errdisabled recovery:** If a wrong device connects and shuts the port down, you'll see `err-disabled` in `show ip interface brief`. To re-enable:
> ```
> interface FastEthernet0/X
>  shutdown
>  no shutdown
> ```
> The port must be manually re-enabled — this is the "violation shutdown" behaviour working as designed.

---

## ✅ Part 3 — VERIFICATION CHECKLIST

### Phase 1 — Infrastructure Verification (After Steps 1–7)

| Test | Command | Pass Result |
|---|---|---|
| VLANs on Core | `show vlan brief` | VLANs 10/20/30/40 active |
| SVIs on Core | `show ip interface brief` | All Vlan10–40 show `up/up` |
| Routing table | `show ip route` | 4× `C` routes + 1× `S*` default |
| Trunks on Core | `show interfaces trunk` | Fa0/24, Gi0/1, Gi0/2 trunking |
| Trunks on SW2 | `show interfaces trunk` | Gi0/1 trunking — allowed 10,40 |
| Trunks on SW3 | `show interfaces trunk` | Gi0/1 + Gi0/2 trunking |
| Trunks on SW4 | `show interfaces trunk` | Gi0/1 trunking |
| Trunks on SW5 | `show interfaces trunk` | Gi0/1 + Gi0/2 trunking |
| Trunks on SW6 | `show interfaces trunk` | Gi0/1 trunking |

### Phase 2 — IP Connectivity Verification (After Step 8, Before Step 10)

| Test | From | To | Expected |
|---|---|---|---|
| Same-VLAN ping | PC1 (VLAN 10) | PC2 (VLAN 10) | ✅ 100% success |
| Gateway ping | PC1 | 192.168.10.1 | ✅ 80–100% (first ARP miss is normal) |
| Inter-VLAN ping | PC1 (VLAN 10) | PC16 (VLAN 20) | ✅ Success (ACLs not yet applied) |
| Staff ping | PC1 | 192.168.40.5 (Server) | ✅ Success |
| Router ping | Core Switch | 192.168.40.254 | ✅ 80–100% |
| DHCP Verify | Any PC | `ipconfig` / Desktop | IP in correct VLAN range |

### Phase 3 — ACL Verification (After Step 10)

| Test | From | To | Expected After ACL |
|---|---|---|---|
| Lab isolation | PC1 (VLAN 10) | PC16 (VLAN 20) | ❌ Blocked (DENY) |
| Lab isolation | PC1 (VLAN 10) | PC51 (VLAN 30) | ❌ Blocked (DENY) |
| Staff access | PC1 (VLAN 10) | 192.168.40.5 | ✅ Permitted |
| Internet sim. | PC1 | Router WAN IP | ✅ Permitted via NAT |
| Staff to all | Laptop0 (VLAN 40) | PC1 / PC16 / PC51 | ✅ Permitted |

### Phase 4 — Management Verification (After Steps 11–12)

| Test | Command | Expected |
|---|---|---|
| SSH from PC to Core | `ssh -l admin 192.168.40.1` (Desktop → Terminal) | Successful login |
| SSH from PC to Switch | `ssh -l admin 192.168.10.2` (switch mgmt IP) | Successful login |
| Telnet blocked | Attempt Telnet to any switch | Connection refused |
| Port security | `show port-security` | All access ports show max: 1, sticky MAC |

---

## 🚨 Potential Pitfalls Summary

| # | Pitfall | Prevention |
|---|---|---|
| 1 | Forgetting `ip routing` on Core Switch | Always `show ip route` first — must show C routes |
| 2 | DHCP server OFF | Toggle ON in Services tab before connecting PCs |
| 3 | Using `#` comments in IOS CLI | Never. Paste clean commands only |
| 4 | `encapsulation dot1q` on 2960 switch | 2960 is Layer 2 only — this cmd doesn't exist there |
| 5 | Trunk allows wrong VLANs | Always use `switchport trunk allowed vlan X,40` — include VLAN 40 so management traffic works |
| 6 | ACL direction wrong (`out` instead of `in`) | Apply ACLs `in` on the source VLAN SVI |
| 7 | Port security on trunk port | Only on access ports — IOS rejects it on trunks |
| 8 | SSH before domain-name / RSA key | Follow order: hostname → domain-name → crypto key → username → vty config |
| 9 | ACL breaking DHCP | Keep `permit ip any any` at end of each ACL — never end with explicit `deny any any` |
| 10 | SVI stays `down/down` | Need at least 1 active device in that VLAN — connect a PC first |
| 11 | Missing return routes on Router | Must have static routes back to all 3 lab subnets via 192.168.40.1 (✅ already done) |
| 12 | Wrong port for Router connection | Router Gi0/0 → Core Switch Fa0/2 must be access port VLAN 40, NOT trunk |
| 13 | 80% ping on first ping | Normal — ARP resolution. Only panic at 0% or sustained failure |
| 14 | Sticky MAC learning wrong device | Apply port security AFTER correct devices are connected and working |

---

## 📁 File Reference Map

| File | Location | Purpose |
|---|---|---|
| Core Switch config | `Configuration Step 01/` | Verified — Step 1 complete |
| Access Switch template | This plan — Steps 3–7 | New — to be applied |
| DHCP pools | This plan — Step 1 | New — to be applied |
| ACL configs | This plan — Step 10 | New — apply last |
| SSH configs | This plan — Step 11 | New |
| Packet Tracer file | `Project Proposal Topology.pkt` | Working file |
| Network diagrams | `Final Project Diagrams/` | Reference |
| Wiring plan | Physical diagram image | Reference for real-world layout |

---

## 🎯 VIVA Talking Points

When the examiner asks **"Explain your design decision":**

- **"I chose a Layer 3 Core Switch over Router-on-a-Stick because it performs inter-VLAN routing in hardware ASICs, not software — this removes the single-link bottleneck and scales to 80+ devices."**
- **"I used VLAN pruning on all trunk ports — for example, the Hardware Lab trunk only carries VLANs 10 and 40, not 20 or 30. This prevents unnecessary broadcast flooding."**
- **"I applied ACLs inbound on the source SVI rather than outbound on the destination — this stops traffic before it enters the routing table, which is more efficient."**
- **"Port security with sticky MAC means the switch learns and locks the first MAC address, preventing rogue devices even if someone physically swaps a cable."**
- **"The DHCP helper-address on each SVI means a single dedicated server handles all four VLANs — centralized, easy to manage."**
