 # VxLAN. Routing.



Цель:

Реализовать передачу суммарных префиксов через EVPN route-type 5.



-1: Разместите двух "клиентов" в разных VRF в рамках одной фабрики.

-2: Настроите маршрутизацию между клиентами через внешнее устройство (граничный роутер  фаерволл  etc)

-3: Зафиксируете в документации - план работы, адресное пространство, схему сети, настройки сетевого оборудования

## Схема сети:

! [](Scheme.png)



 ## Конфигурации устройств:



 -  [spine-1](Config/spine-1.cfg)



 -  [spine-2](Config/spine-2.cfg)



 -  [leaf-1](Config/leaf-1.cfg)



 -  [leaf-2](Config/leaf-2.cfg)



 -  [border-leaf](Config/border-leaf.cfg)



 -  [firewall](Config/firewall.cfg)



 -  [server-1](Config/server-1.cfg)



 -  [server-2](Config/server-2.cfg)



 ## 1 Зафиксированное адресное пространство



 ### PTP link

|IP subnet|Subnet Mask|Description
|---|---|---|
172.16.1.0|255.255.255.254|Leaf1-Spine1
172.16.2.0|255.255.255.254|Leaf1-Spine2
172.16.3.0|255.255.255.254|Leaf2-Spine1
172.16.4.0|255.255.255.254|Leaf2-Spine2
172.16.5.0|255.255.255.254|Border _Leaf-Spine1
172.16.6.0|255.255.255.254|Border _Leaf-Spine2
172.16.7.0|255.255.255.254|Border _Leaf-Firewall
172.16.8.0|255.255.255.254|Border _Leaf-Firewall





 ### Loopback link

|Device|IP Address|Subnet Mask
|---|---|---|
Leaf-1|1.1.1.1|255.255.255.255
Leaf-1|1.1.2.1|255.255.255.255
Leaf-2|1.1.1.2|255.255.255.255
Leaf-2|1.1.2.2|255.255.255.255
Border-Leaf|1.1.1.3|255.255.255.255
Border-Leaf|1.1.2.3|255.255.255.255
Spine-1|2.2.2.1|255.255.255.255
Spine-2|2.2.2.2|255.255.255.255





 ### VLAN-10

|Device|IP Address|Subnet Mask
|---|---|---|
SERVER-1|10.10.1.2|255.255.255.0
Gateway|10.10.1.1|255.255.255.0







 ### VLAN-20

|Device|IP Address|Subnet Mask
|---|---|---|
SERVER-2|10.20.1.2|255.255.255.0
Gateway|10.20.1.1|255.255.255.0







 ## Разместите двух "клиентов" в разных VRF в рамках одной фабрики



* Border-Leaf:



```
Border-Leaf#show ip interface vrf Office-1 brief

 &nbsp;                                                                       Address

Interface      IP Address         Status      Protocol           MTU    Owner

-------------- ------------------ ----------- -------------- ---------- -------

Vlan2          172.16.7.0/31      up          up                1500

Vlan10         10.10.1.1/24       up          up                1500

Vlan4094       unassigned         up          up                9164



Border-Leaf#show ip interface vrf Office-2 brief

 &nbsp;                                                                       Address

Interface      IP Address         Status      Protocol           MTU    Owner

-------------- ------------------ ----------- -------------- ---------- -------

Vlan3          172.16.8.0/31      up          up                1500

Vlan20         10.20.1.1/24       up          up                1500

Vlan4093       unassigned         up          up                9164



Border-Leaf#show ip route vrf Office-1



VRF: Office-1

Codes: C - connected, S - static, K - kernel,

 &nbsp;      O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,

 &nbsp;      E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,

 &nbsp;      N2 - OSPF NSSA external type2, B - Other BGP Routes,

 &nbsp;      B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,

 &nbsp;      I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,

 &nbsp;      A O - OSPF Summary, NG - Nexthop Group Static Route,

 &nbsp;      V - VXLAN Control Service, M - Martian,

 &nbsp;      DH - DHCP client installed default route,

 &nbsp;      DP - Dynamic Policy Route, L - VRF Leaked,

 &nbsp;      G  - gRIBI, RC - Route Cache Route



Gateway of last resort is not set



 &nbsp;C        10.10.1.0/24 is directly connected, Vlan10

 &nbsp;A B      10.10.0.0/16 is directly connected, Null0

 &nbsp;S        10.20.0.0/16    [1/0] via 172.16.7.1, Vlan2

 &nbsp;C        172.16.7.0/31 is directly connected, Vlan2



Border-Leaf#show ip route vrf Office-2



VRF: Office-2

Codes: C - connected, S - static, K - kernel,

 &nbsp;      O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,

 &nbsp;      E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,

 &nbsp;      N2 - OSPF NSSA external type2, B - Other BGP Routes,

 &nbsp;      B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,

 &nbsp;      I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,

 &nbsp;      A O - OSPF Summary, NG - Nexthop Group Static Route,

 &nbsp;      V - VXLAN Control Service, M - Martian,

 &nbsp;      DH - DHCP client installed default route,

 &nbsp;      DP - Dynamic Policy Route, L - VRF Leaked,

 &nbsp;      G  - gRIBI, RC - Route Cache Route



Gateway of last resort is not set



 &nbsp;S        10.10.0.0/16    [1/0] via 172.16.8.1, Vlan3

 &nbsp;C        10.20.1.0/24 is directly connected, Vlan20

 &nbsp;A B      10.20.0.0/16 is directly connected, Null0

 &nbsp;C        172.16.8.0/31 is directly connected, Vlan3

```





 ## Настроите маршрутизацию между клиентами через внешнее устройство

* Firewall:



```

Firewall#show ip route



VRF: default

Codes: C - connected, S - static, K - kernel,

 &nbsp;      O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,

 &nbsp;      E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,

 &nbsp;      N2 - OSPF NSSA external type2, B - Other BGP Routes,

 &nbsp;      B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,

 &nbsp;      I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,

 &nbsp;      A O - OSPF Summary, NG - Nexthop Group Static Route,

 &nbsp;      V - VXLAN Control Service, M - Martian,

 &nbsp;      DH - DHCP client installed default route,

 &nbsp;      DP - Dynamic Policy Route, L - VRF Leaked,

 &nbsp;      G  - gRIBI, RC - Route Cache Route



Gateway of last resort is not set



 &nbsp;S        10.10.0.0/16    [1/0] via 172.16.7.0, Vlan2

 &nbsp;S        10.20.0.0/16    [1/0] via 172.16.8.0, Vlan3

 &nbsp;C        172.16.7.0/31 is directly connected, Vlan2

 &nbsp;C        172.16.8.0/31 is directly connected, Vlan3



Firewall#

```

* Leaf-1:



```
Leaf-1#show bgp evpn route-type ip-prefix ipv4

BGP routing table information for VRF default

Router identifier 1.1.1.1, local AS number 65200

Route status codes:    * - valid, > - active, S - Stale, E - ECMP head, e - ECMP

 &nbsp;                   c - Contributing to ECMP, % - Pending BGP convergence

Origin codes: i - IGP, e - EGP, ? - incomplete

AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop



 &nbsp;         Network                Next Hop              Metric  LocPref Weight  Path

 &nbsp;   * >Ec    RD: 65200:10010 ip-prefix 10.10.0.0/16

 &nbsp;                                1.1.2.3               -       100     0       65100 65400 i

 &nbsp;   *  ec    RD: 65200:10010 ip-prefix 10.10.0.0/16

 &nbsp;                                1.1.2.3               -       100     0       65100 65400 i

 &nbsp;   * >Ec    RD: 65300:10020 ip-prefix 10.20.0.0/16

 &nbsp;                                1.1.2.3               -       100     0       65100 65400 i

 &nbsp;   *  ec    RD: 65300:10020 ip-prefix 10.20.0.0/16

 &nbsp;                                1.1.2.3               -       100     0       65100 65400 i

 &nbsp;   * >Ec    RD: 65200:10010 ip-prefix 172.16.7.0/31

 &nbsp;                                1.1.2.3               -       100     0       65100 65400 i

 &nbsp;   *  ec    RD: 65200:10010 ip-prefix 172.16.7.0/31

 &nbsp;                                1.1.2.3               -       100     0       65100 65400 i

 &nbsp;   * >Ec    RD: 65300:10020 ip-prefix 172.16.8.0/31

 &nbsp;                                1.1.2.3               -       100     0       65100 65400 i

 &nbsp;   *  ec    RD: 65300:10020 ip-prefix 172.16.8.0/31

 &nbsp;                                1.1.2.3               -       100     0       65100 65400 i

Leaf-1#

```

## Выборочная проверка доступности хостов между собой:



* Server-1:



```
Server-1#show ip interface brief

&nbsp;                                                                       Address

Interface        IP Address        Status      Protocol          MTU    Owner

---------------- ----------------- ----------- ------------- ---------- -------

Management1      unassigned        up          up               1500

Vlan10           10.10.1.2/24      up          up               1500



Server-1#

Server-1#ping 10.20.1.2

PING 10.20.1.2 (10.20.1.2) 72(100) bytes of data.

80 bytes from 10.20.1.2: icmp _seq=1 ttl=61 time=61.1 ms

80 bytes from 10.20.1.2: icmp _seq=2 ttl=61 time=65.9 ms

80 bytes from 10.20.1.2: icmp _seq=3 ttl=61 time=60.7 ms

80 bytes from 10.20.1.2: icmp _seq=4 ttl=61 time=53.8 ms

80 bytes from 10.20.1.2: icmp _seq=5 ttl=61 time=63.6 ms



--- 10.20.1.2 ping statistics ---

5 packets transmitted, 5 received, 0% packet loss, time 45ms

rtt min/avg/max/mdev = 53.879/61.077/65.965/4.060 ms, pipe 5, ipg/ewma 11.298/61.028 ms

Server-1#

```









