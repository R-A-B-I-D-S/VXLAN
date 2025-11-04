# Проект на тему "Масштабирование L2-L3 сегмента между двумя центрами обработки данных (ЦОД) с помощью технологии EVPN-VXLAN


## Задача:
   - Спроектировать сеть ЦОД с возможностью масштабирования и обеспечением отказоустойчивости на уровне L2-L3.


## Используемые технологии:
   - VxLAN, EVPN, eBGP, VXLAN Multihoming, LACP.


## Схема сети:
![](img/Schema.jpg)


## Конфигурации устройств:
  - [spine-1](Config/spine-1.cfg)
  - [spine-2](Config/spine-2.cfg)
  - [spine-3](Config/spine-3.cfg)
  - [spine-4](Config/spine-4.cfg)
  - [leaf-1](Config/leaf-1.cfg)
  - [leaf-2](Config/leaf-2.cfg)
  - [leaf-7](Config/leaf-7.cfg)
  - [leaf-8](Config/leaf-8.cfg)
  - [border-leaf-1](Config/border-leaf-1.cfg)
  - [border-leaf-2](Config/border-leaf-2.cfg)
  - [border-leaf-3](Config/border-leaf-3.cfg)
  - [border-leaf-4](Config/border-leaf-4.cfg)
  - [server-1](Config/server-1.cfg)
  - [server-2](Config/server-2.cfg)
  - [server-3](Config/server-3.cfg)
  - [server-4](Config/server-4.cfg)





- Leaf-1
```
Leaf-1#show vxlan vtep 
Remote VTEPS for Vxlan1:

VTEP          Tunnel Type(s)
------------- --------------
1.1.2.3       unicast       
1.1.2.4       unicast       
1.1.2.5       unicast, flood
1.1.2.6       unicast, flood
1.1.2.8       unicast       

Total number of remote VTEPS:  5
Leaf-1#
Leaf-1#show bgp evpn 
BGP routing table information for VRF default
Router identifier 1.1.1.1, local AS number 65100
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 -                     -       -       0       i
 * >      RD: 1.1.2.1:1 auto-discovery 0000:0000:0000:0000:1201
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.3:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.3:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.4:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.4:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.2.3:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.2.3:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.2.4:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65200 65101 i
 *  ec    RD: 1.1.2.4:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.5:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.5:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.6:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.6:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.2.5:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.2.5:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.2.6:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.2.6:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.7:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.7:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.1.8:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.8:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.2.7:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.2.7:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.2.8:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.2.8:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b 10.0.20.2
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b 10.0.20.2
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 * >      RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc
                                 -                     -       -       0       i
 * >      RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.5
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.5
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb
                                 1.1.2.3               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.4:20 mac-ip 5001.00ff.ebdb
                                 1.1.2.4               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.4:20 mac-ip 5001.00ff.ebdb
                                 1.1.2.4               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 1.1.2.3               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.4:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 1.1.2.4               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.4:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 1.1.2.4               -       100     0       65200 65101 i
 * >      RD: 1.1.1.1:10 imet 1.1.2.1
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.3:20 imet 1.1.2.3
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.3:20 imet 1.1.2.3
                                 1.1.2.3               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.4:20 imet 1.1.2.4
                                 1.1.2.4               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.4:20 imet 1.1.2.4
                                 1.1.2.4               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.5:10 imet 1.1.2.5
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.5:10 imet 1.1.2.5
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.6:10 imet 1.1.2.6
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.6:10 imet 1.1.2.6
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.7:20 imet 1.1.2.7
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.7:20 imet 1.1.2.7
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.1.8:20 imet 1.1.2.8
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.8:20 imet 1.1.2.8
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 * >      RD: 1.1.2.1:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.1
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.2.3:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.3
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.2.3:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.3
                                 1.1.2.3               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.2.4:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.4
                                 1.1.2.4               -       100     0       65200 65101 i
 *  ec    RD: 1.1.2.4:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.4
                                 1.1.2.4               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.2.5:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.5
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.2.5:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.5
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.2.6:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.6
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.2.6:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.6
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.2.7:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.7
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.2.7:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.7
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.2.8:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.8
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.2.8:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.8
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
Leaf-1#
Leaf-1#show ip route vrf all 

VRF: default
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 C        1.1.1.1/32 is directly connected, Loopback0
 B E      1.1.1.3/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      1.1.1.4/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      1.1.1.5/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      1.1.1.6/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      1.1.1.7/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      1.1.1.8/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 C        1.1.2.1/32 is directly connected, Loopback1
 B E      1.1.2.3/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      1.1.2.4/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      1.1.2.5/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      1.1.2.6/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      1.1.2.7/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      1.1.2.8/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      2.2.2.1/32 [200/0] via 172.16.1.1, Ethernet3
 B E      2.2.2.2/32 [200/0] via 172.16.2.1, Ethernet4
 B E      2.2.2.3/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      2.2.2.4/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      3.3.3.1/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      3.3.3.2/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      3.3.3.3/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 B E      3.3.3.4/32 [200/0] via 172.16.1.1, Ethernet3
                             via 172.16.2.1, Ethernet4
 C        172.16.1.0/31 is directly connected, Ethernet3
 C        172.16.2.0/31 is directly connected, Ethernet4
 B E      172.16.7.0/31 [200/0] via 172.16.1.1, Ethernet3
                                via 172.16.2.1, Ethernet4
 B E      172.16.8.0/31 [200/0] via 172.16.1.1, Ethernet3
                                via 172.16.2.1, Ethernet4
 B E      194.50.50.0/31 [200/0] via 172.16.1.1, Ethernet3
                                 via 172.16.2.1, Ethernet4


VRF: VRF1
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      10.0.10.2/32 [200/0] via VTEP 1.1.2.5 VNI 999 router-mac 50:01:00:a1:60:83 local-interface Vxlan1
                               via VTEP 1.1.2.6 VNI 999 router-mac 50:01:00:ca:7a:8d local-interface Vxlan1
 B E      10.0.10.5/32 [200/0] via VTEP 1.1.2.6 VNI 999 router-mac 50:01:00:ca:7a:8d local-interface Vxlan1
 C        10.0.10.0/24 is directly connected, Vlan10
 B E      10.0.20.2/32 [200/0] via VTEP 1.1.2.8 VNI 999 router-mac 50:01:00:9b:56:6c local-interface Vxlan1
 B E      10.0.20.3/32 [200/0] via VTEP 1.1.2.4 VNI 999 router-mac 50:01:00:a7:87:1f local-interface Vxlan1
                               via VTEP 1.1.2.3 VNI 999 router-mac 50:01:00:ff:ea:cb local-interface Vxlan1

Leaf-1#
Leaf-1#show ip bgp summary 
BGP summary information for VRF default
Router identifier 1.1.1.1, local AS number 65100
Neighbor Status Codes: m - Under maintenance
  Neighbor   V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  2.2.2.1    4 65200            440       360    0    0 03:04:10 Estab   22     22
  2.2.2.2    4 65200            442       388    0    0 03:04:10 Estab   22     22
  172.16.1.1 4 65200            245       241    0    0 03:04:15 Estab   22     22
  172.16.2.1 4 65200            250       238    0    0 03:04:16 Estab   22     22
```


- Leaf-2
```
Leaf-2#show vxlan  vtep
Remote VTEPS for Vxlan1:

VTEP          Tunnel Type(s)
------------- --------------
1.1.2.3       unicast       
1.1.2.5       flood, unicast
1.1.2.6       flood, unicast
1.1.2.8       unicast       

Total number of remote VTEPS:  4
Leaf-2#
Leaf-2#show bgp  evpn
BGP routing table information for VRF default
Router identifier 1.1.1.2, local AS number 65100
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 1.1.1.2:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 -                     -       -       0       i
 * >      RD: 1.1.2.2:1 auto-discovery 0000:0000:0000:0000:1201
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.3:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.3:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.4:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.4:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.2.3:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.2.3:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.2.4:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65200 65101 i
 *  ec    RD: 1.1.2.4:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.5:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.5:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.6:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.6:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.2.5:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.2.5:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.2.6:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.2.6:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.7:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.7:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.1.8:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.8:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.2.7:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.2.7:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.2.8:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.2.8:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b 10.0.20.2
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b 10.0.20.2
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 * >      RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc
                                 -                     -       -       0       i
 * >      RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.5
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.5
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb
                                 1.1.2.3               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 1.1.2.3               -       100     0       65200 65101 i
 * >      RD: 1.1.1.2:10 imet 1.1.2.2
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.3:20 imet 1.1.2.3
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.3:20 imet 1.1.2.3
                                 1.1.2.3               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.4:20 imet 1.1.2.4
                                 1.1.2.4               -       100     0       65200 65101 i
 *  ec    RD: 1.1.1.4:20 imet 1.1.2.4
                                 1.1.2.4               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.1.5:10 imet 1.1.2.5
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.5:10 imet 1.1.2.5
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.6:10 imet 1.1.2.6
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.1.6:10 imet 1.1.2.6
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.1.7:20 imet 1.1.2.7
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.7:20 imet 1.1.2.7
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.1.8:20 imet 1.1.2.8
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.1.8:20 imet 1.1.2.8
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 * >      RD: 1.1.2.2:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.2
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.2.3:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.3
                                 1.1.2.3               -       100     0       65200 65101 i
 *  ec    RD: 1.1.2.3:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.3
                                 1.1.2.3               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.2.4:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.4
                                 1.1.2.4               -       100     0       65200 65101 i
 *  ec    RD: 1.1.2.4:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.4
                                 1.1.2.4               -       100     0       65200 65101 i
 * >Ec    RD: 1.1.2.5:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.5
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.2.5:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.5
                                 1.1.2.5               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.2.6:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.6
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 *  ec    RD: 1.1.2.6:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.6
                                 1.1.2.6               -       100     0       65200 65101 65103 i
 * >Ec    RD: 1.1.2.7:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.7
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.2.7:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.7
                                 1.1.2.7               -       100     0       65200 65101 65103 65300 65104 i
 * >Ec    RD: 1.1.2.8:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.8
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
 *  ec    RD: 1.1.2.8:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.8
                                 1.1.2.8               -       100     0       65200 65101 65103 65300 65104 i
Leaf-2#
Leaf-2#show ip route vrf all

VRF: default
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 C        1.1.1.2/32 is directly connected, Loopback0
 B E      1.1.1.3/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      1.1.1.4/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      1.1.1.5/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      1.1.1.6/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      1.1.1.7/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      1.1.1.8/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 C        1.1.2.2/32 is directly connected, Loopback1
 B E      1.1.2.3/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      1.1.2.4/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      1.1.2.5/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      1.1.2.6/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      1.1.2.7/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      1.1.2.8/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      2.2.2.1/32 [200/0] via 172.16.3.1, Ethernet6
 B E      2.2.2.2/32 [200/0] via 172.16.4.1, Ethernet3
 B E      2.2.2.3/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      2.2.2.4/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      3.3.3.1/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      3.3.3.2/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      3.3.3.3/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 B E      3.3.3.4/32 [200/0] via 172.16.4.1, Ethernet3
                             via 172.16.3.1, Ethernet6
 C        172.16.3.0/31 is directly connected, Ethernet6
 C        172.16.4.0/31 is directly connected, Ethernet3
 B E      172.16.7.0/31 [200/0] via 172.16.4.1, Ethernet3
                                via 172.16.3.1, Ethernet6
 B E      172.16.8.0/31 [200/0] via 172.16.4.1, Ethernet3
                                via 172.16.3.1, Ethernet6
 B E      194.50.50.0/31 [200/0] via 172.16.4.1, Ethernet3
                                 via 172.16.3.1, Ethernet6


VRF: VRF1
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      10.0.10.2/32 [200/0] via VTEP 1.1.2.5 VNI 999 router-mac 50:01:00:a1:60:83 local-interface Vxlan1
                               via VTEP 1.1.2.6 VNI 999 router-mac 50:01:00:ca:7a:8d local-interface Vxlan1
 B E      10.0.10.5/32 [200/0] via VTEP 1.1.2.6 VNI 999 router-mac 50:01:00:ca:7a:8d local-interface Vxlan1
 C        10.0.10.0/24 is directly connected, Vlan10
 B E      10.0.20.2/32 [200/0] via VTEP 1.1.2.8 VNI 999 router-mac 50:01:00:9b:56:6c local-interface Vxlan1
 B E      10.0.20.3/32 [200/0] via VTEP 1.1.2.3 VNI 999 router-mac 50:01:00:ff:ea:cb local-interface Vxlan1

Leaf-2#
Leaf-2#show ip bgp summary 
BGP summary information for VRF default
Router identifier 1.1.1.2, local AS number 65100
Neighbor Status Codes: m - Under maintenance
  Neighbor   V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  2.2.2.1    4 65200            428       372    0    0 03:01:47 Estab   22     22
  2.2.2.2    4 65200            432       375    0    0 03:01:50 Estab   22     22
  172.16.3.1 4 65200            241       239    0    0 03:01:55 Estab   22     22
  172.16.4.1 4 65200            242       239    0    0 03:01:55 Estab   22     22
Leaf-2#
```


- Border-Leaf-1
```
Border-Leaf-1#show vxlan vtep 
Remote VTEPS for Vxlan1:

VTEP          Tunnel Type(s)
------------- --------------
1.1.2.1       unicast       
1.1.2.2       unicast       
1.1.2.5       unicast       
1.1.2.7       unicast, flood
1.1.2.8       unicast, flood

Total number of remote VTEPS:  5
Border-Leaf-1#
Border-Leaf-1#show bgp evpn 
BGP routing table information for VRF default
Router identifier 1.1.1.4, local AS number 65101
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.1.2:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.2:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.2.1:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.2.1:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.2.2:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.2.2:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65200 65100 i
 * >      RD: 1.1.1.4:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 -                     -       -       0       i
 * >      RD: 1.1.2.4:1 auto-discovery 0000:0000:0000:0000:3401
                                 -                     -       -       0       i
 * >      RD: 1.1.1.5:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65103 i
 * >      RD: 1.1.2.5:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65103 i
 * >      RD: 1.1.1.7:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.1.8:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.2.7:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.2.8:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.1.8:20 mac-ip 5001.0050.554b
                                 1.1.2.8               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.1.8:20 mac-ip 5001.0050.554b 10.0.20.2
                                 1.1.2.8               -       100     0       65103 65300 65104 i
 * >Ec    RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc
                                 1.1.2.2               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.2               -       100     0       65200 65100 i
 * >      RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.5               -       100     0       65103 i
 * >      RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.5               -       100     0       65103 i
 * >Ec    RD: 1.1.1.1:10 imet 1.1.2.1
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.1:10 imet 1.1.2.1
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.1.2:10 imet 1.1.2.2
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.2:10 imet 1.1.2.2
                                 1.1.2.2               -       100     0       65200 65100 i
 * >      RD: 1.1.1.4:20 imet 1.1.2.4
                                 -                     -       -       0       i
 * >      RD: 1.1.1.5:10 imet 1.1.2.5
                                 1.1.2.5               -       100     0       65103 i
 * >      RD: 1.1.1.7:20 imet 1.1.2.7
                                 1.1.2.7               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.1.8:20 imet 1.1.2.8
                                 1.1.2.8               -       100     0       65103 65300 65104 i
 * >Ec    RD: 1.1.2.1:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.1
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.2.1:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.1
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.2.2:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.2
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.2.2:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.2
                                 1.1.2.2               -       100     0       65200 65100 i
 * >      RD: 1.1.2.4:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.4
                                 -                     -       -       0       i
 * >      RD: 1.1.2.5:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.5
                                 1.1.2.5               -       100     0       65103 i
 * >      RD: 1.1.2.7:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.7
                                 1.1.2.7               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.2.8:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.8
                                 1.1.2.8               -       100     0       65103 65300 65104 i
Border-Leaf-1#
Border-Leaf-1#show ip route vrf all 

VRF: default
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      1.1.1.1/32 [200/0] via 172.16.7.1, Ethernet4
                             via 172.16.8.1, Ethernet5
 B E      1.1.1.2/32 [200/0] via 172.16.7.1, Ethernet4
                             via 172.16.8.1, Ethernet5
 C        1.1.1.4/32 is directly connected, Loopback0
 B E      1.1.1.5/32 [200/0] via 194.50.50.1, Ethernet8
 B E      1.1.1.7/32 [200/0] via 194.50.50.1, Ethernet8
 B E      1.1.1.8/32 [200/0] via 194.50.50.1, Ethernet8
 B E      1.1.2.1/32 [200/0] via 172.16.7.1, Ethernet4
                             via 172.16.8.1, Ethernet5
 B E      1.1.2.2/32 [200/0] via 172.16.7.1, Ethernet4
                             via 172.16.8.1, Ethernet5
 C        1.1.2.4/32 is directly connected, Loopback1
 B E      1.1.2.5/32 [200/0] via 194.50.50.1, Ethernet8
 B E      1.1.2.7/32 [200/0] via 194.50.50.1, Ethernet8
 B E      1.1.2.8/32 [200/0] via 194.50.50.1, Ethernet8
 B E      2.2.2.1/32 [200/0] via 172.16.7.1, Ethernet4
 B E      2.2.2.2/32 [200/0] via 172.16.8.1, Ethernet5
 B E      2.2.2.3/32 [200/0] via 194.50.50.1, Ethernet8
 B E      2.2.2.4/32 [200/0] via 194.50.50.1, Ethernet8
 C        3.3.3.1/32 is directly connected, Loopback2
 B E      3.3.3.2/32 [200/0] via 194.50.50.1, Ethernet8
 C        172.16.7.0/31 is directly connected, Ethernet4
 C        172.16.8.0/31 is directly connected, Ethernet5
 C        194.50.50.0/31 is directly connected, Ethernet8


VRF: VRF1
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      10.0.10.2/32 [200/0] via VTEP 1.1.2.5 VNI 999 router-mac 50:01:00:a1:60:83 local-interface Vxlan1
 B E      10.0.10.3/32 [200/0] via VTEP 1.1.2.1 VNI 999 router-mac 50:01:00:26:39:7b local-interface Vxlan1
                               via VTEP 1.1.2.2 VNI 999 router-mac 50:01:00:c7:fa:a0 local-interface Vxlan1
 B E      10.0.20.2/32 [200/0] via VTEP 1.1.2.8 VNI 999 router-mac 50:01:00:9b:56:6c local-interface Vxlan1
 C        10.0.20.0/24 is directly connected, Vlan20

Border-Leaf-1#
Border-Leaf-1#show ip bgp summary 
BGP summary information for VRF default
Router identifier 1.1.1.4, local AS number 65101
Neighbor Status Codes: m - Under maintenance
  Neighbor    V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  2.2.2.1     4 65200            210       205    0    0 01:39:03 Estab   5      5
  2.2.2.2     4 65200            211       196    0    0 01:39:04 Estab   5      5
  3.3.3.2     4 65103            163       177    0    0 01:39:04 Estab   9      9
  172.16.7.1  4 65200            125       125    0    0 01:39:05 Estab   5      5
  172.16.8.1  4 65200            127       125    0    0 01:39:05 Estab   5      5
  194.50.50.1 4 65103            124       123    0    0 01:39:05 Estab   9      9
Border-Leaf-1#
```
- Border-Leaf-2
```
Border-Leaf-2#show vxlan vtep
Remote VTEPS for Vxlan1:

VTEP          Tunnel Type(s)
------------- --------------
1.1.2.1       flood, unicast
1.1.2.2       flood, unicast
1.1.2.8       unicast       

Total number of remote VTEPS:  3
Border-Leaf-2#
Border-Leaf-2#show bgp evpn 
BGP routing table information for VRF default
Router identifier 1.1.1.5, local AS number 65103
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.2:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.2.1:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.2.2:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.4:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65101 i
 * >      RD: 1.1.2.4:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65101 i
 * >      RD: 1.1.1.5:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 -                     -       -       0       i
 * >      RD: 1.1.2.5:1 auto-discovery 0000:0000:0000:0000:5601
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.7:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.7:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.1.8:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.8:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.2.7:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65300 65104 i
 *  ec    RD: 1.1.2.7:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.2.8:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.2.8:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b
                                 1.1.2.8               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b 10.0.20.2
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b 10.0.20.2
                                 1.1.2.8               -       100     0       65300 65104 i
 * >      RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc
                                 1.1.2.1               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.1               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc
                                 -                     -       -       0       i
 * >      RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 -                     -       -       0       i
 * >      RD: 1.1.1.1:10 imet 1.1.2.1
                                 1.1.2.1               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.2:10 imet 1.1.2.2
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.4:20 imet 1.1.2.4
                                 1.1.2.4               -       100     0       65101 i
 * >      RD: 1.1.1.5:10 imet 1.1.2.5
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.7:20 imet 1.1.2.7
                                 1.1.2.7               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.7:20 imet 1.1.2.7
                                 1.1.2.7               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.1.8:20 imet 1.1.2.8
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.8:20 imet 1.1.2.8
                                 1.1.2.8               -       100     0       65300 65104 i
 * >      RD: 1.1.2.1:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.1
                                 1.1.2.1               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.2.2:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.2
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.2.4:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.4
                                 1.1.2.4               -       100     0       65101 i
 * >      RD: 1.1.2.5:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.5
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.2.7:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.7
                                 1.1.2.7               -       100     0       65300 65104 i
 *  ec    RD: 1.1.2.7:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.7
                                 1.1.2.7               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.2.8:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.8
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.2.8:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.8
                                 1.1.2.8               -       100     0       65300 65104 i
Border-Leaf-2#
Border-Leaf-2#show ip route vrf all 

VRF: default
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      1.1.1.1/32 [200/0] via 194.50.50.0, Ethernet8
 B E      1.1.1.2/32 [200/0] via 194.50.50.0, Ethernet8
 B E      1.1.1.4/32 [200/0] via 194.50.50.0, Ethernet8
 C        1.1.1.5/32 is directly connected, Loopback0
 B E      1.1.1.7/32 [200/0] via 172.16.9.1, Ethernet4
                             via 172.16.10.1, Ethernet6
 B E      1.1.1.8/32 [200/0] via 172.16.9.1, Ethernet4
                             via 172.16.10.1, Ethernet6
 B E      1.1.2.1/32 [200/0] via 194.50.50.0, Ethernet8
 B E      1.1.2.2/32 [200/0] via 194.50.50.0, Ethernet8
 B E      1.1.2.4/32 [200/0] via 194.50.50.0, Ethernet8
 C        1.1.2.5/32 is directly connected, Loopback1
 B E      1.1.2.7/32 [200/0] via 172.16.9.1, Ethernet4
                             via 172.16.10.1, Ethernet6
 B E      1.1.2.8/32 [200/0] via 172.16.9.1, Ethernet4
                             via 172.16.10.1, Ethernet6
 B E      2.2.2.1/32 [200/0] via 194.50.50.0, Ethernet8
 B E      2.2.2.2/32 [200/0] via 194.50.50.0, Ethernet8
 B E      2.2.2.3/32 [200/0] via 172.16.9.1, Ethernet4
 B E      2.2.2.4/32 [200/0] via 172.16.10.1, Ethernet6
 B E      3.3.3.1/32 [200/0] via 194.50.50.0, Ethernet8
 C        3.3.3.2/32 is directly connected, Loopback2
 B E      172.16.7.0/31 [200/0] via 194.50.50.0, Ethernet8
 B E      172.16.8.0/31 [200/0] via 194.50.50.0, Ethernet8
 C        172.16.9.0/31 is directly connected, Ethernet4
 C        172.16.10.0/31 is directly connected, Ethernet6
 C        194.50.50.0/31 is directly connected, Ethernet8


VRF: VRF1
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      10.0.10.3/32 [200/0] via VTEP 1.1.2.1 VNI 999 router-mac 50:01:00:26:39:7b local-interface Vxlan1
                               via VTEP 1.1.2.2 VNI 999 router-mac 50:01:00:c7:fa:a0 local-interface Vxlan1
 C        10.0.10.0/24 is directly connected, Vlan10
 B E      10.0.20.2/32 [200/0] via VTEP 1.1.2.8 VNI 999 router-mac 50:01:00:9b:56:6c local-interface Vxlan1

Border-Leaf-2#
Border-Leaf-2#show ip bgp summary 
BGP summary information for VRF default
Router identifier 1.1.1.5, local AS number 65103
Neighbor Status Codes: m - Under maintenance
  Neighbor    V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  2.2.2.3     4 65300          16842     17310    0    0    5d03h Estab   5      5
  2.2.2.4     4 65300          16865     17303    0    0    5d03h Estab   5      5
  3.3.3.1     4 65101           9232      9283    0    0 01:40:57 Estab   12     12
  172.16.9.1  4 65300          15846     15855    0    0    9d08h Estab   5      5
  172.16.10.1 4 65300          15861     15859    0    0    9d08h Estab   5      5
  194.50.50.0 4 65101          15799     15810    0    0 01:40:58 Estab   12     12
Border-Leaf-2#
```

- Border-Leaf-3
```
Border-Leaf-3#show vxlan vtep 
Remote VTEPS for Vxlan1:

VTEP          Tunnel Type(s)
------------- --------------
1.1.2.1       unicast       
1.1.2.2       unicast       
1.1.2.6       unicast       
1.1.2.7       unicast, flood
1.1.2.8       unicast, flood

Total number of remote VTEPS:  5
Border-Leaf-3#
Border-Leaf-3#show bgp evpn 
BGP routing table information for VRF default
Router identifier 1.1.1.3, local AS number 65101
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.1.2:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.2:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.2.1:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.2.1:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.2.2:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.2.2:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65200 65100 i
 * >      RD: 1.1.1.3:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 -                     -       -       0       i
 * >      RD: 1.1.2.3:1 auto-discovery 0000:0000:0000:0000:3401
                                 -                     -       -       0       i
 * >      RD: 1.1.1.6:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65103 i
 * >      RD: 1.1.2.6:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65103 i
 * >      RD: 1.1.1.7:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.1.8:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.2.7:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.2.8:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.1.8:20 mac-ip 5001.0050.554b
                                 1.1.2.8               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.1.8:20 mac-ip 5001.0050.554b 10.0.20.2
                                 1.1.2.8               -       100     0       65103 65300 65104 i
 * >Ec    RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc
                                 1.1.2.2               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.1:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.2               -       100     0       65200 65100 i
 * >      RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.6               -       100     0       65103 i
 * >      RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.6               -       100     0       65103 i
 * >      RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.5
                                 1.1.2.6               -       100     0       65103 i
 * >      RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb
                                 -                     -       -       0       i
 * >      RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.1:10 imet 1.1.2.1
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.1:10 imet 1.1.2.1
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.1.2:10 imet 1.1.2.2
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.1.2:10 imet 1.1.2.2
                                 1.1.2.2               -       100     0       65200 65100 i
 * >      RD: 1.1.1.3:20 imet 1.1.2.3
                                 -                     -       -       0       i
 * >      RD: 1.1.1.6:10 imet 1.1.2.6
                                 1.1.2.6               -       100     0       65103 i
 * >      RD: 1.1.1.7:20 imet 1.1.2.7
                                 1.1.2.7               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.1.8:20 imet 1.1.2.8
                                 1.1.2.8               -       100     0       65103 65300 65104 i
 * >Ec    RD: 1.1.2.1:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.1
                                 1.1.2.1               -       100     0       65200 65100 i
 *  ec    RD: 1.1.2.1:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.1
                                 1.1.2.1               -       100     0       65200 65100 i
 * >Ec    RD: 1.1.2.2:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.2
                                 1.1.2.2               -       100     0       65200 65100 i
 *  ec    RD: 1.1.2.2:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.2
                                 1.1.2.2               -       100     0       65200 65100 i
 * >      RD: 1.1.2.3:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.3
                                 -                     -       -       0       i
 * >      RD: 1.1.2.6:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.6
                                 1.1.2.6               -       100     0       65103 i
 * >      RD: 1.1.2.7:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.7
                                 1.1.2.7               -       100     0       65103 65300 65104 i
 * >      RD: 1.1.2.8:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.8
                                 1.1.2.8               -       100     0       65103 65300 65104 i
Border-Leaf-3#
Border-Leaf-3#show ip route vrf all 

VRF: default
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      1.1.1.1/32 [200/0] via 172.16.6.1, Ethernet6
                             via 172.16.5.1, Ethernet7
 B E      1.1.1.2/32 [200/0] via 172.16.6.1, Ethernet6
                             via 172.16.5.1, Ethernet7
 C        1.1.1.3/32 is directly connected, Loopback0
 B E      1.1.1.6/32 [200/0] via 194.60.60.1, Ethernet1
 B E      1.1.1.7/32 [200/0] via 194.60.60.1, Ethernet1
 B E      1.1.1.8/32 [200/0] via 194.60.60.1, Ethernet1
 B E      1.1.2.1/32 [200/0] via 172.16.6.1, Ethernet6
                             via 172.16.5.1, Ethernet7
 B E      1.1.2.2/32 [200/0] via 172.16.6.1, Ethernet6
                             via 172.16.5.1, Ethernet7
 C        1.1.2.3/32 is directly connected, Loopback1
 B E      1.1.2.6/32 [200/0] via 194.60.60.1, Ethernet1
 B E      1.1.2.7/32 [200/0] via 194.60.60.1, Ethernet1
 B E      1.1.2.8/32 [200/0] via 194.60.60.1, Ethernet1
 B E      2.2.2.1/32 [200/0] via 172.16.5.1, Ethernet7
 B E      2.2.2.2/32 [200/0] via 172.16.6.1, Ethernet6
 B E      2.2.2.3/32 [200/0] via 194.60.60.1, Ethernet1
 B E      2.2.2.4/32 [200/0] via 194.60.60.1, Ethernet1
 C        3.3.3.3/32 is directly connected, Loopback2
 B E      3.3.3.4/32 [200/0] via 194.60.60.1, Ethernet1
 C        172.16.5.0/31 is directly connected, Ethernet7
 C        172.16.6.0/31 is directly connected, Ethernet6
 C        194.60.60.0/31 is directly connected, Ethernet1


VRF: VRF1
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      10.0.10.2/32 [200/0] via VTEP 1.1.2.6 VNI 999 router-mac 50:01:00:ca:7a:8d local-interface Vxlan1
 B E      10.0.10.3/32 [200/0] via VTEP 1.1.2.1 VNI 999 router-mac 50:01:00:26:39:7b local-interface Vxlan1
                               via VTEP 1.1.2.2 VNI 999 router-mac 50:01:00:c7:fa:a0 local-interface Vxlan1
 B E      10.0.10.5/32 [200/0] via VTEP 1.1.2.6 VNI 999 router-mac 50:01:00:ca:7a:8d local-interface Vxlan1
 B E      10.0.20.2/32 [200/0] via VTEP 1.1.2.8 VNI 999 router-mac 50:01:00:9b:56:6c local-interface Vxlan1
 C        10.0.20.0/24 is directly connected, Vlan20

Border-Leaf-3#
Border-Leaf-3#show ip  bgp summary 
BGP summary information for VRF default
Router identifier 1.1.1.3, local AS number 65101
Neighbor Status Codes: m - Under maintenance
  Neighbor    V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  2.2.2.1     4 65200            442       412    0    0 03:14:10 Estab   5      5
  2.2.2.2     4 65200            436       405    0    0 03:14:11 Estab   5      5
  3.3.3.4     4 65103            275       281    0    0 02:49:33 Estab   9      9
  172.16.5.1  4 65200            261       239    0    0 03:14:12 Estab   5      5
  172.16.6.1  4 65200            265       240    0    0 03:14:12 Estab   5      5
  194.60.60.1 4 65103            204       206    0    0 02:49:34 Estab   9      9
Border-Leaf-3#
```

- Border-Leaf-4
```
Border-Leaf-4#show vxlan vtep 
Remote VTEPS for Vxlan1:

VTEP          Tunnel Type(s)
------------- --------------
1.1.2.1       unicast, flood
1.1.2.2       unicast, flood
1.1.2.3       unicast       
1.1.2.8       unicast       

Total number of remote VTEPS:  4
Border-Leaf-4#
Border-Leaf-4#show bgp evpn 
BGP routing table information for VRF default
Router identifier 1.1.1.6, local AS number 65103
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.2:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.2.1:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.2.2:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.3:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65101 i
 * >      RD: 1.1.2.3:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65101 i
 * >      RD: 1.1.1.6:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 -                     -       -       0       i
 * >      RD: 1.1.2.6:1 auto-discovery 0000:0000:0000:0000:5601
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.7:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.7:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.1.8:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.8:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.2.7:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65300 65104 i
 *  ec    RD: 1.1.2.7:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.7               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.2.8:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.2.8:1 auto-discovery 0000:0000:0000:0000:7801
                                 1.1.2.8               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b
                                 1.1.2.8               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b 10.0.20.2
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.8:20 mac-ip 5001.0050.554b 10.0.20.2
                                 1.1.2.8               -       100     0       65300 65104 i
 * >      RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc
                                 -                     -       -       0       i
 * >      RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 -                     -       -       0       i
 * >      RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.5
                                 -                     -       -       0       i
 * >      RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb
                                 1.1.2.3               -       100     0       65101 i
 * >      RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 1.1.2.3               -       100     0       65101 i
 * >      RD: 1.1.1.1:10 imet 1.1.2.1
                                 1.1.2.1               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.2:10 imet 1.1.2.2
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.1.3:20 imet 1.1.2.3
                                 1.1.2.3               -       100     0       65101 i
 * >      RD: 1.1.1.6:10 imet 1.1.2.6
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.7:20 imet 1.1.2.7
                                 1.1.2.7               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.7:20 imet 1.1.2.7
                                 1.1.2.7               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.1.8:20 imet 1.1.2.8
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.1.8:20 imet 1.1.2.8
                                 1.1.2.8               -       100     0       65300 65104 i
 * >      RD: 1.1.2.1:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.1
                                 1.1.2.1               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.2.2:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.2
                                 1.1.2.2               -       100     0       65101 65200 65100 i
 * >      RD: 1.1.2.3:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.3
                                 1.1.2.3               -       100     0       65101 i
 * >      RD: 1.1.2.6:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.6
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.2.7:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.7
                                 1.1.2.7               -       100     0       65300 65104 i
 *  ec    RD: 1.1.2.7:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.7
                                 1.1.2.7               -       100     0       65300 65104 i
 * >Ec    RD: 1.1.2.8:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.8
                                 1.1.2.8               -       100     0       65300 65104 i
 *  ec    RD: 1.1.2.8:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.8
                                 1.1.2.8               -       100     0       65300 65104 i
Border-Leaf-4#
Border-Leaf-4#show ip route vrf all 

VRF: default
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      1.1.1.1/32 [200/0] via 194.60.60.0, Ethernet1
 B E      1.1.1.2/32 [200/0] via 194.60.60.0, Ethernet1
 B E      1.1.1.3/32 [200/0] via 194.60.60.0, Ethernet1
 C        1.1.1.6/32 is directly connected, Loopback0
 B E      1.1.1.7/32 [200/0] via 172.16.12.1, Ethernet3
                             via 172.16.11.1, Ethernet5
 B E      1.1.1.8/32 [200/0] via 172.16.12.1, Ethernet3
                             via 172.16.11.1, Ethernet5
 B E      1.1.2.1/32 [200/0] via 194.60.60.0, Ethernet1
 B E      1.1.2.2/32 [200/0] via 194.60.60.0, Ethernet1
 B E      1.1.2.3/32 [200/0] via 194.60.60.0, Ethernet1
 C        1.1.2.6/32 is directly connected, Loopback1
 B E      1.1.2.7/32 [200/0] via 172.16.12.1, Ethernet3
                             via 172.16.11.1, Ethernet5
 B E      1.1.2.8/32 [200/0] via 172.16.12.1, Ethernet3
                             via 172.16.11.1, Ethernet5
 B E      2.2.2.1/32 [200/0] via 194.60.60.0, Ethernet1
 B E      2.2.2.2/32 [200/0] via 194.60.60.0, Ethernet1
 B E      2.2.2.3/32 [200/0] via 172.16.11.1, Ethernet5
 B E      2.2.2.4/32 [200/0] via 172.16.12.1, Ethernet3
 B E      3.3.3.3/32 [200/0] via 194.60.60.0, Ethernet1
 C        3.3.3.4/32 is directly connected, Loopback2
 C        172.16.11.0/31 is directly connected, Ethernet5
 C        172.16.12.0/31 is directly connected, Ethernet3
 C        194.60.60.0/31 is directly connected, Ethernet1


VRF: VRF1
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      10.0.10.3/32 [200/0] via VTEP 1.1.2.2 VNI 999 router-mac 50:01:00:c7:fa:a0 local-interface Vxlan1
 C        10.0.10.0/24 is directly connected, Vlan10
 B E      10.0.20.2/32 [200/0] via VTEP 1.1.2.8 VNI 999 router-mac 50:01:00:9b:56:6c local-interface Vxlan1
 B E      10.0.20.3/32 [200/0] via VTEP 1.1.2.3 VNI 999 router-mac 50:01:00:ff:ea:cb local-interface Vxlan1

Border-Leaf-4#
```

- Leaf-7 
```
Leaf-7#show vxlan vtep 
Remote VTEPS for Vxlan1:

VTEP          Tunnel Type(s)
------------- --------------
1.1.2.2       unicast       
1.1.2.3       flood, unicast
1.1.2.4       flood, unicast
1.1.2.5       unicast       
1.1.2.6       unicast       

Total number of remote VTEPS:  5
Leaf-7#
Leaf-7#show bgp evpn 
BGP routing table information for VRF default
Router identifier 1.1.1.7, local AS number 65104
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.2:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.2:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.2.1:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.2.1:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.2.2:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.2.2:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.3:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.3:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.4:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.4:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.2.3:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.2.3:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.2.4:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.2.4:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.5:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.5:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.6:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.6:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.2.5:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.2.5:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.2.6:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.2.6:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65300 65103 i
 * >      RD: 1.1.1.7:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 -                     -       -       0       i
 * >      RD: 1.1.2.7:1 auto-discovery 0000:0000:0000:0000:7801
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.6               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.6               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.5
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.5
                                 1.1.2.6               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.1:10 imet 1.1.2.1
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.1:10 imet 1.1.2.1
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.2:10 imet 1.1.2.2
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.2:10 imet 1.1.2.2
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.3:20 imet 1.1.2.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.3:20 imet 1.1.2.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.4:20 imet 1.1.2.4
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.4:20 imet 1.1.2.4
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.5:10 imet 1.1.2.5
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.5:10 imet 1.1.2.5
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.6:10 imet 1.1.2.6
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.6:10 imet 1.1.2.6
                                 1.1.2.6               -       100     0       65300 65103 i
 * >      RD: 1.1.1.7:20 imet 1.1.2.7
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.2.1:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.1
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.2.1:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.1
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.2.2:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.2
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.2.2:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.2
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.2.3:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.2.3:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.2.4:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.4
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.2.4:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.4
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.2.5:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.5
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.2.5:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.5
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.2.6:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.6
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.2.6:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.6
                                 1.1.2.6               -       100     0       65300 65103 i
 * >      RD: 1.1.2.7:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.7
                                 -                     -       -       0       i
Leaf-7#
Leaf-7#show ip route vrf all 

VRF: default
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      1.1.1.1/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      1.1.1.2/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      1.1.1.3/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      1.1.1.4/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      1.1.1.5/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      1.1.1.6/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 C        1.1.1.7/32 is directly connected, Loopback0
 B E      1.1.2.1/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      1.1.2.2/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      1.1.2.3/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      1.1.2.4/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      1.1.2.5/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      1.1.2.6/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 C        1.1.2.7/32 is directly connected, Loopback1
 B E      2.2.2.1/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      2.2.2.2/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      2.2.2.3/32 [200/0] via 172.16.13.1, Ethernet3
 B E      2.2.2.4/32 [200/0] via 172.16.14.1, Ethernet4
 B E      3.3.3.1/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      3.3.3.2/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      3.3.3.3/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      3.3.3.4/32 [200/0] via 172.16.13.1, Ethernet3
                             via 172.16.14.1, Ethernet4
 B E      172.16.7.0/31 [200/0] via 172.16.13.1, Ethernet3
                                via 172.16.14.1, Ethernet4
 B E      172.16.8.0/31 [200/0] via 172.16.13.1, Ethernet3
                                via 172.16.14.1, Ethernet4
 C        172.16.13.0/31 is directly connected, Ethernet3
 C        172.16.14.0/31 is directly connected, Ethernet4
 B E      194.50.50.0/31 [200/0] via 172.16.13.1, Ethernet3
                                 via 172.16.14.1, Ethernet4


VRF: VRF1
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      10.0.10.2/32 [200/0] via VTEP 1.1.2.5 VNI 999 router-mac 50:01:00:a1:60:83 local-interface Vxlan1
                               via VTEP 1.1.2.6 VNI 999 router-mac 50:01:00:ca:7a:8d local-interface Vxlan1
 B E      10.0.10.3/32 [200/0] via VTEP 1.1.2.2 VNI 999 router-mac 50:01:00:c7:fa:a0 local-interface Vxlan1
 B E      10.0.10.5/32 [200/0] via VTEP 1.1.2.6 VNI 999 router-mac 50:01:00:ca:7a:8d local-interface Vxlan1
 B E      10.0.20.3/32 [200/0] via VTEP 1.1.2.3 VNI 999 router-mac 50:01:00:ff:ea:cb local-interface Vxlan1
 C        10.0.20.0/24 is directly connected, Vlan20

Leaf-7#
Leaf-7#show ip bgp summary 
BGP summary information for VRF default
Router identifier 1.1.1.7, local AS number 65104
Neighbor Status Codes: m - Under maintenance
  Neighbor    V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  2.2.2.3     4 65300          17870     17546    0    0    9d10h Estab   22     22
  2.2.2.4     4 65300          17874     17473    0    0    9d10h Estab   22     22
  172.16.13.1 4 65300          16236     16192    0    0    9d12h Estab   22     22
  172.16.14.1 4 65300          16246     16186    0    0    9d10h Estab   22     22
Leaf-7#
```

- Leaf-8
```
Leaf-8#show vxlan vtep 
Remote VTEPS for Vxlan1:

VTEP          Tunnel Type(s)
------------- --------------
1.1.2.2       unicast       
1.1.2.3       flood, unicast
1.1.2.4       flood, unicast
1.1.2.5       unicast       
1.1.2.6       unicast       

Total number of remote VTEPS:  5
Leaf-8#
Leaf-8#show bgp evpn 
BGP routing table information for VRF default
Router identifier 1.1.1.8, local AS number 65104
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.1:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.2:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.2:10 auto-discovery 0 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.2.1:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.2.1:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.2.2:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.2.2:1 auto-discovery 0000:0000:0000:0000:1201
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.3:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.3:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.4:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.4:20 auto-discovery 0 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.2.3:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.2.3:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.2.4:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.2.4:1 auto-discovery 0000:0000:0000:0000:3401
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.5:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.5:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.6:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.6:10 auto-discovery 0 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.2.5:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.2.5:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.2.6:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.2.6:1 auto-discovery 0000:0000:0000:0000:5601
                                 1.1.2.6               -       100     0       65300 65103 i
 * >      RD: 1.1.1.8:20 auto-discovery 0 0000:0000:0000:0000:7801
                                 -                     -       -       0       i
 * >      RD: 1.1.2.8:1 auto-discovery 0000:0000:0000:0000:7801
                                 -                     -       -       0       i
 * >      RD: 1.1.1.8:20 mac-ip 5001.0050.554b
                                 -                     -       -       0       i
 * >      RD: 1.1.1.8:20 mac-ip 5001.0050.554b 10.0.20.2
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.2:10 mac-ip 5001.0069.bdbc 10.0.10.3
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc
                                 1.1.2.6               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.5:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.2
                                 1.1.2.6               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.5
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.6:10 mac-ip 5001.00f1.e6fc 10.0.10.5
                                 1.1.2.6               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.3:20 mac-ip 5001.00ff.ebdb 10.0.20.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.1:10 imet 1.1.2.1
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.1:10 imet 1.1.2.1
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.2:10 imet 1.1.2.2
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.1.2:10 imet 1.1.2.2
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.1.3:20 imet 1.1.2.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.3:20 imet 1.1.2.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.4:20 imet 1.1.2.4
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.1.4:20 imet 1.1.2.4
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.1.5:10 imet 1.1.2.5
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.5:10 imet 1.1.2.5
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.1.6:10 imet 1.1.2.6
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.1.6:10 imet 1.1.2.6
                                 1.1.2.6               -       100     0       65300 65103 i
 * >      RD: 1.1.1.8:20 imet 1.1.2.8
                                 -                     -       -       0       i
 * >Ec    RD: 1.1.2.1:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.1
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.2.1:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.1
                                 1.1.2.1               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.2.2:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.2
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 *  ec    RD: 1.1.2.2:1 ethernet-segment 0000:0000:0000:0000:1201 1.1.2.2
                                 1.1.2.2               -       100     0       65300 65103 65101 65200 65100 i
 * >Ec    RD: 1.1.2.3:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.2.3:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.3
                                 1.1.2.3               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.2.4:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.4
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 *  ec    RD: 1.1.2.4:1 ethernet-segment 0000:0000:0000:0000:3401 1.1.2.4
                                 1.1.2.4               -       100     0       65300 65103 65101 i
 * >Ec    RD: 1.1.2.5:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.5
                                 1.1.2.5               -       100     0       65300 65103 i
 *  ec    RD: 1.1.2.5:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.5
                                 1.1.2.5               -       100     0       65300 65103 i
 * >Ec    RD: 1.1.2.6:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.6
                                 1.1.2.6               -       100     0       65300 65103 i
 *  ec    RD: 1.1.2.6:1 ethernet-segment 0000:0000:0000:0000:5601 1.1.2.6
                                 1.1.2.6               -       100     0       65300 65103 i
 * >      RD: 1.1.2.8:1 ethernet-segment 0000:0000:0000:0000:7801 1.1.2.8
                                 -                     -       -       0       i
Leaf-8#
Leaf-8#show ip route vrf all 

VRF: default
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      1.1.1.1/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      1.1.1.2/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      1.1.1.3/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      1.1.1.4/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      1.1.1.5/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      1.1.1.6/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 C        1.1.1.8/32 is directly connected, Loopback0
 B E      1.1.2.1/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      1.1.2.2/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      1.1.2.3/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      1.1.2.4/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      1.1.2.5/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      1.1.2.6/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 C        1.1.2.8/32 is directly connected, Loopback1
 B E      2.2.2.1/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      2.2.2.2/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      2.2.2.3/32 [200/0] via 172.16.15.1, Ethernet6
 B E      2.2.2.4/32 [200/0] via 172.16.16.1, Ethernet5
 B E      3.3.3.1/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      3.3.3.2/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      3.3.3.3/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      3.3.3.4/32 [200/0] via 172.16.16.1, Ethernet5
                             via 172.16.15.1, Ethernet6
 B E      172.16.7.0/31 [200/0] via 172.16.16.1, Ethernet5
                                via 172.16.15.1, Ethernet6
 B E      172.16.8.0/31 [200/0] via 172.16.16.1, Ethernet5
                                via 172.16.15.1, Ethernet6
 C        172.16.15.0/31 is directly connected, Ethernet6
 C        172.16.16.0/31 is directly connected, Ethernet5
 B E      194.50.50.0/31 [200/0] via 172.16.16.1, Ethernet5
                                 via 172.16.15.1, Ethernet6


VRF: VRF1
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B E      10.0.10.2/32 [200/0] via VTEP 1.1.2.5 VNI 999 router-mac 50:01:00:a1:60:83 local-interface Vxlan1
                               via VTEP 1.1.2.6 VNI 999 router-mac 50:01:00:ca:7a:8d local-interface Vxlan1
 B E      10.0.10.3/32 [200/0] via VTEP 1.1.2.2 VNI 999 router-mac 50:01:00:c7:fa:a0 local-interface Vxlan1
 B E      10.0.10.5/32 [200/0] via VTEP 1.1.2.6 VNI 999 router-mac 50:01:00:ca:7a:8d local-interface Vxlan1
 B E      10.0.20.3/32 [200/0] via VTEP 1.1.2.3 VNI 999 router-mac 50:01:00:ff:ea:cb local-interface Vxlan1
 C        10.0.20.0/24 is directly connected, Vlan20

Leaf-8#
Leaf-8#show ip bgp summary 
BGP summary information for VRF default
Router identifier 1.1.1.8, local AS number 65104
Neighbor Status Codes: m - Under maintenance
  Neighbor    V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  2.2.2.3     4 65300          17828     17535    0    0    9d10h Estab   22     22
  2.2.2.4     4 65300          17820     17418    0    0    9d10h Estab   22     22
  172.16.15.1 4 65300          16214     16168    0    0    9d11h Estab   22     22
  172.16.16.1 4 65300          16217     16171    0    0    9d11h Estab   22     22
Leaf-8#
```

## Выборочная проверка доступности хостов между собой

- Server-1
```
SERVER-1#show ip interface brief
                                                                        Address
Interface        IP Address        Status      Protocol          MTU    Owner  
---------------- ----------------- ----------- ------------- ---------- -------
Management1      unassigned        up          up               1500           
Vlan10           10.0.10.3/24      up          up               1500           

SERVER-1#
SERVER-1#ping 10.0.10.2
PING 10.0.10.2 (10.0.10.2) 72(100) bytes of data.
80 bytes from 10.0.10.2: icmp_seq=1 ttl=64 time=78.6 ms
80 bytes from 10.0.10.2: icmp_seq=2 ttl=64 time=69.0 ms
80 bytes from 10.0.10.2: icmp_seq=3 ttl=64 time=61.5 ms
80 bytes from 10.0.10.2: icmp_seq=4 ttl=64 time=53.8 ms
80 bytes from 10.0.10.2: icmp_seq=5 ttl=64 time=46.0 ms

--- 10.0.10.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 43ms
rtt min/avg/max/mdev = 46.099/61.825/78.634/11.365 ms, pipe 5, ipg/ewma 10.847/69.414 ms
SERVER-1#
SERVER-1#ping 10.0.20.2
PING 10.0.20.2 (10.0.20.2) 72(100) bytes of data.
80 bytes from 10.0.20.2: icmp_seq=1 ttl=62 time=58.1 ms
80 bytes from 10.0.20.2: icmp_seq=2 ttl=62 time=48.2 ms
80 bytes from 10.0.20.2: icmp_seq=3 ttl=62 time=40.9 ms
80 bytes from 10.0.20.2: icmp_seq=4 ttl=62 time=35.9 ms
80 bytes from 10.0.20.2: icmp_seq=5 ttl=62 time=41.3 ms

--- 10.0.20.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 44ms
rtt min/avg/max/mdev = 35.907/44.923/58.151/7.687 ms, pipe 5, ipg/ewma 11.124/51.145 ms
SERVER-1#
SERVER-1#ping 10.0.20.3
PING 10.0.20.3 (10.0.20.3) 72(100) bytes of data.
80 bytes from 10.0.20.3: icmp_seq=1 ttl=62 time=38.2 ms
80 bytes from 10.0.20.3: icmp_seq=2 ttl=62 time=28.5 ms
80 bytes from 10.0.20.3: icmp_seq=3 ttl=62 time=21.0 ms
80 bytes from 10.0.20.3: icmp_seq=4 ttl=62 time=19.7 ms
80 bytes from 10.0.20.3: icmp_seq=5 ttl=62 time=23.1 ms

--- 10.0.20.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 66ms
rtt min/avg/max/mdev = 19.727/26.143/38.236/6.757 ms, pipe 4, ipg/ewma 16.543/31.871 ms
SERVER-1#
```

-Server-4
```
SERVER-4#ping 10.0.20.3
PING 10.0.20.3 (10.0.20.3) 72(100) bytes of data.
80 bytes from 10.0.20.3: icmp_seq=1 ttl=64 time=87.5 ms
80 bytes from 10.0.20.3: icmp_seq=2 ttl=64 time=77.6 ms
80 bytes from 10.0.20.3: icmp_seq=3 ttl=64 time=71.2 ms
80 bytes from 10.0.20.3: icmp_seq=4 ttl=64 time=64.7 ms
80 bytes from 10.0.20.3: icmp_seq=5 ttl=64 time=57.3 ms

--- 10.0.20.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 44ms
rtt min/avg/max/mdev = 57.344/71.720/87.540/10.395 ms, pipe 5, ipg/ewma 11.103/78.892 ms
SERVER-4#
SERVER-4#ping 10.0.10.2
PING 10.0.10.2 (10.0.10.2) 72(100) bytes of data.
80 bytes from 10.0.10.2: icmp_seq=1 ttl=62 time=93.7 ms
80 bytes from 10.0.10.2: icmp_seq=2 ttl=62 time=191 ms
80 bytes from 10.0.10.2: icmp_seq=3 ttl=62 time=220 ms
80 bytes from 10.0.10.2: icmp_seq=4 ttl=62 time=216 ms
80 bytes from 10.0.10.2: icmp_seq=5 ttl=62 time=211 ms

--- 10.0.10.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 44ms
rtt min/avg/max/mdev = 93.722/186.674/220.508/47.553 ms, pipe 5, ipg/ewma 11.068/142.171 ms
SERVER-4#
SERVER-4#ping 10.0.10.3
PING 10.0.10.3 (10.0.10.3) 72(100) bytes of data.
80 bytes from 10.0.10.3: icmp_seq=1 ttl=62 time=96.8 ms
80 bytes from 10.0.10.3: icmp_seq=2 ttl=62 time=86.6 ms
80 bytes from 10.0.10.3: icmp_seq=3 ttl=62 time=79.5 ms
80 bytes from 10.0.10.3: icmp_seq=4 ttl=62 time=71.8 ms
80 bytes from 10.0.10.3: icmp_seq=5 ttl=62 time=64.7 ms

--- 10.0.10.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 43ms
rtt min/avg/max/mdev = 64.702/79.904/96.811/11.205 ms, pipe 5, ipg/ewma 10.864/87.560 ms
SERVER-4#```
```