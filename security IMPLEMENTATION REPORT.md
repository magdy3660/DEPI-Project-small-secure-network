### Router Configuration

**1. Disable Unnecessary Services:**

```
interface GigabitEthernet0/0
 no service telnet
 no service ftp
 no service http
```
## Edge Router Configuration
To control the traffic from the internal network to the internet and vice versa.
ACLs
```
access-list ACL-IN deny ip any any
access-list ACL-IN permit tcp any any eq 80 
access-list ACL-IN permit tcp any any eq 443
```
Apply the ACL to the inbound traffic:
```
interface eth0
ip access-group ACL-IN in
exit
```

### Firewall 1 (FW1) Configuration

1. **Stateful Packet Inspection**: Ensure that only established connections are allowed.
2. **Basic Rules**: Allow traffic from the LAN to the WAN, but restrict external access.

#### Configuration Steps:


2. Set  allow outgoing traffic and deny incoming traffic.
```
access-list OUTGOING permit ip any any access-list INCOMING deny ip any any
```
**2. Create Permit Rules:**
```
access-list 100 permit tcp any any eq 80
access-list 100 permit tcp any any eq 22
access-list 100 permit tcp any any eq 443
access-list 100 permit tcp any any eq 21
access-list 100 permit udp any any eq 53
```
**3. Apply Rules to Interfaces:**
```
interface GigabitEthernet1/2
 ip access-group 100 inside
```

```
ip access-group OUTGOING out
```
## Protect Vty Line and enable ssh

```
line vty 0 4
login local
transport input ssh
```

```
**4. Configure Logging:**

```
### RSA Key Generation Commands

1. **Access the device CLI** (e.g., via console or SSH).
    
2. **Enter Global Configuration Mode**:
```
crypto key generate rsa
How many bits in the modulus [512]: 2048
```
Enable SSH version 2
```
ip ssh version 2
```

```
ip domain-name project.depi
```
set a timeout and authentication retries for SSH sessions:
```
ip ssh time-out 60
ip ssh authentication-retries 3
```
**Create User Accounts**  for management access:
```
username admin privilege 15 secret your_password_here
```

rootRouter Configuration

Allow a maximum of 3 devices per VLAN
```
`switchport port-security maximum 3`
```
 Disable unused ports
 ```
interface range FastEthernet10/25 - 48
 shutdown
```



## Week 3: Network Security Implementation

### 1. Firewall Configuration (FW1 and FW2)
- [ ] Configure FW1 (previously EdgeRouter) with DMZ and rootRouter links
- [ ] Set up FW2 for data center protection
- [ ] Implement basic firewall rules on both FW1 and FW2
- [ ] Configure NAT/PAT on FW1 for internet access

### 2. VLAN Security
- [ ] Implement port security on Switch0 and Switch1
- [ ] Configure VLAN access control lists (VACLs) on Switch0 and Switch1
- [ ] Set up inter-VLAN routing security on rootRouter

### 3. Access Control Lists (ACLs)
- [ ] Create and apply standard and extended ACLs on rootRouter
- [ ] Implement ACLs on FW1 for DMZ protection
- [ ] Configure ACLs on FW2 for data center security

### 4. Additional Security Measures
- [ ] Enable SSH for secure remote management on all devices
- [ ] Implement DHCP snooping on Switch0 and Switch1
- [ ] Configure Dynamic ARP Inspection (DAI) on Switch0 and Switch1
- [ ] Set up IP Source Guard on access ports of Switch0 and Switch1

### 5. Documentation
- Document all security configurations
- - Create a comprehensive security policy document
- Write a report on the effectiveness of implemented security measures