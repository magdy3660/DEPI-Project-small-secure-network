# Packet Tracer Implementation

![[network topology.png]]

# Router Configuration
EdgeRouter
- edge connects to the internet and the internal network.
- Interface `GigabitEthernet0/0` connects to the external network (ISP).
- Interface `GigabitEthernet0/1` connects to the internal network.

- **Interface GigabitEthernet0/0**: This is the interface through which the router connects to the outside (public) network (typically the internet). The router will use the IP address assigned to this interface as the public IP for outgoing traffic.
```
Router1>enable
Router1#configure terminal
Router1(config)#hostname rootRouter

rootRouter(config)#interface GigabitEthernet0/0
rootRouter(config-if)ip address dhcp 
rootRouter(config-if)#no shutdown
rootRouter(config-if)#exit

rootRouter(config)#interface GigabitEthernet0/1
rootRouter(config-if)#ip address 192.168.1.1 255.255.255.0     ! Private IP
rootRouter(config-if)#no shutdown
rootRouter(config-if)#exit
```

- **Inside Source**: It tells the router to translate the source IP addresses of packets originating from the internal network (marked as "inside") using the access list defined as `1`.
- **Overload**: This allows multiple internal IP addresses to be translated to a single public IP address. This is commonly known as Port Address Translation (PAT), enabling many internal devices to share a single public IP address.
```
Router1(config)#ip nat inside source list 1 interface GigabitEthernet0/0 overload
Router1(config)#access-list 1 permit 192.168.1.0 0.0.0.255
Router1(config)#ip routing
Router1(config)#end
Router1#write memory
Router1#copy running-config startup-config
```

# Global Configurations
Basic configurations for Router1, Router2, and Router3 non re-iterated
```
Router(config)#hostname Router-name
R2(config)#banner motd $
-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
UNAUTHORIZED ACCESS IS PROHIBITED
-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
```
## Switch Configuration

### Switch3 (DMZ Switch)
```
Switch3>enable 
Switch3#configure terminal 
Switch3(config)#hostname dmzSwitch 
Switch3(config)#enable secret 2435fdfvge 
Switch3(config)#end
Switch3#copy running-config startup-config
```

### Switch0

```
SW1(config)#banner motd $
-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-
UNAUTHORIZED ACCESS IS PROHIBITED
-=-=-=-=-=-=-
SW1#copy running-config startup-config
```
### Switch1


### Firewalls

### FW1 Firewall
```
ASAFirewall>enable ASAFirewall#configure terminal 
ASAFirewall(config)#hostname FW1
ASAFirewall(config)#interface GigabitEthernet1/1
ASA1(config-if)#ip address 192.168.13.1 255.255.255.0
ASAFirewall(config)#default-gateway 192.168.60.1
ASAFirewall(config-if)#nameif outside
ASAFirewall(config)#interface GigabitEthernet1/3
ASAFirewall(config-if)#nameif DMZ 
ASAFirewall(config-if)#security-level 0 

```
### FW2 Firewall
```
ASAFirewall>enable ASAFirewall#configure terminal 
ASAFirewall(config)#hostname FW2 
ASAFirewall(config)#interface GigabitEthernet1/1
ASAFirewall(config-if)#nameif outside
ASAFirewall(config-if)#security-level 0
ASAFirewall(config-if)#ip address 192.168.5.1 255.255.255.0
ASAFirewall(config)#default-gateway 192.168.6.2

ASAFirewall(config)#interface GigabitEthernet1/3
ASAFirewall(config-if)#nameif DMZ 
ASAFirewall(config-if)#security-level 0 
ASAFirewall(config-if)#ip address 192.168.13.1 255.255.255.0
```

sw1  Banner Configuration
```


```
Switch1 hostname Configuration and encryption
```
Switch(config)#hostname SW1
SW1(config)#enable secret cisco
SW1(config)#enable password notcisco
SW1(config)#service password-encryption
```

##  Conclusion

This network design provides a robust, secure, and scalable infrastructure. It effectively separates external-facing services from internal resources while allowing for efficient inter-VLAN communication and centralized management of critical services. The use of various routing protocols, security measures, and WAN technologies ensures optimal performance and protection of network resources.
```

## FIREWALL CONFIG
```
FWE(config-if)#route ROUTERINSIDE 0.0.0.0 0.0.0.0 192.168.5.1
FW2(config-if)#route ExternalConnections 0.0.0.0 0.0.0.0 192.168.6.1

FW(config)#
```

ROUTER2
route any request to 192.168.5.0/24 through the firewall at 192.168.6.2 interface related to me
```
ip route 192.168.5.0 255.255.255.0 192.168.6.2
```
### FTP Configuration
