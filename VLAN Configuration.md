This report aims to document VLANs Creation, Inter-Vlan Routing, and IP Adressing.

### **Create VLANs (VLANs 10 and 30) on  Switch0**

| **Description**                 | **Command**                       |
| ------------------------------- | --------------------------------- |
| Enter global configuration mode | Switch# conf t                    |
| Create VLAN 10                  | Switch(config)# vlan 10           |
| Give a name to VLAN 10          | Switch(config-vlan)# name Library |
| Create VLAN 20                  | Switch(config-vlan)# vlan 30      |
| Give a name to VLAN 20          | Switch(config-vlan)# name Labs    |
| Exit the VLAN config. mode      | Switch(config-vlan)# exit         |
| Check if the VLANs were created | Switch # show vlan brief          |
### **Assign the VLANs to switch ports**

# Switch 0
### Create VLAN 20
```
switch(config)# interface fa0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config-if)# exit

Switch0config#interface fa0/2
Switch0(config-if)#switchport mode access
Switch(config-if)#switchport access vlan20
exit
```
Assign port 1, 2 to VLAN 10
```
witch(config)# interface fa0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)# exit

Switch0config#interface fa0/2
Switch0(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
exit
```

Assign port 4 and 5 to VLAN 30 
```
interface fa0/4
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 30
Switch(config-if)#exit

interface fa0/5
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 30
exit
```
# SWITCH1
### **Create VLAN 20 on  Switch1**
| **Description**                 | **Command**                         |
| ------------------------------- | ----------------------------------- |
| Enter global configuration mode | Switch# conf t                      |
| Create VLAN 10                  | Switch(config)# vlan 20             |
| Give a name to VLAN 10          | Switch(config-vlan)# name classroom |
| Create VLAN 20                  | Switch(config-vlan)# vlan 20        |
| Check if the VLAN was created   | Switch # show vlan brief            |
enable VLAN  port 10 to enable Trunking, and set port mode to trunk

#  Trunking 
-----------------------
VLAN Trunking enables multiple VLANs to be carried over a single physical link between switches. This allows you to extend VLANs across multiple switches in a network. we're enabling it here to extend VLAN10 to Switch1.
- Switch0 has two devices in VLAN 10
- Switch1 has one device in VLAN 10
- A trunk link between Switch0 and Switch1 allows VLAN 10 traffic to pass between the switches

## switch0
### Enable Trunking on Switch0 fa0/7 Port
allows Device on Switch0 FA0/7 to connect to VLAN20 on Switch1
```
Switch0(config)#interface f0/7
Switch0(config-if)#switchport access vlan 20
Switch0(config-if)#switchport mode trunk
Switch1(config-if)#switchport trunk allowed vlan add 20
```

## Switch1
### Enable Trunking  on Switch1 FA0/7 Port

Enable VLAN 10 & assign fa0/7 to VLAN 10 Through Trunking for switch1
```
Switch1(config)#interface FastEthernet 0/7 
Switch1(config-if)#switchport access vlan 10
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# switchport trunk allowed vlan add 10
```
### Review Configuration: 
```
Switch0#show vlan brief

VLAN Name Status Ports
---- -------------------------------- --------- -------------------------------

1 default active Fa0/8, Fa0/9, Fa0/10, Fa0/11

Fa0/12, Fa0/13, Fa0/14, Fa0/15

Fa0/16, Fa0/17, Fa0/18, Fa0/19

Fa0/20, Fa0/21, Fa0/22, Fa0/23

Fa0/24, Gig0/2

10 Library active Fa0/1, Fa0/5

20 classroom active Fa0/6

30 Labs active Fa0/2, Fa0/3
```

# Router on a stick Configuration

ROAS or Router on a Stick, which is a method for inter-VLAN routing using a single physical interface divided into subinterfaces for each Vlan.
## rootRouter Configuration
```
Router(config)# interface gigabitethernet1/0 
Router(config)# interface gigabitethernet1/0.10 
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-if)# no shutdown
Router(config)# interface gigabitethernet1/0.20 
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router2(config)# interface gigabitethernet1/0.30
Router(config-subif)# encapsulation dot1Q 30
Router(config-subif)# ip address 192.168.30.1 255.255.255.0
```
