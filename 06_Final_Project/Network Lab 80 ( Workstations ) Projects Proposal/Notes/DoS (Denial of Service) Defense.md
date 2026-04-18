### DoS (Denial of Service) Defense

This protects your infrastructure from being overwhelmed by malicious or accidental traffic spikes that could "choke" the trunk ports or the router.

- **Inter-VLAN Protection:** Since your **ER7206 Router** handles the routing between your three labs (inter-VLAN), a DoS attack in the Graphics Lab could potentially crash the gateway for the NDICT Lab. DoS defense prevents this.
    
- **Specific Protections on your TP-Link Switches:**
    
    - **Land Attack:** Prevents packets where the source and destination IP are the same (which loops the processor).
        
    - **Scanning/Flooding:** Limits the number of "Sync" packets or "ICMP" (ping) requests a single port can send per second.
        
    - **Blat Attack:** Drops packets with abnormal TCP flags that try to confuse the switch's state table.