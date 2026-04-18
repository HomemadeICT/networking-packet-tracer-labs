

---

# **Switch Placement Discussion (Centralized vs Distributed Design)**

In this network design, there was a discussion about whether all switches should be placed in the server room (centralized) or distributed across each lab.

My senior highlighted some important concerns about security and reliability, which I understand and agree with to some extent.

---

## **1. Senior’s Concerns**

The main concerns are:

- **Physical Security:**  
    If switches are placed inside labs, students or unauthorized users might plug in their own devices, disconnect cables, or interfere with the network.
    
- **Environmental Conditions:**  
    Server rooms are designed for 24/7 operation with proper cooling.  
    Labs may become hot or dusty, which can affect switch performance or cause failures.
    

---

## **2. Reasons for Keeping Switches in Each Lab**

However, based on my design and understanding from **Cisco Packet Tracer**, there are practical reasons to keep switches inside each lab:

- **Cable Distance Limitation:**  
    Ethernet cables (Cat6/Cat6a) support up to about **100 meters**.  
    If all PCs are connected directly to the server room, this limit might be exceeded.
    
- **Reduced Cabling Complexity:**  
    Instead of running many cables (30–50) to the server room, we can use:
    
    - One **uplink cable** (10Gbps)
        
    - Connected to a **local lab switch**
        
- **Easy Troubleshooting:**  
    Having switches inside the lab makes it easier to check connections and fix issues quickly during practical sessions.
    

---

## **3. Proposed Solution (Balanced Approach)**

To solve both sides (security + practicality), I suggest a **combined approach**:

### **A. Locked Cabinet for Security**

- Install a **wall-mounted rack cabinet** in each lab
    
- Keep switches inside and **lock it**
    
- Only authorized staff can access it
    

👉 This improves physical security similar to a server room.

---

### **B. Port Security Configuration**

- Configure **Port Security (Sticky MAC)** on switches
    
- Optionally use **802.1X authentication**
    

👉 If an unknown device is connected:

- The port will automatically **block/shutdown**
    

---

### **C. Proper Equipment for Lab Environment**

- Use **managed switches**
    
- Ensure:
    
    - Good ventilation
        
    - Stable power supply
        

👉 This helps maintain reliable 24/7 operation.

---

## **4. Final Design Approach**

The best solution is to follow a standard hierarchical design:

- **Core Switch → Server Room**
    
- **Access Switches → Inside each lab (secured cabinets)**
    

This approach:

- Maintains **security**
    
- Reduces **cabling issues**
    
- Improves **network performance and manageability**
    

---

## **Conclusion**

In conclusion, while centralizing switches improves security, it is not always practical for larger buildings.

By using:

- Locked cabinets
    
- Port security configurations
    
- Proper network design
    

we can safely keep switches inside labs while maintaining both **security and efficiency**.

---
