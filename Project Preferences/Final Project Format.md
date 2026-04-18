# 📐 Final Project Format — Networking Practical Book

> **Last Updated:** 2026-04-18  
> Documents the actual structure, format, and standards used across the completed project.

---

## 🏗️ GitHub Repository Structure (Actual — Implemented)

```
Networking Practical Book/
│
├── 01_Basic_Connectivity/
│   └── one-router-4-switch-8pcs/
│       ├── README.md                      ← Professional lab report
│       ├── one router 4 switch 8 pcs.pkt  ← Packet Tracer file
│       └── screenshots/        ← Raw session notes
│
├── 02_Network_Services/
│   ├── dhcp-server/           ← DHCP auto IP assignment
│   ├── dns-web-server/        ← DNS + HTTP single subnet
│   ├── dns-web-router/        ← DNS + HTTP across routed subnets
│   ├── mail-server/           ← SMTP/POP3 email services
│   └── web-project/           ← Web server deployment
│
├── 03_VLAN_and_Switching/
│   ├── 1.vtp/                 ← VLAN Trunking Protocol
│   ├── 2.vlan-basic/          ← VLAN creation & port assignment
│   ├── 3.inter-vlan-routing/  ← L3 switch inter-VLAN
│   ├── 4.router-on-a-stick/   ← Single-arm router inter-VLAN
│   └── 5.inter-vlan-advanced/ ← Advanced inter-VLAN configs
│
├── 04_Security_and_Remote_Access/
│   ├── port-security/         ← MAC-based port security
│   └── telnet/                ← Remote CLI management
│
├── 05_Wireless/
│   ├── IOT Practical/         ← IoT actuator + Home Gateway
│   └── Wireless Networking/   ← WRT300N wireless config + screenshots
│
├── 06_Final_Project/
│   ├── Capstone Project/
│   │   └── TechNova-Enterprise-Network-Design/
│   │       ├── README.md      ← 616 lines — full lab report
│   │       ├── .pkt + .pdf    ← Simulation + exported PDF
│   │       └── screenshots/
│   │
│   └── Network Lab 80 ( Workstations ) Projects Proposal/
│       ├── README.md          ← 1,458 lines — enterprise documentation
│       ├── Project Proposal Topology.pkt
│       ├── Professional_Full_Proposal.pdf / .docx
│       ├── Final Project Diagrams/
│       ├── Images/            ← All infographics + Cisco PT screenshots
│       ├── Configuration Step 01-02/
│       └── Notes/             ← 15 reference documents
│
├── Project Preferences/       ← Meta/planning documents
├── README.md                  ← Root navigation hub
└── .gitignore
```

---

## 📄 Lab README Template (Standardised Format — Used Across All 17 Labs)

Every lab follows this consistent structure:

```md
# 📡 Lab: [Lab Title]

## 🎯 Objective
[What this lab demonstrates and the end goal]

---

## 🧠 Concepts Covered
[Table of technologies/protocols with descriptions]

---

## 🛠️ Devices Used
[Table: Device | Model | Qty | Role]

---

## 🗺️ High-Level Network Plan
[ASCII topology diagram showing device layout and connections]

### Addressing & VLAN Schema
[Table: Department | VLAN ID | Subnet | Gateway | DHCP Scope]

---

## ⚙️ Low-Level Configuration (Device-by-Device)
[Step-by-step CLI commands with explanatory comments]
[Each phase clearly separated with rationale]

---

## ✅ Verification Checklist
[Table or list of tests performed with pass/fail results]
[CLI commands used for verification]

---

## ❗ Common Issues & Fixes
[Table: Issue | Symptom | Fix — real problems encountered]

---

## 📁 Lab File
[.pkt filename]

---

## 🚀 What I Learned
[Personal reflection on key takeaways]

---

## 📸 Screenshots
[Embedded Packet Tracer screenshots as evidence]
```

---

## 🏢 Enterprise Project Documentation Format (Network Lab 80)

The flagship project README uses an expanded format:

### Structure (1,458 lines):
1. **Project Overview** — Scale, concepts demonstrated, physical device count
2. **High-Level Design (HLD)**
   - Network Topology (ASCII + diagram images)
   - Design Decision Rationale (L3 Core vs RoAS — with comparison table)
   - SVI Explanation (with infographic)
   - VLAN Strategy (with purpose table)
   - IP Addressing Plan (with DHCP pool ranges)
   - Security Policy (source/destination matrix)
   - Device Inventory (all 90+ devices listed)
3. **Physical Wiring Plan** — Room-by-room cable layout
4. **Configuration Lifecycle** — 13-step ordered sequence with rationale
5. **Low-Level Design (LLD)** — Device-by-device configs
   - Port Mode Selection Guide (with infographic)
   - 8 devices fully documented with CLI commands + interface maps
6. **Verification with Screenshots** — CLI output proving every claim
   - Core Switch verification (VLANs, SVIs, routes, trunks)
   - Access switch verification (trunk + port security)
   - End-to-end connectivity (inter-VLAN pings)
   - DHCP IP assignment proof (per-VLAN ipconfig)
   - Port security verification
7. **Traffic Flow Diagrams** — Two end-to-end scenarios explained step-by-step
8. **Mistakes & Troubleshooting Journal** — 5 real errors with diagnosis and fixes
9. **Golden Rules Learned** — 12 rules distilled from real experience
10. **Future Enhancements** — ACLs, SSH, Port Security (fully designed, documented, ready to apply)
11. **Project Status** — Complete status matrix
12. **Repository Structure** — File map

---

## 📊 Visual Assets Created

### Custom Infographics (Network Lab 80):
- `TRAFFIC FLOW COMPARISON LAYER 3 CORE SWITCHING vs ROUTER-ON-A-STICK.png`
- `UNDERSTANDING SVI THE KEY TO INTER-VLAN ROUTING.png`
- `GUIDE TO NETWORKING BEST PRACTICES - PORT MODE SELECTION.png`
- `NETWORK INFRASTRUCTURE CONFIGURATION LIFECYCLE.png`

### Architecture Diagrams:
- `Final Networking Architecture.jpg` — Physical layout
- `Networking Diagram - Packet Tracer Diagram.png` — Logical topology

### Packet Tracer Screenshots:
- 15+ screenshots in `Images/Cisco Packet Tracer Screenshots/`
- Cover: VLAN tables, trunk status, routing tables, ping tests, DHCP proof, port security

---

## 🔧 Technical Depth — What's Actually Implemented

| Area | Original Plan | Actual Implementation |
|---|---|---|
| Network Design | "Multiple switches + 1 router" | 1 L3 Core Switch + 5 Access Switches + 1 Edge Router |
| VLANs | "3 VLANs (HR, IT, Sales)" | 4 VLANs (Hardware, NDICT, Graphics, Staff) |
| Routing | "Router-on-a-Stick" | **Layer 3 SVIs** (main project) + RoAS (TechNova capstone) |
| DHCP | "Auto IP" | Centralised server + `ip helper-address` relay on all SVIs |
| DNS | "Name resolution" | ✅ Configured on DHCP/DNS/Web server |
| Web Server | "Company website" | ✅ Internal web server accessible from all VLANs |
| Email | "Maybe Email" | ✅ Email server in TechNova capstone |
| Port Security | "Limit MAC addresses" | ✅ Sticky MAC + restrict mode (tested on HW Lab) |
| Disable Unused Ports | Listed as goal | ✅ All unused ports shutdown on every switch |
| ACLs | "Maybe in future" | 🔜 Fully designed, configs documented, applying post-publish |
| SSH | Not planned originally | 🔜 Fully designed, configs documented, applying post-publish |
| NAT/PAT | Not planned originally | ✅ Internet simulation via edge router |
| Trunk Pruning | Not planned originally | ✅ Per-lab VLAN pruning on all trunks |
| Network Diagrams | "draw.io or screenshot" | ✅ Custom infographics + PT topology screenshot |
| IP Addressing Table | Listed as goal | ✅ Full table with DHCP ranges in README |
| VLAN Table | Listed as goal | ✅ Full table with purpose descriptions |

---

## 📦 Deliverable Formats

| Deliverable | Format | Location |
|---|---|---|
| Lab reports | Markdown READMEs | Each lab folder |
| Network simulations | `.pkt` files | Each lab folder |
| Formal proposal | `.pdf` + `.docx` | `06_Final_Project/.../Professional_Full_Proposal.*` |
| Architecture diagrams | `.jpg` + `.png` | `06_Final_Project/.../Final Project Diagrams/` |
| Infographics | `.png` | `06_Final_Project/.../Images/` |
| Verification evidence | `.png` screenshots | `06_Final_Project/.../Images/Cisco Packet Tracer Screenshots/` |
| Configuration guides | `.md` | `06_Final_Project/.../Configuration Step 01-02/` |
| Reference documentation | `.md` (15 files) | `06_Final_Project/.../Notes/` |
| Raw session notes | `.md` | `screenshots/` in each lab |
| Root navigation | `README.md` | Repository root |