

---

# 🧠 🏁 FINAL NETWORK CONFIG CHECKLIST

---

# 🌐 1. Router (Cisco 2911)

### ✅ Interfaces

-  `G0/0` → ISP (or simulated internet)
    
-  `G0/1` → Core Switch
    

```bash
no shutdown
ip address configured
```

---

### ✅ DHCP (if using router)

-  Pools created for each VLAN:
    
    - VLAN 10 → 192.168.10.0
        
    - VLAN 20 → 192.168.20.0
        
    - VLAN 30 → 192.168.30.0
        
-  Default gateway set correctly
    
-  DNS added (optional)
    

---

### ✅ NAT (optional but good)

```bash
ip nat inside / outside configured
```

---

---

# 🧠 2. Core Switch (3560 / 3650) — THE BRAIN

---

### ✅ Enable Layer 3

```bash
ip routing
```

---

### ✅ VLANs Created

-  VLAN 10 → Hardware
    
-  VLAN 20 → NDICT
    
-  VLAN 30 → Graphics
    
-  VLAN 40 → Staff/Server
    

---

### ✅ SVIs (Default Gateways)

-  `VLAN 10 → 192.168.10.1`
    
-  `VLAN 20 → 192.168.20.1`
    
-  `VLAN 30 → 192.168.30.1`
    
-  `VLAN 40 → 192.168.40.1`
    
-  All `no shutdown`
    

---

### ✅ Trunk Ports (VERY IMPORTANT)

-  Gi0/1 → Hardware switch → trunk
    
-  Gi0/2 → NDICT switch → trunk
    
-  Gi0/3 → Graphics switch → trunk
    
-  Fa0/24 (if used) → trunk
    

```bash
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
```

---

### ✅ DHCP Relay (ONLY if DHCP on Router)

-  `ip helper-address` set on:
    
    - VLAN 10
        
    - VLAN 20
        
    - VLAN 30
        

---

### ✅ Routing

-  Default route to router:
    

```bash
ip route 0.0.0.0 0.0.0.0 192.168.40.254
```

---

### ✅ ACLs (VLAN Isolation)

-  Hardware ↔ NDICT → blocked
    
-  Hardware ↔ Graphics → blocked
    
-  NDICT ↔ Graphics → blocked
    
-  Staff → allowed
    

---

---

# 🔌 3. Access Switches (2960) — EACH LAB

(Repeat for Hardware / NDICT / Graphics)

---

### ✅ VLAN Exists

-  VLAN created (10 / 20 / 30 depending on lab)
    

---

### ✅ Access Ports (PCs)

```bash
interface range fa0/1 - 24
switchport mode access
switchport access vlan XX
```

---

### ✅ Uplink to Core

```bash
interface gi0/1
switchport mode trunk
```

---

### ✅ Port Security (🔥 high marks)

-  Enabled on all PC ports
    

```bash
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
```

---

### ✅ Unused Ports

-  Shutdown unused ports
    

---

---

# 🖥️ 4. PCs (End Devices)

---

### ✅ IP Assignment

-  Set to **DHCP** OR Static
    

---

### ✅ If Static:

- Hardware → `192.168.10.x`
    
- NDICT → `192.168.20.x`
    
- Graphics → `192.168.30.x`
    

---

### ✅ Gateway

-  Matches VLAN:
    
    - `192.168.10.1`
        
    - `192.168.20.1`
        
    - `192.168.30.1`
        

---

---

# 🧾 5. Server (Optional but Good)

---

### ✅ Placement

-  Connected to Core Switch
    
-  In VLAN 40
    

---

### ✅ Services

-  DNS (optional)
    
-  HTTP (optional)
    
-  SNMP (bonus marks)
    

---

---

# 🧪 6. FINAL TESTING (DON’T SKIP THIS)

---

### ✅ Connectivity Tests

#### ✔ Same VLAN

-  PC → PC (same lab) → **SUCCESS**
    

#### ❌ Different VLAN

-  Hardware → NDICT → **FAIL**
    
-  NDICT → Graphics → **FAIL**
    

#### ✔ Gateway

-  PC → VLAN gateway → **SUCCESS**
    

#### ✔ Core Communication

-  Ping core switch → **SUCCESS**
    

#### ✔ Internet (if configured)

-  Ping router → **SUCCESS**
    

---

---

# 🎯 BONUS (For Viva 🔥)

If they ask “Why your design is good?”:

You say:

- ✔ “Hierarchical model (Core + Access layers)”
    
- ✔ “Inter-VLAN routing handled at L3 switch”
    
- ✔ “Broadcast domains reduced using VLANs”
    
- ✔ “Security enforced via ACLs and port security”
    
- ✔ “Scalable and manageable design”
    

---

# ⚡ FINAL VERDICT

If everything above is checked:

👉 Your network is:

- ✔ Technically correct
    
- ✔ Industry-aligned
    
- ✔ Fully functional in Packet Tracer
    
- ✔ Viva-ready 💯
    

---

If you want, I can next:  
👉 Review your actual `.pkt` file like a lecturer  
👉 Or give you a **“common mistakes examiner will catch” list** (this is super useful)