## **Network  Project Report**
abdelrahman -> topology design, ip scheme, review firewall rules, 
test ACL rules , documentation, inter vlan routing 
MALAK -> PACKET IMPLEMENTATION initial configuration
YEHIA -> ROUTING + VLAN
BASEM -> Security Imp


## Network Diagram:

![[design.png]]
# Topology 

The network is structured into distinct zones, each serving different purposes to ensure security, segmentation, and efficient traffic management. The major zones are:

1. **Internet-facing Zone**: This includes the **EDGE-ROUTER** and the **FW-EXTERNAL** firewall, which protect the network from external threats and connect to the public internet.
    
2. **DMZ Zone**: This is a secured area designed to host externally accessible services such as a web server and DNS server. The DMZ servers are placed in a subnet (192.168.40.0/30) that isolates them from internal network resources, allowing external users limited access while maintaining security.
    
3. **Internal Server Zone**: This contains critical internal servers such as the **Database Server**, **SMB (File-sharing) Server**, and FTP Server. These servers are protected behind the **FW-INTERNAL** firewall and are assigned IP addresses from the 192.168.50.0/24 subnet.
    

5. **VLAN Network**: The network has two core switches (**SW-1** and **SW-2**) connected to the **R2** router. These switches handle VLAN traffic from different subnets, ensuring effective segmentation of network traffic across different departments.
### **VLAN Description**

VLANs are employed to segment network traffic and isolate departments or functions within the network. This segmentation improves security and reduces network congestion. The three VLANs implemented are:

- **VLAN 10** (192.168.10.0/24): Used for a specific group of workstations, ensuring network isolation and security for devices within this group.
- **VLAN 20** (192.168.20.0/24): Another group of workstations separated from other network segments.
- **VLAN 30** (192.168.30.0/24): Provides a third isolated network for devices, possibly for administrative or managerial traffic.

The VLANs are connected to **SW-1** and **SW-2**, which in turn connect to **R2**.

### **Routing on a Stick**

The **R2** router is responsible for routing traffic between the VLANs (inter-VLAN routing) using a "Router-on-a-stick" configuration. This means the router has a single interface connected to the switch, but it handles multiple VLANs by configuring subinterfaces. These subinterfaces are logically associated with the VLANs, allowing the router to route traffic between the different VLANs without needing a dedicated physical interface for each VLAN.

This configuration enables devices on different VLANs to communicate while still maintaining VLAN separation for broadcast traffic.

### **DMZ Zone**

The DMZ (Demilitarized Zone) is designed to host servers that need to be accessible from the internet. In this design, the **Web Server** (192.168.40.5) and the **DNS Server** (192.168.40.10) are placed in the DMZ to allow external users access while preventing direct access to the internal network.

The **DMZ Subnet** (192.168.40.0/30) is isolated from the internal network by the **FW-INTERNAL** firewall, providing a layer of security that filters traffic before it reaches the internal network.

### **Data Center Zone**

The data center zone houses critical infrastructure, including the **Database Server** (192.168.50.15), **SMB Server** (192.168.50.10), and **Active Directory Server** (192.168.50.5). These servers provide essential services such as authentication, file sharing, and database management.

This zone is protected by the **FW-DATA** firewall, ensuring that only authorized traffic can reach these servers. Internal users within the network, such as those on the VLANs, can access these servers for business operations.

### **Firewall and Security**

The network is protected by two layers of firewalls:

- **FW-EXTERNAL**: The first firewall (external) acts as a barrier between the internet and the DMZ zone. It filters traffic before it enters the network.
- **FW-INTERNAL**: The second firewall protects internal resources, ensuring that only filtered and authorized traffic from the DMZ or external sources reaches the internal servers.

# IP ADRESSING SCHEME
-----------------------------------
### Internet-facing Section

| **Device/Subnet** | **IP Address/Range** | **Notes**             |
| ----------------- | -------------------- | --------------------- |
| EDGE-ROUTER       | 192.168.40.1         | Connected to internet |
| FW-EXTERNAL       | 192.168.40.2         | First line of defense |

---
### DMZ Section

| **Device/Subnet** | **IP Address/Range** | **Notes**                     |
| ----------------- | -------------------- | ----------------------------- |
| Zoned subnet      | 192.168.40.0/30      | Default Gateway: 192.168.40.1 |
| DMZ Web Server    | 192.168.40.5         | DMZ web server                |
| DMZ DNS Server    | 192.168.40.10        | DNS                           |

--------------------------------------
### Firewall Section

| **Device/Subnet** | **IP Address/Range** | **Notes**                 |
| ----------------- | -------------------- | ------------------------- |
| FW-INTERNAL ASA0  | 192.168.60.1/30      | Filters requests to DMZ   |
| FW-DATA ASA1      | 192.168.40.1/30      | Protects internal servers |

-------------------
### Internal Servers Section

| **Device** | **IP Address** | **Notes**        |
| ---------- | -------------- | ---------------- |
| DB-Server  | 192.168.50.15  | Data base        |
| SMB Server | 192.168.50.10  | File-sharing     |
| AD Server  | 192.168.50.5   | Active Directory |
| R-DATA     | 192.168.50.1   | For Routing      |

-------------------------
### VLANs Section
VLAN Network 192.168.0.0/16

| **VLAN** | **IP Address/Range** |
| -------- | -------------------- |
| VLAN 10  | 192.168.10.0/24      |
| VLAN 20  | 192.168.20.0/24      |
| VLAN 30  | 192.168.30.0/24      |

---
### Switched Network Section

| **Device** | **IP Address/Range** | **Notes**        |
| ---------- | -------------------- | ---------------- |
| SW-1       | 192.168.50.2         | Connected to R2  |
| SW-2       | 192.168.50.2         | Connected to R2  |
| R2         | 192.168.50.2 (fa0/1) | Router for VLANs |

---
### Overall Network Section

| **Network Address** | **Subnet Mask** |
| ------------------- | --------------- |
| 192.168.0.0/16      | 255.255.0.0     |

| Network Section         | Device/Subnet       | IP Address/Range                               | VLAN | Notes                             |
| ----------------------- | ------------------- | ---------------------------------------------- | ---- | --------------------------------- |
| internal Interface      | EDGE-ROUTER         | 192.168.60.1/30                                | -    | Connected to internet             |
| Interface with E-Router | FW-EXTERNAL         | 192.168.60.2/30                                | -    | First line of defense             |
| [DMZ ]                  | Zoned subnet        | 192.168.13.0/24                                | -    | Default Gateway: 192.168.13.1     |
| DMZ Web  server         | WEB-SERVER          | 192.168.13.5                                   | -    | dmz web server                    |
| DMZ DNS server          | External-DNS-SERVER | 192.168.13.10                                  | -    | DNS server for public interaction |
| Network  Firewall FW1,  | FW1,                | 192.168.13.1/24(DMZ)                           | -    | Filters requests to DMZ           |
| Data Center FW2         | FW-DATA ASA1        | 192.168.5.1/24(CENTER) 192.168.6.2(rootRouter) | -    | Protects internal servers         |
| DMZ SWITCH              | DMZ-Switch          | 192.168.50.1                                   | -    | Main internal switch              |
| DATA  Servers           | DB-Server           | 192.168.5.15                                   | -    |                                   |
|                         | FTP Server          | 192.168.5.10                                   | -    |                                   |
|                         | AD                  | 192.168.5..5                                   | -    |                                   |
| VLANs                   | VLAN 10             | 192.168.10.0/24                                | 10   |                                   |
|                         | VLAN 20             | 192.168.20.0/24                                | 20   |                                   |
|                         | VLAN 30             | 192.168.30.0/24                                | 30   |                                   |
| Switched Network        | SW-0                | VLAN 10,20,30, 998                             | -    | Connected to R2                   |
| Switched Network        | SW-1                | VLAN 10,20,30, 998                             | -    | Connected to R2                   |
|                         |                     |                                                | -    |                                   |
|                         |                     |                                                | -    |                                   |

----------------------------------------------------------------
## Initial Configuration
To-be-added:
- Basic configuration for all routers, eg: name, hostname, password, vty lines, banner, encryption, ssh
- Basic configuration for all switches, eg: name, hostname, password, banner, encryption, ssh
- Ip Addressing for all PCs -> DHCP WHERE DHCP? DC (AD) static()
- VLANS, trunks, intervlan routing, router on a stick, router config 
- VLAN TROUBLESHOOT
- Security Configuration, ACL, Firewall rules, Secure configs for router and switches
- report for effectiveness
- Test network, performance, functionality security assesment, Presentation
- Slides 
 