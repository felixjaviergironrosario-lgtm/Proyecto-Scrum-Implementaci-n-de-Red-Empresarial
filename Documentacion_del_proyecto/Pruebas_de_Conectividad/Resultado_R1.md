```
R1#show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0         172.20.10.161   YES TFTP   up                    up      
GigabitEthernet0/1         unassigned      YES TFTP   up                    up      
GigabitEthernet0/1.10      172.20.8.1      YES TFTP   up                    up      
GigabitEthernet0/1.20      172.20.0.1      YES TFTP   up                    up      
GigabitEthernet0/1.30      172.20.10.65    YES TFTP   up                    up      
GigabitEthernet0/1.40      172.20.10.97    YES TFTP   up                    up      
GigabitEthernet0/2         unassigned      YES TFTP   administratively down down    
GigabitEthernet0/3         unassigned      YES TFTP   administratively down down    
R1#show ipv6 interface brief
GigabitEthernet0/0     [up/up]
    FE80::5054:FF:FE08:D03B
    2001:25:20:9::1
GigabitEthernet0/1     [up/up]
    unassigned
GigabitEthernet0/1.10  [up/up]
    FE80::5054:FF:FE17:6E3F
    2001:25:20:1::1
GigabitEthernet0/1.20  [up/up]
    FE80::5054:FF:FE17:6E3F
    2001:25:20::1
GigabitEthernet0/1.30  [up/up]
    FE80::5054:FF:FE17:6E3F
    2001:25:20:5::1
GigabitEthernet0/1.40  [up/up]
    FE80::5054:FF:FE17:6E3F
    2001:25:20:6::1
GigabitEthernet0/2     [administratively down/down]
    unassigned
GigabitEthernet0/3     [administratively down/down]
    unassigned
R1#show ip route 
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

      172.20.0.0/16 is variably subnetted, 13 subnets, 7 masks
C        172.20.0.0/21 is directly connected, GigabitEthernet0/1.20
L        172.20.0.1/32 is directly connected, GigabitEthernet0/1.20
C        172.20.8.0/24 is directly connected, GigabitEthernet0/1.10
L        172.20.8.1/32 is directly connected, GigabitEthernet0/1.10
S        172.20.9.0/25 [1/0] via 172.20.10.162
S        172.20.9.128/25 [1/0] via 172.20.10.162
S        172.20.10.0/26 [1/0] via 172.20.10.162
C        172.20.10.64/27 is directly connected, GigabitEthernet0/1.30
L        172.20.10.65/32 is directly connected, GigabitEthernet0/1.30
          
R1#how ipv6 route
    ^
% Invalid input detected at '^' marker.

R1#ping 172.20.10.162
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.20.10.162, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms
R1#show ipv6 route   
IPv6 Routing Table - default - 14 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
       B - BGP, HA - Home Agent, MR - Mobile Router, R - RIP
       H - NHRP, I1 - ISIS L1, I2 - ISIS L2, IA - ISIS interarea
       IS - ISIS summary, D - EIGRP, EX - EIGRP external, NM - NEMO
       ND - ND Default, NDp - ND Prefix, DCE - Destination, NDr - Redirect
       RL - RPL, O - OSPF Intra, OI - OSPF Inter, OE1 - OSPF ext 1
       OE2 - OSPF ext 2, ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2
       la - LISP alt, lr - LISP site-registrations, ld - LISP dyn-eid
       lA - LISP away, a - Application
C   2001:25:20::/64 [0/0]
     via GigabitEthernet0/1.20, directly connected
L   2001:25:20::1/128 [0/0]
     via GigabitEthernet0/1.20, receive
C   2001:25:20:1::/64 [0/0]
     via GigabitEthernet0/1.10, directly connected
L   2001:25:20:1::1/128 [0/0]
     via GigabitEthernet0/1.10, receive
S   2001:25:20:2::/64 [1/0]
     via 2001:25:20:9::2
S   2001:25:20:3::/64 [1/0]
     via 2001:25:20:9::2
S   2001:25:20:4::/64 [1/0]
     via 2001:25:20:9::2
C   2001:25:20:5::/64 [0/0]
     via GigabitEthernet0/1.30, directly connected
L   2001:25:20:5::1/128 [0/0]
     via GigabitEthernet0/1.30, receive
C   2001:25:20:6::/64 [0/0]
     via GigabitEthernet0/1.40, directly connected
L   2001:25:20:6::1/128 [0/0]
     via GigabitEthernet0/1.40, receive
C   2001:25:20:9::/64 [0/0]
     via GigabitEthernet0/0, directly connected
L   2001:25:20:9::1/128 [0/0]
     via GigabitEthernet0/0, receive
L   FF00::/8 [0/0]
     via Null0, receive
R1#ping ipv6 2001:25:20:9::2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:25:20:9::2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```