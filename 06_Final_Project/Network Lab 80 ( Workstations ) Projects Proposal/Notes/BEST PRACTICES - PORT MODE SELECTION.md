![Port Mode Selection](../Images/GUIDE%20TO%20NETWORKING%20BEST%20PRACTICES%20-%20PORT%20MODE%20SELECTION.png)

---

### 1. Switch ↔ Switch (Always a Trunk)

**The Rule:** If the link needs to carry more than one VLAN, it **must** be a trunk.

- **Why:** In your TechNova design, the Lab Switch might have Admin, HR, and IT users connected to it. A single "Access" cable can't tell the difference between those departments. A **Trunk** uses 802.1Q tagging to keep their data separated on the same wire.
    

### 2. Switch ↔ Router (The Two Methods)

This is where students often get confused, but your table is spot on:

- **Access Mode (Simple):** Use this when the Router interface is just a "member" of one specific VLAN (like your Staff VLAN 40). The router doesn't even know VLANs exist; it just thinks it's on a normal network.
    
- **Trunk Mode (Router-on-a-Stick / RoaS):** Use this when one physical router cable needs to handle **all** your VLANs (10, 20, 30, 50, and 99). The Router must "see" the tags to know which sub-interface should handle the traffic.
    

### 3. Switch ↔ End Device (Always Access)

**The Rule:** PCs, Printers, and standard Servers do not understand VLAN tags.

- **The Logic:** If you send a "tagged" packet to a Windows PC, the PC's network card will usually drop it because it looks like "garbage" data at the end of the frame. Access ports "strip" the tags off before the data hits the PC, making it a "clean" Ethernet frame.
    

---

### The Only "Professional" Exception

There is one common exception you will see in the real world: **IP Phones.**

- Often, a PC plugs into the back of an IP Phone, and the Phone plugs into the Switch.
    
- In this case, the port is technically an **Access Port**, but it carries a "Voice VLAN" and a "Data VLAN" simultaneously. It’s a hybrid, but for your portfolio and assignments, sticking to your table is the 100% correct "Engineering Foundation."
    

---

