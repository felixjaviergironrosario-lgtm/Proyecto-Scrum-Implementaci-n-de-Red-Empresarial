```
SW#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up      
GigabitEthernet0/1     unassigned      YES unset  up                    up      
GigabitEthernet0/2     unassigned      YES unset  up                    up      
GigabitEthernet0/3     unassigned      YES unset  up                    up      
SW#show interfaces status

Port      Name               Status       Vlan       Duplex  Speed Type 
Gi0/0                        connected    70         a-full   auto RJ45
Gi0/1                        connected    70         a-full   auto RJ45
Gi0/2                        connected    1          a-full   auto RJ45
Gi0/3                        connected    1          a-full   auto RJ45
SW#show interfaces trunk
SW#show vtp status
VTP Version capable             : 1 to 3
VTP version running             : 1
VTP Domain Name                 : 
VTP Pruning Mode                : Disabled
VTP Traps Generation            : Disabled
Device ID                       : 5254.0014.8000
Configuration last modified by 0.0.0.0 at 11-21-26 17:39:10
Local updater ID is 0.0.0.0 (no valid interface found)

Feature VLAN:
--------------
VTP Operating Mode                : Server
Maximum VLANs supported locally   : 1005
Number of existing VLANs          : 6
Configuration Revision            : 1
MD5 digest                        : 0xD5 0x6F 0x98 0xDD 0x1E 0x55 0xD9 0x59 
                                    0xD2 0xDA 0x29 0x2B 0x1C 0xDE 0x5E 0x06 
```