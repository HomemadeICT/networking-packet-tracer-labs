# 📋 Project Context — Networking Practical Book

> **Last Updated:** 2026-04-18  
> This document captures the full context of the Networking Practical Book project — its purpose, scope, structure, and current state.

---

## 🎯 Purpose

This repository is a **comprehensive, portfolio-ready collection of Cisco Packet Tracer networking labs** created as part of the **NDICT NVQ Level 5** qualification. It progresses from fundamental connectivity concepts through to a full **enterprise-grade campus network** deployment.

**Author:** D.M. Theekshana Prabodha  
**Company (fictional branding):** TheekMind (theekmind.com)  
**Qualification:** NDICT NVQ Level 5 — Networking

---

## 🏗️ Repository Structure (Actual — As Built)

```
Networking Practical Book/
│
├── 01_Basic_Connectivity/
│   └── one-router-4-switch-8pcs/          ← LAN topology, multi-switch, basic routing
│       ├── README.md                       (professional lab report ✅)
│       ├── one router 4 switch 8 pcs.pkt
│       └── screenshots/
│
├── 02_Network_Services/
│   ├── dhcp-server/                       ← DHCP auto IP assignment
│   │   ├── README.md ✅ | DHCP SERVER.pkt
│   ├── dns-web-server/                    ← DNS + Web hosting (single subnet)
│   │   ├── README.md ✅ | DNS and WEB Server.pkt
│   ├── dns-web-router/                    ← DNS + Web across subnets with router
│   │   ├── README.md ✅ | DNS+ Web Servers With Router.pkt
│   ├── mail-server/                       ← SMTP/POP3 email
│   │   ├── README.md ✅ | mail practical.pkt | Switch0_running-config.txt
│   └── web-project/                       ← Web server practical
│       ├── README.md ✅ | project with web server.pkt
│
├── 03_VLAN_and_Switching/
│   ├── 1.vtp/                             ← VLAN Trunking Protocol
│   │   ├── README.md ✅ | VTP Practical Lab Sheet.pdf
│   ├── 2.vlan-basic/                      ← Basic VLAN creation & port assignment
│   │   ├── README.md ✅ | Creating and Configuring VLANs.pkt
│   ├── 3.inter-vlan-routing/              ← Layer 3 switch inter-VLAN routing
│   │   ├── README.md ✅ | Inter Vlan Routing.pkt
│   ├── 4.router-on-a-stick/               ← Single-arm router inter-VLAN routing
│   │   ├── README.md ✅ | Router on Stick.pkt
│   └── 5.inter-vlan-advanced/             ← Advanced inter-VLAN configurations
│       ├── README.md ✅ | Inter Vlan configuration Practical 2.pkt
│
├── 04_Security_and_Remote_Access/
│   ├── port-security/                     ← MAC-based port security
│   │   ├── README.md ✅ | Port Security.pkt
│   └── telnet/                            ← Remote access via Telnet
│       ├── README.md ✅ | telnet practical 01.pkt
│
├── 05_Wireless/
│   ├── IOT Practical/                     ← IoT actuator + Home Gateway connectivity
│   │   ├── README.md ✅ | IOT Practical.pkt
│   └── Wireless Networking/               ← WRT300N wireless config
│       ├── README.md ✅ | Wireless Networking.pkt | Screenshots (3)
│
├── 06_Final_Project/
│   ├── Capstone Project/
│   │   └── TechNova-Enterprise-Network-Design/  ← Router-on-a-Stick capstone
│   │       ├── README.md ✅ (616 lines — full lab report)
│   │       ├── TechNova-Enterprise-Network-Design.pkt
│   │       ├── TechNova-Enterprise-Network-Design.pdf
│   │       └── topology screenshot
│   │
│   └── Network Lab 80 ( Workstations ) Projects Proposal/  ← PRIMARY FINAL PROJECT
│       ├── README.md ✅ (1,458 lines — comprehensive enterprise documentation)
│       ├── Project Proposal Topology.pkt
│       ├── Professional_Full_Proposal.pdf
│       ├── Professional_Full_Proposal.docx
│       ├── Final Project Diagrams/        ← Architecture diagrams
│       ├── Images/                        ← All screenshots + infographics
│       │   └── Cisco Packet Tracer Screenshots/ ← Verification evidence
│       ├── Configuration Step 01/         ← Core Switch config journey
│       ├── Configuration Step 02/         ← Access switch configs
│       ├── Notes/                         ← 15 reference documents including:
│       │   ├── Full Implementation Plan.md (794 lines)
│       │   ├── Inter-VLAN Routing.md
│       │   ├── Network Design Best Practices.md
│       │   ├── All Hardware Equipments.md
│       │   └── ... (13 more reference docs)
│       └── Until 2026.04.16 Progress/     ← Progress snapshots
│
├── Project Preferences/                    ← THIS FOLDER — meta/planning docs
│   ├── Project Context.md                  ← You are here
│   ├── Career Leverage Roadmap.md
│   ├── Final Delivarables.md
│   └── Final Project Format.md
│
├── README.md                              ← Root navigation hub (✅ completed)
└── .gitignore
```

---

## 📊 Lab Inventory Summary

| # | Category | Labs | Key Technologies | Status |
|---|---|---|---|---|
| 01 | Basic Connectivity | 1 lab | LAN topology, multi-switch, basic routing | ✅ Complete |
| 02 | Network Services | 5 labs | DHCP, DNS, Web Server, Mail Server, DNS+Router | ✅ Complete |
| 03 | VLAN & Switching | 5 labs | VTP, VLAN, Inter-VLAN (L3 switch + RoAS), Advanced | ✅ Complete |
| 04 | Security & Remote Access | 2 labs | Port Security (sticky MAC), Telnet | ✅ Complete |
| 05 | Wireless & IoT | 2 labs | WRT300N wireless, IoT actuator/Home Gateway | ✅ Complete |
| 06 | Final Projects | 2 projects | Enterprise Network (L3 Core + RoAS Capstone) | ✅ Complete |
| | **Total** | **17 labs/projects** | | |

---

## 🏢 Final Project: Network Lab 80 Workstations (PRIMARY)

The flagship project — a fully configured hierarchical campus network for 80+ workstations:

| Feature | Status | Details |
|---|---|---|
| Hierarchical Design | ✅ Complete | Core L3 Switch (3560) + 5 Access Switches (2960) |
| VLAN Segmentation | ✅ Complete | VLAN 10 (Hardware), 20 (NDICT), 30 (Graphics), 40 (Staff) |
| Inter-VLAN Routing | ✅ Complete | Layer 3 SVIs on Core Switch (not Router-on-a-Stick) |
| DHCP | ✅ Complete | Centralised server + `ip helper-address` relay on all SVIs |
| NAT/PAT | ✅ Complete | Internet simulation via edge router (Cisco 2911) |
| Trunking & Pruning | ✅ Complete | 802.1Q trunks with per-lab VLAN pruning |
| Port Security | ✅ Partial | Tested on HW Lab switch (sticky MAC, restrict mode) |
| ACL VLAN Isolation | 🔜 Documented | Configs ready, applying post-publish |
| SSH Management | 🔜 Documented | Configs ready, applying post-publish |
| Screenshots & Verification | ✅ Complete | CLI screenshots proving all claims |

---

## 🌐 Capstone Project: TechNova Enterprise Network

A secondary project demonstrating **Router-on-a-Stick** architecture:

- 5 VLANs (Admin, HR, IT, Server, Management)
- Centralised DHCP, DNS, HTTP services
- `ip helper-address` relay for cross-VLAN DHCP
- Device hardening (enable secret, VTY passwords, banners)
- Full verification checklist (all items ✅)

---

## 📝 Documentation Philosophy

Every lab README follows this structure:
1. **Objective** — What the lab demonstrates
2. **Concepts Covered** — Technologies and protocols used
3. **Devices Used** — Device inventory table
4. **High-Level Design** — Network topology (ASCII diagram)
5. **Low-Level Configuration** — Device-by-device CLI commands with explanations
6. **Verification** — CLI output and expected results
7. **Common Issues** — Troubleshooting table
8. **What I Learned** — Personal reflection
9. **My Personal Experience** — Raw notes from the build session

Each lab also includes:
- `.pkt` Packet Tracer file (openable, fully configured)
- `screenshots/` folder with raw session notes

---

## 🔑 Key Design Decisions

| Decision | Rationale |
|---|---|
| **Layer 3 Core Switch (3560) over Router-on-a-Stick** | 80+ devices — single trunk link bottleneck unacceptable. Hardware ASICs route at wire speed. |
| **VLAN ID = third octet** (VLAN 10 → 192.168.10.0) | Intuitive troubleshooting — IP instantly reveals department |
| **DHCP relay instead of per-VLAN servers** | Single server, centralised management, no IP duplication |
| **Trunk pruning** | Security + performance — HW Lab trunk carries only VLAN 10,40 |
| **ACLs last (post-publish)** | GitHub commit history shows progression: baseline → secured |
| **Port security: restrict mode** | Less disruptive than shutdown in a classroom environment |
| **TechNova as second project** | Demonstrates both L3 Core and Router-on-a-Stick architectures |
