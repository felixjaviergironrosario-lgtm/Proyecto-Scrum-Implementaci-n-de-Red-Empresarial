```
SW4#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up      
GigabitEthernet0/1     unassigned      YES unset  up                    up      
GigabitEthernet0/2     unassigned      YES unset  up                    up      
GigabitEthernet0/3     unassigned      YES unset  up                    up      
Port-channel1          unassigned      YES unset  up                    up      
SW4#show interfaces trunk

Port        Mode             Encapsulation  Status        Native vlan
Gi0/0       on               802.1q         trunking      1
Po1         on               802.1q         trunking      1

Port        Vlans allowed on trunk
Gi0/0       10,20,30,40
Po1         10,20,30,40

Port        Vlans allowed and active in management domain
Gi0/0       10,20,30,40
Po1         10,20,30,40

Port        Vlans in spanning tree forwarding state and not pruned
Gi0/0       10,20,30,40
Po1         10,20,30,40
SW4#show etherchannel summary
Flags:  D - down        P - bundled in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      N - not in use, no aggregation
        f - failed to allocate aggregator

        M - not in use, minimum links not met
        m - not in use, port not aggregated due to minimum links not met
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port

        A - formed by Auto LAG


Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
1      Po1(SU)         LACP      Gi0/1(P)    Gi0/2(P)    

SW4#show vtp status
VTP Version capable             : 1 to 3
VTP version running             : 3
VTP Domain Name                 : producto1
VTP Pruning Mode                : Disabled
VTP Traps Generation            : Disabled
Device ID                       : 5254.000e.8000

Feature VLAN:
--------------
VTP Operating Mode                : Primary Server
Number of existing VLANs          : 10
Number of existing extended VLANs : 0
Maximum VLANs supported locally   : 4096
Configuration Revision            : 2
Primary ID                        : 5254.000e.8000
Primary Description               : SW4
MD5 digest                        : 0x41 0xD0 0x70 0x4F 0x13 0xE8 0x17 0x9C 
                                    0xB2 0x2D 0x42 0xD8 0x7C 0xD0 0x18 0xA0 


Feature MST:
--------------
VTP Operating Mode                : Transparent
          
          
Feature UNKNOWN:
--------------
VTP Operating Mode                : Transparent
```