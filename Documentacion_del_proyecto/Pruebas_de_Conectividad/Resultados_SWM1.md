```SWM1#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up      
GigabitEthernet0/1     172.20.10.162   YES TFTP   up                    up      
GigabitEthernet0/2     172.20.10.1     YES TFTP   up                    up      
GigabitEthernet0/3     unassigned      YES unset  up                    up      
Vlan50                 172.20.9.129    YES TFTP   up                    up      
Vlan60                 172.20.9.1      YES TFTP   up                    up      
SWM1#show ipv6 interface brief
GigabitEthernet0/0     [up/up]
    unassigned
GigabitEthernet0/1     [up/up]
    FE80::5054:FF:FE1E:3443
    2001:25:20:9::2
GigabitEthernet0/2     [up/up]
    FE80::5054:FF:FE1C:F287
    2001:25:20:4::1
GigabitEthernet0/3     [up/up]
    unassigned
Vlan50                 [up/up]
    FE80::5054:FF:FE05:8032
    2001:25:20:3::1
Vlan60                 [up/up]
    FE80::5054:FF:FE05:803C
    2001:25:20:2::1
SWM1#show ip route 
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is not set

      172.20.0.0/16 is variably subnetted, 12 subnets, 7 masks
S        172.20.0.0/21 [1/0] via 172.20.10.161
S        172.20.8.0/24 [1/0] via 172.20.10.161
C        172.20.9.0/25 is directly connected, Vlan60
L        172.20.9.1/32 is directly connected, Vlan60
C        172.20.9.128/25 is directly connected, Vlan50
L        172.20.9.129/32 is directly connected, Vlan50
C        172.20.10.0/26 is directly connected, GigabitEthernet0/2
L        172.20.10.1/32 is directly connected, GigabitEthernet0/2
S        172.20.10.64/27 [1/0] via 172.20.10.161
          
SWM1#how ipv6 route
      ^
% Invalid input detected at '^' marker.

SWM1#ping 172.20.10.161
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.20.10.161, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
SWM1#show ipv6 route   
IPv6 Routing Table - default - 13 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, R - RIP, I1 - ISIS L1, I2 - ISIS L2
       IA - ISIS interarea, IS - ISIS summary, D - EIGRP, EX - EIGRP external
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       RL - RPL, O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1
       OE2 - OSPF ext 2, ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2
       la - LISP alt, lr - LISP site-registrations, ld - LISP dyn-eid
       lA - LISP away, a - Application
S   2001:25:20::/64 [1/0]
     via 2001:25:20:9::1
S   2001:25:20:1::/64 [1/0]
     via 2001:25:20:9::1
C   2001:25:20:2::/64 [0/0]
     via Vlan60, directly connected
L   2001:25:20:2::1/128 [0/0]
     via Vlan60, receive
C   2001:25:20:3::/64 [0/0]
     via Vlan50, directly connected
L   2001:25:20:3::1/128 [0/0]
     via Vlan50, receive
C   2001:25:20:4::/64 [0/0]
     via GigabitEthernet0/2, directly connected
L   2001:25:20:4::1/128 [0/0]
     via GigabitEthernet0/2, receive
S   2001:25:20:5::/64 [1/0]
     via 2001:25:20:9::1
S   2001:25:20:6::/64 [1/0]
     via 2001:25:20:9::1
C   2001:25:20:9::/64 [0/0]
     via GigabitEthernet0/1, directly connected
L   2001:25:20:9::2/128 [0/0]
     via GigabitEthernet0/1, receive
L   FF00::/8 [0/0]
     via Null0, receive
SWM1#ping ipv6 2001:25:20:9::1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:25:20:9::1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
SWM1#
```