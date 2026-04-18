

---

# 📌 📘 Network Infrastructure Project

## Functional & Non-Functional Requirements

---

# 🧩 ✅ Functional Requirements

_(What the system MUST do)_

---

## 🌐 1. Network Connectivity

- The network must provide connectivity for **80+ end devices** across three laboratories:
    
    - Hardware Lab
        
    - NDICT Lab
        
    - Graphics Lab
        
- All devices within the same lab must communicate seamlessly.
    

---

## 🧱 2. VLAN Segmentation

- The network must implement VLAN-based segmentation:
    
    - VLAN 10 → Hardware Lab
        
    - VLAN 20 → NDICT Lab
        
    - VLAN 30 → Graphics Lab
        
    - VLAN 40 → Staff/Management
        
- Each VLAN must operate as an independent broadcast domain.
    

---

## 🔁 3. Inter-VLAN Routing

- The system must support **inter-VLAN routing** via a Layer 3 switch.
    
- Only authorized VLANs (e.g., Staff VLAN) should be able to communicate with all other VLANs.
    

---

## 🚫 4. Access Control (Security)

- The network must restrict communication between student VLANs:
    
    - Hardware ↔ NDICT → blocked
        
    - Hardware ↔ Graphics → blocked
        
    - NDICT ↔ Graphics → blocked
        
- ACLs must be implemented to enforce these restrictions.
    

---

## 📡 5. Dynamic IP Addressing (DHCP)

- The system must automatically assign IP addresses to all client devices.
    
- Each VLAN must have its own DHCP scope.
    

---

## 🌍 6. Internet Access (Simulated)

- All VLANs must have controlled access to external networks via a router.
    
- NAT must be implemented to allow private IP communication externally.
    

---

## 🔌 7. Trunking & VLAN Propagation

- Switch-to-switch links must support multiple VLANs using IEEE 802.1Q trunking.
    
- VLAN traffic must be properly tagged across trunk links.
    

---

## 8. Device Management

- Network devices must support **secure remote management**:
    - SSH access is enabled for administrative access
- The network design **allows integration of centralized monitoring systems** (e.g., SNMP-based tools),  
    however, this is **planned as a future enhancement and not implemented in the current version**.

---

## 🔐 9. Port Security

- Access switch ports must restrict unauthorized devices:
    
    - Maximum 1 MAC address per port
        
    - Sticky MAC learning enabled
        
    - Violation action: shutdown
        

---

## 🧾 10. Server Services (Optional Enhancement)

- A server must provide:
    
    - DNS services (optional)
        
    - Monitoring/logging capabilities
        

---

---

# ⚙️ 🚀 Non-Functional Requirements

_(How well the system performs)_

---

## ⚡ 1. Performance

- The network must support smooth communication with minimal latency within each lab.
    
- Gigabit uplinks should be used where possible to reduce congestion.
    
- The system should handle simultaneous usage by 80+ devices.
    

---

## 📈 2. Scalability

- The design must allow easy expansion:
    
    - Additional labs or VLANs can be added without redesigning the network
        
- IP addressing scheme must support future growth.
    

---

## 🔒 3. Security

- Unauthorized access must be minimized using:
    
    - VLAN segmentation
        
    - ACLs
        
    - Port security
        
- Management access must be secured using SSH instead of Telnet.
    

---

## 🧰 4. Maintainability

- The network must be easy to troubleshoot:
    
    - Clear hierarchical design (Core–Access model)
        
    - Logical IP addressing scheme
        
- Devices should be centrally manageable.
    

---

## 🔄 5. Reliability

- The network must ensure consistent uptime during lab sessions.
    
- Failure in one lab should not impact other labs.
    

---

## 🧩 6. Availability

- Core services (routing, DHCP) must remain accessible at all times.
    
- Network design should avoid single points of failure where possible.
    

---

## 7. Manageability & Monitoring _(Future Enhancement)_

- The network design **anticipates support** for monitoring and management tools such as:
    - SNMP-based monitoring
    - Syslog logging
- These features are **not implemented in the current version**, but the architecture allows easy integration in future upgrades.
- Future implementation will enable administrators to:
    - Monitor device health and traffic
    - Detect failures and performance issues

---

## 🧑‍🏫 8. Usability

- End users (students/staff) should:
    
    - Connect without manual configuration
        
    - Receive automatic IP configuration via DHCP
        
- Network access should be seamless and transparent.
    

---

## 💾 9. Documentation

- The system must include:
    
    - Network topology diagram
        
    - IP addressing plan
        
    - Configuration documentation
        

---

## 🧪 10. Testability

- The network must support validation through:
    
    - Ping tests
        
    - VLAN isolation verification
        
    - DHCP functionality checks
        

---

# 🏁 💡 Pro Tip for GitHub README

After this section, add:

- 📷 Network Diagram (your image)
    
- ⚙️ Configuration Files (CLI)
    
- 🧪 Testing Results (screenshots)
    
- 📚 Lessons Learned
    

---

# ⚡ Final Thought

This is now:

- ✔ Portfolio-ready
    
- ✔ Industry-style documentation
    
- ✔ Strong enough for interviews
    

---

If you want next level:  
👉 I can turn this into a **clean README.md (perfect GitHub format)**  
👉 Or help you write a **project description that impresses recruiters in 10 seconds**