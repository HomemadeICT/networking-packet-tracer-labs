# 📡 Lab: VLAN Trunking Protocol (VTP)

## 🎯 Objective

This lab demonstrates the configuration and verification of **VLAN Trunking Protocol (VTP)** on a multi-switch network using Cisco 2950 switches. The goal is to configure a single **VTP Server** switch and have it automatically propagate VLAN definitions to **VTP Client** switches across the network — eliminating the need to manually create VLANs on every switch individually.

---

## 🧠 Concepts Covered

- **VTP Modes** — Understanding the three operating modes: Server, Client, and Transparent, and when to use each.
- **VTP Domain** — A shared administrative boundary that groups all participating switches. A switch only accepts VTP advertisements from switches in the same domain.
- **VTP Revision Number** — A counter that tracks changes to the VLAN database. Switches always adopt the configuration with the **highest** revision number — a critical source of network outages if misunderstood.
- **VLAN Propagation** — How a Server's VLAN database is automatically distributed to all Client switches over trunk links.
- **802.1Q Trunking as a Prerequisite** — VTP advertisements travel only over trunk links. Without a trunk, VTP updates never reach the next switch.

---

## 🛠️ Devices Used

| Device | Model | Role |
| :--- | :--- | :--- |
| **Switch0** | Cisco Catalyst 2950-24 | VTP Server — source of VLAN truth |
| **Switch1** | Cisco Catalyst 2950-24 | VTP Client — receives VLAN database from Server |
| **Switch2** | Cisco Catalyst 2950-24 | VTP Client — receives VLAN database from Server |

> VTP advertisements travel across **802.1Q trunk links** between the switches. The two Client switches only need VLAN configuration applied once — on the Server — and VTP does the rest.

---

## ⚙️ Configuration Steps

### 📐 Step 1 — High-Level Network Plan (HLD)

The strategy is simple: define all VLANs **once** on the VTP Server. The Server advertises those VLANs over trunk links to every Client switch in the same VTP domain. No manual VLAN creation is needed on the Clients.

#### 🗺️ VTP Design Plan

| Switch | VTP Mode | VTP Domain | VTP Password | Role |
| :--- | :--- | :--- | :--- | :--- |
| Switch0 | `server` | `BCOT` | `cisco` | Authoritative source of VLAN database |
| Switch1 | `client` | `BCOT` | `cisco` | Receives & enforces VLANs from Server |
| Switch2 | `client` | `BCOT` | `cisco` | Receives & enforces VLANs from Server |

#### 🗺️ VLAN Plan (Defined on Server, propagated to Clients)

| VLAN ID | Name | Purpose |
| :---: | :--- | :--- |
| `10` | HR | Human Resources Department |
| `20` | IT | Information Technology Department |
| `30` | SALES | Sales Department |

#### 🖼️ Topology Diagram

![Network Topology](screenshots/VTP%20Practical%20Lab.png)

> All three switches are connected in a linear chain via trunk links: **Switch0 ↔ Switch1 ↔ Switch2**. VTP advertisements flow from the Server (Switch0) through Switch1 and on to Switch2.

---

### 🔧 Step 2 — Low-Level Configuration (LLD)

---

#### 🖥️ Switch0 — VTP Server Configuration

The Server is the only switch where VLANs are created. All other switches receive these definitions automatically.

##### 2A. Enter Global Configuration Mode

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch0
Switch0(config)#
```

##### 2B. Configure VTP Server Mode

```
Switch0(config)# vtp mode server
Switch0(config)# vtp domain BCOT
Switch0(config)# vtp password cisco
```

| Command | Explanation |
| :--- | :--- |
| `vtp mode server` | Sets this switch as the VTP Server — the only switch allowed to create, modify, or delete VLANs |
| `vtp domain BCOT` | Defines the VTP domain name. All switches sharing this name will exchange VTP advertisements |
| `vtp password cisco` | Secures VTP advertisements with a password — only switches with the matching password accept updates |

> ✅ After this step, Switch0 is the **source of truth** for the VLAN database. Any changes made here will propagate to all Client switches.

##### 2C. Create the VLAN Database (On Server Only)

```
Switch0(config)# vlan 10
Switch0(config-vlan)# name HR
Switch0(config-vlan)# exit

Switch0(config)# vlan 20
Switch0(config-vlan)# name IT
Switch0(config-vlan)# exit

Switch0(config)# vlan 30
Switch0(config-vlan)# name SALES
Switch0(config-vlan)# exit
```

> 💡 This is the **only place** you need to define VLANs. VTP will synchronise these definitions to Switch1 and Switch2 automatically — as long as trunk links are active and both switches are in the same VTP domain.

##### 2D. Configure Trunk Links on Switch0

VTP advertisements only travel over trunk ports. Configure the uplink port toward Switch1 as a trunk.

```
Switch0(config)# interface FastEthernet0/1
Switch0(config-if)# switchport mode trunk
Switch0(config-if)# exit
Switch0(config)# end
Switch0# write memory
```

---

#### 🖥️ Switch1 — VTP Client Configuration

##### 2E. Set VTP Client Mode

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch1

Switch1(config)# vtp mode client
Switch1(config)# vtp domain BCOT
Switch1(config)# vtp password cisco
```

| Command | Explanation |
| :--- | :--- |
| `vtp mode client` | Sets this switch as a VTP Client — it **cannot** create or delete VLANs locally. It only accepts VTP advertisements from the Server |
| `vtp domain BCOT` | Must match the Server's domain name exactly (case-sensitive) |
| `vtp password cisco` | Must match the Server's VTP password exactly |

> ⚠️ **Critical:** A switch in `client` mode will reject any manual `vlan X` commands. Any attempt to create a VLAN locally returns: `VTP VLAN configuration not allowed when device is in CLIENT mode.` This is by design.

##### 2F. Configure Trunk Links on Switch1

Switch1 needs trunk ports on both sides — toward Switch0 (Server) to receive updates, and toward Switch2 to pass them along.

```
Switch1(config)# interface FastEthernet0/1
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# exit

Switch1(config)# interface FastEthernet0/2
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# exit

Switch1(config)# end
Switch1# write memory
```

---

#### 🖥️ Switch2 — VTP Client Configuration

##### 2G. Set VTP Client Mode

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch2

Switch2(config)# vtp mode client
Switch2(config)# vtp domain BCOT
Switch2(config)# vtp password cisco
```

##### 2H. Configure Trunk Link on Switch2

Switch2 connects back toward Switch1 only (it is the end of the chain).

```
Switch2(config)# interface FastEthernet0/1
Switch2(config-if)# switchport mode trunk
Switch2(config-if)# exit

Switch2(config)# end
Switch2# write memory
```

> ✅ At this point, all three switches are configured. VTP advertisements begin flowing from Switch0 → Switch1 → Switch2. Within seconds, all Client switches will have VLANs 10 (HR), 20 (IT), and 30 (SALES) in their VLAN database — without any manual configuration on the Clients.

---

## ✅ Verification

### Step 1 — Verify VTP Status on All Switches

Run `show vtp status` on each switch to confirm VTP mode, domain, and revision number.

```
Switch0# show vtp status
```

#### Expected Output (Server):

```
VTP Version                     : 2
Configuration Revision          : 3
Maximum VLANs supported locally : 255
Number of existing VLANs        : 8
VTP Operating Mode              : Server
VTP Domain Name                 : BCOT
VTP Pruning Mode                : Disabled
VTP V2 Mode                     : Disabled
VTP Traps Generation            : Disabled
MD5 digest                      : ...
```

> ✅ **Key checks:**
> - `VTP Operating Mode: Server` on Switch0.
> - `VTP Operating Mode: Client` on Switch1 and Switch2.
> - `VTP Domain Name: BCOT` **matches on all three switches**.
> - `Configuration Revision` is **identical** on all switches — this is the synchronisation proof.

---

### Step 2 — Verify VLAN Propagation (`show vlan brief`)

Run `show vlan brief` on **Switch1** and **Switch2** — switches where you **never** ran any `vlan` commands. If VTP is working, the VLANs defined on Switch0 will appear here automatically.

```
Switch1# show vlan brief
Switch2# show vlan brief
```

#### Expected Output (on Client switches):

| VLAN | Name | Status | Ports |
| :--- | :--- | :--- | :--- |
| 1 | default | active | (unassigned ports) |
| 10 | HR | active | |
| 20 | IT | active | |
| 30 | SALES | active | |

> ✅ **Proof of success:** VLANs 10, 20, and 30 appear on Client switches even though `vlan` commands were **never typed** on those switches. VTP propagated the database automatically.

---

### Step 3 — Verify Trunk Status (`show interfaces trunk`)

Confirm that all trunk links are active and carrying the VLANs:

```
Switch0# show interfaces trunk
```

| Port | Mode | Encapsulation | Status | Native VLAN |
| :--- | :--- | :--- | :--- | :--- |
| Fa0/1 | on | 802.1q | trunking | 1 |

> ✅ If any port shows `not-trunking`, VTP advertisements are not reaching the next switch. Fix with `switchport mode trunk` on both ends of the affected link.

---

### Verification Checklist

- [x] **VTP Mode:** `show vtp status` confirms Switch0 = Server, Switch1 & Switch2 = Client.
- [x] **VTP Domain:** All switches show `VTP Domain Name: BCOT`.
- [x] **Revision Match:** Configuration Revision number is identical across all switches.
- [x] **VLAN Propagation:** `show vlan brief` on Clients shows VLANs 10, 20, 30 — without manual configuration.
- [x] **Trunk Links:** `show interfaces trunk` confirms all inter-switch links are in `trunking` state.

---

## ❗ Common Issues

| Issue | Symptom | Fix |
| :--- | :--- | :--- |
| **Domain name mismatch** | Client shows no VLANs | Ensure `vtp domain` name matches **exactly** (case-sensitive) on all switches |
| **Password mismatch** | Revision numbers drift apart | Ensure `vtp password` matches on all switches |
| **Trunk not configured** | VLANs don't propagate | Run `switchport mode trunk` on both ends of every inter-switch link |
| **High revision number bomb** | Clients wipe Server's VLAN database | Never connect a used switch (high revision #) as Client without first resetting it (`vtp mode transparent` → `vtp mode client`) |
| **Client rejects VLAN commands** | `VTP VLAN configuration not allowed` error | This is correct behaviour — create VLANs on the Server, not the Client |
| **VLANs exist but ports unassigned** | VTP propagates VLAN definitions, not port assignments | Port-to-VLAN assignments must still be configured locally on each switch |

> ⚠️ **The Revision Number Danger:** This is the most notorious VTP gotcha in production networks. If a switch with a **higher VTP revision number** joins your domain as a Client, it will **overwrite** the Server's VLAN database with its own — potentially deleting all your VLANs and taking down the network. Always reset a switch's VTP revision number to 0 before adding it to a production network (set mode to `transparent`, then back to `client`).

---

## 📁 Files

> 📝 **Reference Lab Sheet** — This lab includes a detailed PDF guide with Cisco 2950-specific step-by-step instructions. Open it alongside Packet Tracer to follow along precisely.

- [`VTP Practical Lab Sheet using Cisco 2950 Switches update.pdf`](VTP%20Practical%20Lab%20Sheet%20using%20Cisco%202950%20Switches%20update.pdf) — Detailed lab sheet for this practical.

---

## 🚀 What I Learned

- **VTP Simplifies Large Networks:** In a real campus network with 50+ switches, creating VLANs manually on every switch is error-prone and time-consuming. VTP allows an administrator to define VLANs once on the Server and have them propagate automatically — a significant operational advantage.
- **The Revision Number is Everything:** VTP synchronises to the highest revision number it sees. This is a powerful but dangerous feature. A carelessly introduced switch with a high revision number can silently wipe an entire production VLAN database. Always reset revision numbers before adding switches to existing domains.
- **VTP Does Not Assign Ports:** A common misconception. VTP propagates VLAN **definitions** (ID and name), not port assignments. Each switch still requires local `switchport access vlan X` configuration on its access ports.
- **Trunks are a Prerequisite:** VTP advertisements travel inside CDP/VTP frames on trunk links. If there is no trunk between two switches, VTP updates never arrive — the Clients will never learn the VLANs.
- **Password Protection is Critical:** Without a VTP password, any switch plugged into the network can inject false VLAN updates. Always configure `vtp password` in production environments.
