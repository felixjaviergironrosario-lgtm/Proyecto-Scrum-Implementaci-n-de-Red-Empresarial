```
SW5#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up      
GigabitEthernet0/1     unassigned      YES unset  up                    up      
GigabitEthernet0/2     unassigned      YES unset  up                    up      
GigabitEthernet0/3     unassigned      YES unset  up                    up      
Port-channel1          unassigned      YES unset  up                    up      
SW5#show interfaces trunk

Port        Mode             Encapsulation  Status        Native vlan
Po1         on               802.1q         trunking      1

Port        Vlans allowed on trunk
Po1         10,20,30,40

Port        Vlans allowed and active in management domain
Po1         20,30

Port        Vlans in spanning tree forwarding state and not pruned
Po1         20,30
SW5#show etherchannel summary
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

SW5#show vtp status
VTP Version capable             : 1 to 3
VTP version running             : 1
VTP Domain Name                 : producto1
VTP Pruning Mode                : Disabled
VTP Traps Generation            : Disabled
Device ID                       : 5254.0012.8000
Configuration last modified by 0.0.0.0 at 11-21-26 17:39:03

Feature VLAN:
--------------
VTP Operating Mode                : Client
Maximum VLANs supported locally   : 1005
Number of existing VLANs          : 7
Configuration Revision            : 2
MD5 digest                        : 0x9C 0x60 0x5D 0x34 0x83 0x01 0xC8 0x0D 
                                    0x70 0x52 0x1E 0x4D 0xC4 0x32 0xC6 0x7E 
*** MD5 digest checksum mismatch on trunk: Po1 ***
```