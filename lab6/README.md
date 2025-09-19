### Настроить маршрутизацию (ассиметричная) в рамках Overlay между клиентами.

### Цели: 

- Part 1: Настроите каждого клиента в своем VNI.
- Part 2: Настроите маршрутизацию между клиентами.
- Part 3: Зафиксируете в документации - план работы, адресное пространство, схему сети, конфигурацию устройств


###  1 Настроите BGP peering между Leaf и Spine в AF l2vpn evpn.

### Настройка Leaf-1

```
Leaf-1#show running-config section bgp
router bgp 65100
   router-id 1.1.1.1
   maximum-paths 3 ecmp 3
   neighbor OVERLAY peer group
   neighbor OVERLAY remote-as 65200
   neighbor OVERLAY update-source Loopback0
   neighbor OVERLAY ebgp-multihop 10
   neighbor OVERLAY send-community extended
   neighbor UNDERLAY peer group
   neighbor UNDERLAY remote-as 65200
   neighbor UNDERLAY bfd
   neighbor 2.2.2.1 peer group OVERLAY
   neighbor 2.2.2.2 peer group OVERLAY
   neighbor 2.2.2.3 peer group OVERLAY
   neighbor 172.16.1.1 peer group UNDERLAY
   no neighbor 172.16.1.1 route-map in
   no neighbor 172.16.1.1 route-map out
   neighbor 172.16.2.1 peer group UNDERLAY
   neighbor 172.16.3.1 peer group UNDERLAY
   redistribute connected route-map ADVERT_NET
   !
   vlan 100
      rd auto
      route-target both 100:100100
      redistribute learned
   !
   vlan 200
      rd auto
      route-target both 100:100200
      redistribute learned
   !
   address-family evpn
      neighbor OVERLAY activate
Leaf-1#
```

### Настройка Leaf-2

```
Leaf-2#show running-config section bgp
router bgp 65101
   router-id 1.1.1.2
   maximum-paths 3 ecmp 3
   neighbor OVERLAY peer group
   neighbor OVERLAY remote-as 65200
   no neighbor OVERLAY next-hop-unchanged
   neighbor OVERLAY update-source Loopback0
   neighbor OVERLAY ebgp-multihop 10
   neighbor OVERLAY send-community extended
   neighbor UNDERLAY peer group
   neighbor UNDERLAY remote-as 65200
   neighbor UNDERLAY bfd
   neighbor 2.2.2.1 peer group OVERLAY
   neighbor 2.2.2.2 peer group OVERLAY
   neighbor 2.2.2.3 peer group OVERLAY
   neighbor 172.16.4.1 peer group UNDERLAY
   neighbor 172.16.5.1 peer group UNDERLAY
   neighbor 172.16.6.1 peer group UNDERLAY
   redistribute connected route-map ADVERT_NET
   !
   vlan 100
      rd auto
      route-target both 100:100100
      redistribute learned
   !
   vlan 200
      rd auto
      route-target both 100:100200
      redistribute learned
   !
   address-family evpn
      neighbor OVERLAY activate
Leaf-2#
```


### Настройка Spine-1

```
Spine-1#show running-config section bgp
ip as-path access-list RM_BGP permit ^651([0-4][0-9]|50|00)$ any
router bgp 65200
   router-id 2.2.2.1
   bgp listen range 1.1.0.0/16 peer-group OVERLAY peer-filter ASN_LEAFS
   bgp listen range 172.16.0.0/20 peer-group UNDERLAY peer-filter ASN_LEAFS
   neighbor OVERLAY peer group
   neighbor OVERLAY next-hop-unchanged
   neighbor OVERLAY update-source Loopback0
   neighbor OVERLAY ebgp-multihop 10
   neighbor OVERLAY send-community extended
   neighbor UNDERLAY peer group
   neighbor UNDERLAY bfd
   !
   address-family evpn
      neighbor OVERLAY activate
   !
   address-family ipv4
      network 2.2.2.1/32
Spine-1#
```

### Настройка Spine-2

```
Spine-2#show running-config section bgp
router bgp 65200
   router-id 2.2.2.2
   bgp listen range 1.1.0.0/16 peer-group OVERLAY peer-filter ASN_LEAFS
   bgp listen range 172.16.0.0/20 peer-group UNDERLAY peer-filter ASN_LEAFS
   neighbor OVERLAY peer group
   neighbor OVERLAY next-hop-unchanged
   neighbor OVERLAY update-source Loopback0
   neighbor OVERLAY ebgp-multihop 10
   neighbor OVERLAY send-community extended
   neighbor UNDERLAY peer group
   neighbor UNDERLAY bfd
   !
   address-family evpn
      neighbor OVERLAY activate
   !
   address-family ipv4
      network 2.2.2.2/32
Spine-2#
```

### Настройка Spine-3

```
Spine-3#show running-config section bgp
router bgp 65200
   router-id 2.2.2.3
   bgp listen range 1.1.0.0/16 peer-group OVERLAY peer-filter ASN_LEAFS
   bgp listen range 172.16.0.0/20 peer-group UNDERLAY peer-filter ASN_LEAFS
   neighbor OVERLAY peer group
   neighbor OVERLAY next-hop-unchanged
   neighbor OVERLAY update-source Loopback0
   neighbor OVERLAY ebgp-multihop 10
   neighbor OVERLAY send-community extended
   neighbor UNDERLAY peer group
   neighbor UNDERLAY bfd
   !
   address-family evpn
      neighbor OVERLAY activate
   !
   address-family ipv4
      network 2.2.2.3/32
Spine-3#
```


### 2 Зафиксируете в документации - план работы, адресное пространство, схему сети, конфигурацию устройств:



### Планы работ:
- 1:  Построить схему "VxLAN EVPN L3"  
- 2:  Зафиксировать адресное пространство
- 3:  Проверить маршрутизацию между разными VNI
- 4:  Прикрепить конфиги устройств
### 1 Зафиксированное адресное пространство

### PTP link

|IP subnet|Subnet Mask|Description
|---|---|---|
172.16.1.0|255.255.255.254|Leaf1-Spine1
172.16.2.0|255.255.255.254|Leaf1-Spine2
172.16.3.0|255.255.255.254|Leaf1-Spine3
172.16.4.0|255.255.255.254|Leaf2-Spine1
172.16.5.0|255.255.255.254|Leaf2-Spine2
172.16.6.0|255.255.255.254|Leaf2-Spine3





### Loopback link

|Device|IP Address|Subnet Mask
|---|---|---|
Leaf-1|1.1.1.1|255.255.255.255
Leaf-1|1.1.2.1|255.255.255.255
Leaf-2|1.1.1.2|255.255.255.255
Leaf-2|1.1.2.2|255.255.255.255
Spine-1|2.2.2.1|255.255.255.255
Spine-2|2.2.2.2|255.255.255.255
Spine-3|2.2.2.3|255.255.255.255



### Схема "VxLAN EVPN L3"

![scheme](scheme.png)
check.png

### 3 Проверить маршрутизацию между разными VNI

![check](check.png)

Конфиги устройств, прикладываю в отдельной папке







