This is a classic Cisco Packet Tracer project that follows a hierarchical network design. Since you have over 80 devices and specific security requirements, we need to be very organized with the **IP Addressing Schema** and the **CLI configurations**.

Here is the step-by-step technical guide to configuring this network based on your requirements.

---

## 1. IP Addressing Plan (Proposed)

To meet the "Scalability" and "DHCP" requirements, we will use a Class C private range with logical separation.

|**Lab / VLAN**|**VLAN ID**|**Subnet**|**Gateway (L3 Switch)**|
|---|---|---|---|
|**Hardware Lab**|10|`192.168.10.0/24`|`192.168.10.1`|
|**NDICT Lab**|20|`192.168.20.0/24`|`192.168.20.1`|
|**Graphics Lab**|30|`192.168.30.0/24`|`192.168.30.1`|
|**Staff/Mgmt**|40|`192.168.40.0/24`|`192.168.40.1`|

---

## 2. Layer 2: Switching & Port Security

On your **Access Switches** (the ones directly connected to PCs), you need to create the VLANs and assign ports.

### Create VLANs (Repeat on all switches)

Bash

```
vlan 10
 name Hardware_Lab
vlan 20
 name NDICT_Lab
vlan 30
 name Graphics_Lab
vlan 40
 name Staff_Mgmt
exit
```

### Port Security (Requirement #9)

Apply this to the ports connected to PCs (e.g., FastEthernet 0/1 - 24):

Bash

```
interface range fa0/1-24
 switchport mode access
 switchport access vlan 10  # Change per lab
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky
```

### Trunking (Requirement #7)

On links connecting Switch to Switch or Switch to Router:

Bash

```
interface gig0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,40
```

---

## 3. Layer 3: Inter-VLAN Routing & DHCP

Since you have a **Layer 3 Switch** (Core), we will use **SVIs (Switch Virtual Interfaces)** to act as the gateways.

### Enable Routing and Gateways

Bash

```
ip routing

interface vlan 10
 ip address 192.168.10.1 255.255.255.0
interface vlan 20
 ip address 192.168.20.1 255.255.255.0
# ... repeat for VLAN 30 and 40
```

### DHCP Pools (Requirement #5)

The L3 switch will automatically hand out IPs:

Bash

```
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool VLAN10_POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
# ... repeat for other pools
```

---

## 4. Security: Access Control Lists (ACLs)

Requirement #4 states that labs cannot talk to each other, but **Staff** can talk to everyone. We apply these to the VLAN interfaces.

Bash

```
ip access-list extended LAB_RESTRICTION
 permit ip 192.168.10.0 0.0.0.255 192.168.40.0 0.0.0.255  # Allow to Staff
 deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255   # Block to NDICT
 deny ip 192.168.10.0 0.0.0.255 192.168.30.0 0.0.0.255   # Block to Graphics
 permit ip any any # Allow Internet/Local
 
interface vlan 10
 ip access-group LAB_RESTRICTION in
```

---

## 5. Edge: Internet Access (NAT)

On the **Router** connected to the "ISP":

Bash

```
interface gig0/0 (Internal side)
 ip nat inside
interface gig0/1 (External side)
 ip nat outside

access-list 1 permit 192.168.0.0 0.0.255.255
ip nat inside source list 1 interface gig0/1 overload
```

---

## 6. Remote Management (SSH)

Requirement #8: Disable Telnet and use SSH.

Bash

```
hostname CoreSwitch
ip domain-name lab.local
crypto key generate rsa  # Choose 1024 or 2048
username admin privilege 15 secret your_password

line vty 0 4
 transport input ssh
 login local
```

# 