# Домашние задание 2
## Построение Underlay сети(OSPF)

> # Spine-1  
```
feature ospf

interface Ethernet1/1
  description to leaf-1
  no switchport
  no ip redirects
  ip address 10.2.1.0/31
  ip router ospf UNDELAY area 0.0.0.0
  no shutdown

interface Ethernet1/2
  description to leaf-2
  no switchport
  no ip redirects
  ip address 10.2.1.2/31
  ip router ospf UNDELAY area 0.0.0.0
  no shutdown

interface Ethernet1/3
  no switchport
  no ip redirects
  ip address 10.2.1.4/31
  ip router ospf UNDELAY area 0.0.0.0
  no shutdown

interface loopback1
  ip address 10.0.1.0/32
  ip router ospf UNDELAY area 0.0.0.0

interface loopback2
  ip address 10.1.1.0/32

router ospf UNDELAY
  router-id 10.0.1.0
```
### Вывод маршрутной информации
```
10.0.0.1/32, ubest/mbest: 1/0
    *via 10.2.1.1, Eth1/1, [110/41], 01:24:04, ospf-UNDELAY, intra
10.0.0.2/32, ubest/mbest: 1/0
    *via 10.2.1.3, Eth1/2, [110/41], 01:24:03, ospf-UNDELAY, intra
10.0.0.3/32, ubest/mbest: 1/0
    *via 10.2.1.5, Eth1/3, [110/41], 01:19:37, ospf-UNDELAY, intra
10.0.1.0/32, ubest/mbest: 2/0, attached
    *via 10.0.1.0, Lo1, [0/0], 01:52:43, local
    *via 10.0.1.0, Lo1, [0/0], 01:52:43, direct
10.0.2.0/32, ubest/mbest: 3/0
    *via 10.2.1.1, Eth1/1, [110/81], 01:24:03, ospf-UNDELAY, intra
    *via 10.2.1.3, Eth1/2, [110/81], 01:23:57, ospf-UNDELAY, intra
    *via 10.2.1.5, Eth1/3, [110/81], 01:19:37, ospf-UNDELAY, intra
10.1.1.0/32, ubest/mbest: 2/0, attached
    *via 10.1.1.0, Lo2, [0/0], 01:52:43, local
    *via 10.1.1.0, Lo2, [0/0], 01:52:43, direct
10.2.1.0/31, ubest/mbest: 1/0, attached
    *via 10.2.1.0, Eth1/1, [0/0], 01:52:44, direct
10.2.1.0/32, ubest/mbest: 1/0, attached
    *via 10.2.1.0, Eth1/1, [0/0], 01:52:44, local
10.2.1.2/31, ubest/mbest: 1/0, attached
    *via 10.2.1.2, Eth1/2, [0/0], 01:52:44, direct
10.2.1.2/32, ubest/mbest: 1/0, attached
    *via 10.2.1.2, Eth1/2, [0/0], 01:52:44, local
10.2.1.4/31, ubest/mbest: 1/0, attached
    *via 10.2.1.4, Eth1/3, [0/0], 01:52:44, direct
10.2.1.4/32, ubest/mbest: 1/0, attached
    *via 10.2.1.4, Eth1/3, [0/0], 01:52:44, local
10.2.2.0/31, ubest/mbest: 1/0
    *via 10.2.1.1, Eth1/1, [110/80], 01:24:04, ospf-UNDELAY, intra
10.2.2.2/31, ubest/mbest: 1/0
    *via 10.2.1.3, Eth1/2, [110/80], 01:24:03, ospf-UNDELAY, intra
10.2.2.4/31, ubest/mbest: 1/0
    *via 10.2.1.5, Eth1/3, [110/80], 01:19:37, ospf-UNDELAY, intra
```


> # Spine-2 
```
feature ospf

interface Ethernet1/1
  description to leaf-1
  no switchport
  no ip redirects
  ip address 10.2.2.0/31
  ip router ospf UNDELAY area 0.0.0.0
  no shutdown

interface Ethernet1/2
  description to leaf-2
  no switchport
  no ip redirects
  ip address 10.2.2.2/31
  ip router ospf UNDELAY area 0.0.0.0
  no shutdown

interface Ethernet1/3
  no switchport
  no ip redirects
  ip address 10.2.2.4/31
  ip router ospf UNDELAY area 0.0.0.0
  no shutdown

interface loopback1
  ip address 10.0.2.0/32
  ip router ospf UNDELAY area 0.0.0.0

interface loopback2
  ip address 10.1.2.0/32

router ospf UNDELAY
  router-id 10.0.2.0
```
### Вывод маршрутной информации
```
10.0.0.1/32, ubest/mbest: 1/0
    *via 10.2.2.1, Eth1/1, [110/41], 01:29:27, ospf-UNDELAY, intra
10.0.0.2/32, ubest/mbest: 1/0
    *via 10.2.2.3, Eth1/2, [110/41], 01:29:21, ospf-UNDELAY, intra
10.0.0.3/32, ubest/mbest: 1/0
    *via 10.2.2.5, Eth1/3, [110/41], 01:25:00, ospf-UNDELAY, intra
10.0.1.0/32, ubest/mbest: 3/0
    *via 10.2.2.1, Eth1/1, [110/81], 01:29:27, ospf-UNDELAY, intra
    *via 10.2.2.3, Eth1/2, [110/81], 01:29:21, ospf-UNDELAY, intra
    *via 10.2.2.5, Eth1/3, [110/81], 01:25:00, ospf-UNDELAY, intra
10.0.2.0/32, ubest/mbest: 2/0, attached
    *via 10.0.2.0, Lo1, [0/0], 01:57:59, local
    *via 10.0.2.0, Lo1, [0/0], 01:57:59, direct
10.1.2.0/32, ubest/mbest: 2/0, attached
    *via 10.1.2.0, Lo2, [0/0], 01:57:59, local
    *via 10.1.2.0, Lo2, [0/0], 01:57:59, direct
10.2.1.0/31, ubest/mbest: 1/0
    *via 10.2.2.1, Eth1/1, [110/80], 01:29:27, ospf-UNDELAY, intra
10.2.1.2/31, ubest/mbest: 1/0
    *via 10.2.2.3, Eth1/2, [110/80], 01:29:21, ospf-UNDELAY, intra
10.2.1.4/31, ubest/mbest: 1/0
    *via 10.2.2.5, Eth1/3, [110/80], 01:25:00, ospf-UNDELAY, intra
10.2.2.0/31, ubest/mbest: 1/0, attached
    *via 10.2.2.0, Eth1/1, [0/0], 01:58:01, direct
10.2.2.0/32, ubest/mbest: 1/0, attached
    *via 10.2.2.0, Eth1/1, [0/0], 01:58:01, local
10.2.2.2/31, ubest/mbest: 1/0, attached
    *via 10.2.2.2, Eth1/2, [0/0], 01:58:01, direct
10.2.2.2/32, ubest/mbest: 1/0, attached
    *via 10.2.2.2, Eth1/2, [0/0], 01:58:01, local
10.2.2.4/31, ubest/mbest: 1/0, attached
    *via 10.2.2.4, Eth1/3, [0/0], 01:58:00, direct
10.2.2.4/32, ubest/mbest: 1/0, attached
    *via 10.2.2.4, Eth1/3, [0/0], 01:58:00, local
```
